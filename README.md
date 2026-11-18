# zoned in

A Discord bot that takes the pain out of timezones. Save your timezone once and it works in every server the bot is in. When you mention a time like `6pm`, the bot replies with the equivalent UTC time, so everyone knows what you meant.

> **@Alex** is from UTC+8, in UTC+0 that is **10:00 (10:00 AM)**

## Features

- **One timezone per user, everywhere.** Set it in one server and it applies in all of them.
- **Automatic conversion.** Reads messages and detects times like `6pm`, `6:30 pm`, `18:00` and `7 p.m.`, then replies with the UTC equivalent. It adds "(next day)" or "(previous day)" when the date shifts.
- **Slash commands** to set timezones, look up other users' local time, and convert times.
- **Admin controls.** Server admins, or a role they choose, can set or remove other users' timezones.
- **Daylight saving aware** when you use a named timezone such as `America/New_York`.

## Commands

| Command | What it does |
| --- | --- |
| `/settimezone timezone [user]` | Set your timezone. Admins can set someone else's with `user`. |
| `/mytimezone` | Show your saved timezone. |
| `/removetimezone [user]` | Delete your saved timezone (admins can remove others'). |
| `/timefor user` | Show the current local time for a user. |
| `/convert time [user]` | Convert a time from your timezone to UTC, and optionally to another user's timezone. |
| `/setadminrole [role]` | (Manage Server) Choose the role that can set other users' timezones. Leave empty to clear. |
| `/autoconvert enabled` | Turn automatic message replies on or off for the server. |

Timezones can be IANA names (`Asia/Manila`, `America/New_York`, case-insensitive) or UTC offsets (`UTC+8`, `+5:30`, `GMT-3`). The `timezone` option autocompletes IANA names as you type.

### Who counts as an admin?

Anyone with the **Administrator** or **Manage Server** permission, plus members of the role set with `/setadminrole`.

## How time detection works

The bot looks for times written with am/pm (`6pm`, `6:30 pm`, `7 p.m.`) or in 24-hour form with a colon (`18:00`). A bare number like `6` is never treated as a time. Times inside code blocks, links, mentions and Discord timestamps are ignored. Only people who have saved a timezone get replies, and at most five times per message are converted.

## Setup (self-hosting)

1. Install Python 3.10 or newer, then the dependencies:
   ```
   pip install -r requirements.txt
   ```
2. Copy `.env.example` to `.env` and put your bot token in it:
   ```
   DISCORD_TOKEN=your-bot-token-here
   ```
   Never share or commit this file.
3. In the [Discord Developer Portal](https://discord.com/developers/applications), open your application, go to **Bot**, and turn on **Message Content Intent**. Without it the bot can't read messages.
4. Invite the bot using the `bot` and `applications.commands` scopes, with these permissions: View Channels, Send Messages, Read Message History.
5. Start it:
   ```
   python bot.py
   ```

Slash commands are registered globally when the bot starts and may take a few minutes to appear.

### Configuration

| Variable | Required | Description |
| --- | --- | --- |
| `DISCORD_TOKEN` | Yes | Your bot token from the Developer Portal. |
| `ZONEDIN_DB` | No | Path to the SQLite database file. Defaults to `zonedin.db`. |

## Data

zoned in stores only what it needs to work, in a local SQLite file: your Discord user ID and timezone, plus each server's ID, admin role and auto-conversion setting. Message text is read to detect times but is never stored. See the [Privacy Policy](PRIVACY.md) for details.

## Legal

- [Terms of Service](TERMS.md)
- [Privacy Policy](PRIVACY.md)

## Troubleshooting

- **The bot doesn't reply to times:** check that Message Content Intent is enabled, that the bot can send messages in the channel, that you've set a timezone with `/settimezone`, and that `/autoconvert` hasn't been turned off.
- **Slash commands don't show up:** wait a few minutes after the first start, and make sure the bot was invited with the `applications.commands` scope.
- **"DISCORD_TOKEN is missing":** your `.env` file is missing, misnamed, or not in the same folder as `bot.py`.
