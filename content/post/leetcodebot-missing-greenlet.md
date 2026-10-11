+++
date = '2026-08-28T20:27:00+08:00'
draft = false
title = 'LeetCodeBot: The Bug the Async Migration Did Not Write'
+++

## Context

[LeetCodeBot](https://github.com/xDDoubleTea/LeetCodeBot) is a Discord bot for looking up and discussing LeetCode problems. It stores problems and their topic tags with SQLAlchemy on SQLite.

In August 2026, [#35](https://github.com/xDDoubleTea/LeetCodeBot/pull/35) moved the database layer from synchronous SQLAlchemy to async (`sqlite+aiosqlite`). The bot kept running in production afterwards, with no errors.

## What happened

A fresh clone could not start the bot. Deleting the database file to "start clean" made it fail every time:

```
sqlalchemy.exc.MissingGreenlet: greenlet_spawn has not been called;
can't call await_only() here. Was IO attempted in an unexpected place?

  File "core/leetcode_problem.py", line 420, in add_problem_to_db
    if db_tag not in db_problem.tags:
```

The failing expression is a plain attribute read: `db_problem.tags`.

## The line nobody touched

Here is what the migration did to `add_problem_to_db`:

```diff
-        with self.database_manager as db:
+        async with self.async_database_manager as db:
-            db_problem = db.query(Problem).filter_by(...).first()
+            result = await db.scalars(select(Problem).where(...))
-                db.flush()
+                await db.flush()

                 if db_tag not in db_problem.tags:      # untouched
                     db_problem.tags.append(db_tag)     # untouched
```

Every line that did IO gained an `await`. The crashing line has no `+` or `-` at all: it is unchanged context.

It was missed because it does not look like IO. There is no call and nothing to await. But `Problem.tags` is a lazy-loaded relationship, so reading it emits a `SELECT`.

The same read against each engine:

```
SYNC  -> db_problem.tags  : []              works, lazy load just runs
ASYNC -> FAILED: MissingGreenlet            same line, different engine
```

In sync SQLAlchemy, lazy loading is the default and just works. The async engine quietly takes that away. So the migration's hazard was not in the lines it rewrote; it was a capability removed from a line it left alone.

## What `MissingGreenlet` means

SQLAlchemy's ORM is written as synchronous code, and the IO happens many frames deep. Making all of that `async def` would force every caller above it to become async too.

Instead, SQLAlchemy uses **greenlets**, coroutines that can suspend from anywhere in a call stack:

- `greenlet_spawn(fn)` runs synchronous ORM code inside a greenlet that it marks as a bridge.
- `await_only(awaitable)` is called deep inside that code when it needs IO. It switches back to the event loop, awaits, and returns the result.

`await_only` first checks that it is inside such a bridge:

```python
current = getcurrent()
if not getattr(current, "__sqlalchemy_greenlet_provider__", False):
    _safe_cancel_awaitable(awaitable)
    raise exc.MissingGreenlet(...)
return current.parent.switch(awaitable)
```

So the name means: synchronous ORM code tried to do IO, but there is no bridge in the current frame to hand that IO to the event loop. The bridge is observable with SQLAlchemy's own `in_greenlet()`:

```
in my own async frame         : False
inside SQLAlchemy's internals : True   <- statement about to run
```

Every awaited API (`await session.scalars(...)`, `await session.refresh(...)`) enters through `greenlet_spawn`. A bare attribute read does not: it runs in my own frame, where `in_greenlet()` is `False`.

The rule for async SQLAlchemy: **any attribute that may trigger IO must be loaded by something you awaited.**

## Why production never crashed

Both branches of `add_problem_to_db` reached the loop with `tags` unloaded:

| Branch                                          | State at the loop                              | Result                           |
| ----------------------------------------------- | ---------------------------------------------- | -------------------------------- |
| Existing problem, loaded without `selectinload` | persistent, `tags` unloaded                    | lazy `SELECT`, `MissingGreenlet` |
| New problem: `db.add()` then `await db.flush()` | the flush makes it persistent, `tags` unloaded | lazy `SELECT`, `MissingGreenlet` |

Yet production never reached either branch:

- `add_problem_to_db` runs only when the daily problem is missing from the database.
- The SQLite file lives in a bind-mounted host directory, so it survives every deploy.
- The daily refresh bulk-upserts every problem through Core inserts, which never touch the ORM relationship.

On a warm database the function was effectively dead code. On an empty one it was the only path, and it failed every time.

Three things had to line up to hide it:

1. The async engine made implicit lazy loading illegal.
2. Only this function relied on it. The other four queries in the file already used `selectinload`, for unrelated reasons.
3. This function is unreachable on a populated database.

Remove any one, and the bug would have shown up the day the migration merged.

## The fix

Both branches need their own fix ([#45](https://github.com/xDDoubleTea/LeetCodeBot/pull/45)):

```python
# 1. Eager load on the existing-problem query.
result = await db.scalars(
    select(Problem)
    .options(selectinload(Problem.tags))
    .where(Problem.problem_id == problem.problem_id)
)
db_problem = result.first()
if not db_problem:
    db.add(problem)
    await db.flush()
    db_problem = problem
    # 2. Load the collection for the newly persistent object.
    await db.refresh(db_problem, attribute_names=["tags"])
```

The new tests cover both branches and a `.tags` read after the session closes, since the bot caches the returned object. All three fail with `MissingGreenlet` against the old code.

To confirm end to end, I deleted the database and started the real bot. It passed the old crash site and logged in.

## What I learned

- When migrating to async, review the unchanged lines too. A line can become invalid without being edited.
- In async SQLAlchemy, an attribute access can be IO. Load relationships explicitly.
- A bug that only appears on cold state can live in `main` for weeks. Test from an empty database, not just against the one in production.
