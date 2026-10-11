+++
date = '2026-08-28T20:27:00+08:00'
draft = false
title = 'LeetCodeBot: One Session, Many Commands'
categories = ['Debugging']
tags = ['python', 'sqlalchemy', 'asyncio', 'testing']
series = ['LeetCodeBot']
+++

## Context

[LeetCodeBot](https://github.com/xDDoubleTea/LeetCodeBot) talks to its database through `AsyncDatabaseManager`, an async context manager that hands out SQLAlchemy sessions:

```python
async with self.async_database_manager as db:
    db.add(row)
# leaving the block commits, or rolls back on error
```

`main.py` creates **one** manager and shares it across every cog. The old version looked like this:

```python
async def __aenter__(self) -> AsyncSession:
    self.session = self._sessionmaker()
    return self.session

async def __aexit__(self, exc_type, exc_val, exc_tb):
    try:
        assert self.session
        if exc_type:
            await self.session.rollback()
        else:
            await self.session.commit()
        await self.session.close()
    except AssertionError:
        return True
    finally:
        return False
```

It has two bugs ([#38](https://github.com/xDDoubleTea/LeetCodeBot/issues/38)).

## Bug 1: `__aexit__` commits the wrong session

`__aenter__` returns a new session, so the caller's `db` is bound to that object. But `__aexit__` reads `self.session`, which belongs to the shared manager.

Anything that reassigns `self.session` between enter and exit breaks this:

- **Another command running concurrently.** discord.py runs each command in its own task.
- **A nested `async with`** inside the same command.

The outer block then commits and closes someone else's session, and its own is silently dropped.

Simulating ten concurrent commands that each add one row: **1 row of 10 was committed.** The other nine sessions were never flushed and kept their connections checked out.

The bot already had two nested call sites:

- `get_thread_by_problem_id`: read-only, but the outer session is never closed. A leaked connection.
- `create_thread_in_db`: the row is staged on the outer session while the inner one is committed. **The row is lost.**

Like the [MissingGreenlet bug]({{< ref "leetcodebot-missing-greenlet" >}}), both sit behind cache checks. A warm bot skips the inner `async with`, so the bug stayed hidden.

## Bug 2: `return` inside `finally`

```python
finally:
    return False
```

A `return` in `finally` discards any exception in flight:

- A failing `commit()`, for example an `IntegrityError` from a unique constraint, is swallowed. The caller is told the write succeeded.
- `close()` is skipped on that path, leaking the connection.
- The `except AssertionError: return True` branch is dead code: `finally` overrides its return value.

One correction to my own issue: an exception raised in the `async with` _body_ still reaches the caller, because returning `False` from `__aexit__` means "don't suppress". Only exceptions raised _inside_ `__aexit__` are lost.

## The fix: a `ContextVar` stack

The session moves off the shared instance into a module-level `ContextVar` ([#46](https://github.com/xDDoubleTea/LeetCodeBot/pull/46)).

Each `asyncio.Task` copies the context when it starts, so one command's `set()` is invisible to every other. A small demo, with four tasks writing both a global and a `ContextVar`:

```
task-0: shared='task-3'     ctx='task-0'
task-1: shared='task-3'     ctx='task-1'
task-2: shared='task-3'     ctx='task-2'
task-3: shared='task-3'     ctx='task-3'
```

The global ends up as whoever wrote last. The `ContextVar` keeps each task's own value.

A single value is still not enough, because nesting happens **inside one task**:

```
Single value (nested in ONE task):
    exiting inner: single.get() -> 'inner'
    exiting outer: single.get() -> 'inner'    <- wrong session

Stack (nested in ONE task):
    exiting inner: popped -> 'inner'
    exiting outer: popped -> 'outer'
```

So the `ContextVar` holds a tuple used as a stack:

```python
_session_stack: ContextVar[tuple[AsyncSession, ...]] = ContextVar(
    "_session_stack", default=()
)

async def __aenter__(self) -> AsyncSession:
    session = self._sessionmaker()
    _session_stack.set((*_session_stack.get(), session))
    return session

async def __aexit__(self, exc_type, exc_val, exc_tb) -> bool:
    stack = _session_stack.get()
    if not stack:
        return False
    session = stack[-1]
    _session_stack.set(stack[:-1])
    try:
        if exc_type:
            await session.rollback()
        else:
            await session.commit()
    finally:
        await session.close()
    return False
```

`close()` is now in `finally`, and `return False` is outside it, so commit errors reach the caller. None of the 17 call sites needed to change.

## Tests that could never fail

Two of the obvious tests **passed against the buggy code**. I only found out by reverting the fix and checking which tests went red.

**Concurrency test.** Run ten tasks and assert they got ten distinct sessions. It passes on the old code: `__aenter__` really does hand out a fresh session each time. The bug is in which session `__aexit__` commits. Asserting on **committed rows** catches it: `assert 1 == 10`.

**Nesting test.** Open an outer and an inner block, then check the outer session still works. It does, so the test passes on the old code. To catch the bug, the test has to write to the outer session _after_ the inner block closes, then check the row survived:

```python
async with database_manager as outer:
    async with database_manager as inner:
        assert inner is not outer
    outer.add(GuildForumChannel(...))   # the write create_thread_in_db loses

async with database_manager as session:
    assert await session.scalar(select(GuildForumChannel)) is not None
```

Against the old manager: `assert None is not None`, the same data loss as `create_thread_in_db`.

## Worse than a lost row

I also drove the real `create_thread_in_db` with all caches emptied, so both nested blocks run. With the fix, the row persists. With the old manager, the script does not fail: it **hangs**. The leaked outer session never returns its connection to the pool, and the next session waits for it forever.

So the reported symptom was a failed assertion, but the real defect was a connection leak that could freeze the bot.

## What I learned

- Per-request state must not live on a shared object. In asyncio, `ContextVar` gives each task its own copy.
- Never `return` inside `finally`. It silently swallows exceptions.
- **A regression test that has never been seen to fail is a guess.** Revert the fix and watch the test go red before trusting it.
