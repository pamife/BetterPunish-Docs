# Commands

Arguments in angle brackets are required. Arguments in square brackets are optional. Bukkit checks the command permission declared in `plugin.yml` before the handler runs.

## Punishments

| Command | Description | Permission |
| --- | --- | --- |
| `/ban <player> <reason> [-s]` | Apply a configured ban when the reason is an exact configured key; otherwise apply a permanent custom ban. | `betterpunish.ban` |
| `/tempban <player> <duration> <reason> [-s]` | Apply a temporary custom ban. | `betterpunish.tempban` |
| `/unban <player\|id>` | Revoke an active ban by player name or record ID. | `betterpunish.unban` |
| `/mute <player> <reason> [-s]` | Apply a configured mute when the reason is an exact configured key; otherwise apply a permanent custom mute. | `betterpunish.mute` |
| `/tempmute <player> <duration> <reason> [-s]` | Apply a temporary custom mute. If a temporary mute is already active, the duration is appended to its remaining end time. | `betterpunish.tempmute` |
| `/unmute <player\|id>` | Revoke an active regular mute or shadowmute. | `betterpunish.unmute` |
| `/shadowmute <player> <reason> [-s]` | Apply a configured or permanent custom shadowmute. | `betterpunish.shadowmute` |
| `/tempshadowmute <player> <duration> <reason> [-s]` | Apply a temporary custom shadowmute. | `betterpunish.tempshadowmute` |
| `/warn <player> <reason> [-s]` | Record a warning. Offline players receive it on their next join. | `betterpunish.warn` |
| `/kick <player> <reason> [-s]` | Kick and record an online player. | `betterpunish.kick` |
| `/offend <player> <reason> [-s]` | Apply the next configured offense level. Disabled in `LEGACY` mode. | `betterpunish.offend` |
| `/history <player> [page]` | Show paginated punishment history. | `betterpunish.history` |
| `/checkban <player\|id>` | Show a record or a player's active ban, mute, shadowmute, freeze state, and staff notes. | `betterpunish.checkban` |
| `/check <player\|id>` | Same handler as `/checkban`. | `betterpunish.checkban` |
| `/proof add <id> <url>` | Attach an evidence reference to a punishment. | `betterpunish.proof` |
| `/proof remove <id> <url\|index>` | Remove evidence by exact value or one-based index. | `betterpunish.proof` |
| `/proof list <id>` | List clickable evidence references. | `betterpunish.proof` |
| `/punishedit <id>` | Open the edit GUI in game. | `betterpunish.edit` |
| `/punishedit <id> reason <value>` | Change the displayed reason. | `betterpunish.edit` |
| `/punishedit <id> duration <value>` | Change total duration from the record's original creation time. | `betterpunish.edit` |
| `/punishedit <id> staff <value>` | Change the displayed issuing moderator. | `betterpunish.edit` |

The final `-s` or `--silent` flag suppresses the public punishment announcement. It does not hide the action from the console, authorized staff alerts, local storage, Supabase, or the Discord companion endpoint.

## Dashboards and investigation

| Command | Description | Permission |
| --- | --- | --- |
| `/punishgui [category] [page]` | Open the dashboard or a category directly. Categories: `active`, `recent`, `expired`, `revoked`, `bans`, `mutes`, `warns`, `shadowmutes`, `search`. In-game only. | `betterpunish.gui` |
| `/punishments [category] [page]` | Alternate command for `/punishgui`. | `betterpunish.gui` |
| `/pinfo <player\|id>` | Open the player profile GUI. Console falls back to the text check. | `betterpunish.checkban` |
| `/sus` | Open the in-memory suspects dashboard. In-game only. | `betterpunish.sus` |
| `/suspects` | Alternate command for `/sus`. | `betterpunish.sus` |

## Reports and staff tools

| Command | Description | Permission |
| --- | --- | --- |
| `/report <player> <reason>` | Report an online player. In-game only. | `betterpunish.report` |
| `/reports [page]` | Open the non-resolved report queue. In-game only. | `betterpunish.reports` |
| `/note add <player> <text>` | Add an internal staff note. | `betterpunish.note` |
| `/note list <player>` | List staff notes. | `betterpunish.note` |
| `/note remove <player> <id>` | Remove one note. `delete` is also accepted as the subcommand. | `betterpunish.note` |
| `/note clear <player>` | Remove all notes for the player. | `betterpunish.note` |
| `/freeze <player> [reason]` | Toggle freeze for an online player. | `betterpunish.freeze` |
| `/unfreeze <player>` | Explicitly unfreeze an online player. | `betterpunish.freeze` |
| `/clearchat [all\|-a] [reason]` | Push existing chat out of view for regular players, or everyone with `all`/`-a`. | `betterpunish.clearchat` |
| `/mutechat [reason]` | Toggle global chat mute. | `betterpunish.mutechat` |
| `/slowmode <seconds\|off>` | Set or disable the in-memory global slow mode. | `betterpunish.slowmode` |

## Chat review and logging

| Command | Description | Permission |
| --- | --- | --- |
| `/chatreview [page]` | Open the chat-filter review queue. In-game only. | `betterpunish.chatreview` |
| `/playermsgs <player> [limit]` | Query logged private-message commands involving a player. | `betterpunish.commandlogs` |
| `/playercommands <player> [limit]` | Query logged commands issued by a player. | `betterpunish.commandlogs` |
| `/allmsgs [limit]` | Query recent logged private-message commands for all players. | `betterpunish.commandlogs` |
| `/livemsgs <player\|all\|off>` | Start or stop live private-message monitoring. In-game only. | `betterpunish.live` |
| `/livecommands <player\|all\|off>` | Start or stop live command monitoring. In-game only. | `betterpunish.live` |

Query limits must be positive and are capped at 100.

## Administration

| Command | Description | Permission |
| --- | --- | --- |
| `/punishreload` | Reload configuration and the reloadable local services. | `betterpunish.reload` |
| `/punishmigrate` | Upload core local punishment history rows to configured Supabase storage. | `betterpunish.migrate` |
| `/punishpull` | Replace local punishment history with rows pulled from Supabase. | `betterpunish.pull` |
| `/punishcleardb` | Reset local punishment history and pending warnings; also request deletion of the matching Supabase tables when configured. | `betterpunish.cleardb` |

!!! danger
    `/punishpull` and `/punishcleardb` change stored moderation data. Back up the `data/` directory first.
