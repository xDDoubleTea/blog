+++
date = '2026-10-10T18:25:00+08:00'
draft = false
title = 'DragonBot Ticket Close Hang'
categories = ['Debugging']
tags = ['python', 'discord.py', 'rate-limit']
+++

## Context

[DragonBot](https://github.com/xDDoubleTea/DragonBot) is a customer-support ticket bot for a purchasing-agent business on Discord, written with `discord.py`.

- A customer opens a ticket, which is a private channel visible only to the customer and staff.
- Every status change renames the channel to `{type}-{id:04d}-{status}`, so staff can see the status from the channel list.
- Closing a ticket sets the status to `CLOSED`, exports the conversation with DiscordChatExporter, posts the transcript to an archive channel, and DMs the customer a feedback form.

## What Happened

After a staff member confirmed closing a ticket, the bot sometimes sat on 「正在生成頻道紀錄以及傳送回饋單給顧客...」 for up to 10 minutes before the after-close buttons appeared. The owner reported it too.

On 2026-10-04, a ticket closed at 20:02 only finished at 20:12:45. There was **nothing** in the logs: no error, no warning.

## Investigation

My first guess was that the bot made too many API calls and got rate limited. Two things did not fit:

- `discord.py` handles HTTP 429 by sleeping and retrying. A normal rate limit costs seconds, not 10 minutes.
- The close finished about 10 minutes after the click, far longer than any normal retry.

The DM to the customer was also not the cause: `customer.send` already catches `Forbidden` and skips customers who have DMs closed.

So I read the close path again, counting how many times the channel gets renamed. A ticket that is answered and closed quickly is renamed **three** times:

| Event                                        | Status                 | Rename |
| -------------------------------------------- | ---------------------- | ------ |
| Staff's first reply                          | `OPEN` → `IN_PROGRESS` | 1      |
| Click 關閉頻道                               | → `RESOLVED`           | 2      |
| Confirm close (first line of `close_ticket`) | → `CLOSED`             | **3**  |

## Root cause

Discord allows only **2 channel renames per channel per 10 minutes**. The third rename has to wait for the window to reset.

- `set_ticket_status` awaited the rename, so `close_ticket` waited on it too.
- `discord.py` reads the exhausted bucket from the response headers and waits _before_ sending the request. Discord never returns a 429, so there is nothing to log.
- Even `discord.py`'s own "We are being rate limited" warnings never showed up. The bot starts with `bot.start()`, which, unlike `bot.run()`, does not configure `discord.py`'s logging.

## The actual fix in code

### Rename in the background

`set_ticket_status` now schedules the rename as a per-channel task instead of awaiting it:

```python
def _schedule_channel_rename(self, ticket: Ticket) -> None:
    # Only the latest status matters, so drop a rename still waiting on the limit
    self._cancel_pending_rename(channel_id=ticket.channel_id)
    task = asyncio.create_task(
        self._rename_channel_in_background(ticket=ticket),
        name=f"rename-ticket-{ticket.db_id}",
    )
    self._rename_tasks[ticket.channel_id] = task
```

- A newer status cancels a rename still waiting on the limit, so only the latest name is applied.
- Deleting a ticket cancels its pending rename.
- A rename slower than 5 seconds is logged as a warning, so the limit is visible from now on.

The trade-off: after a quick close, the channel name can lag the real status by up to 10 minutes. The close itself is immediate.

### Timeout for the exporter

DiscordChatExporter ran through `subprocess.run` with no timeout, so a hung export would block the close forever. It now times out after 180 seconds and reports the error in the channel:

```diff
-        subprocess.run(command, capture_output=True, text=True, check=True)
+        subprocess.run(
+            command,
+            capture_output=True,
+            text=True,
+            check=True,
+            timeout=EXPORT_TIMEOUT_SECONDS,
+        )
```

### Log `discord.py` too

```python
# bot.start(), unlike bot.run(), doesn't configure discord.py's logging.
discord_logger = logging.getLogger("discord")
discord_logger.setLevel(logging.INFO)
if not discord_logger.hasHandlers():
    discord_logger.addHandler(console_handler)
    discord_logger.addHandler(file_handler)
```

### Follow-up: stale archive names

Moving the rename to the background broke one thing. `close_ticket` used `channel.name` for the archive title, the transcript file name and the feedback DM. The name now could still be the old one, so ticket 8416's transcript came out as `代購-8416-處理完畢` instead of `已關閉`.

The fix is `get_ticket_channel_name(ticket)`, which builds the name from the ticket's status instead of reading the channel. `set_ticket_channel_name` uses it too, so the two cannot drift apart.

After both changes, closing a ticket finishes in seconds.

## What I learned

- A symptom that looks like "too many requests" can be a **per-resource** limit. Discord's rename limit is per channel, not per bot.
- A missing log line is evidence too. Here it meant the library never got a 429, because it waited before sending.
- Check what a library does silently, like waiting out rate limits, before blaming the network.
- `bot.start()` and `bot.run()` are not equivalent: only `bot.run()` sets up logging.
