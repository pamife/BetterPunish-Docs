# Permissions

Most command permissions default to server operators. `betterpunish.report` defaults to all players; bypass and exemption nodes default to false.

## Punishment commands

| Permission | Default | Purpose |
| --- | --- | --- |
| `betterpunish.ban` | OP | Use `/ban`. |
| `betterpunish.tempban` | OP | Use `/tempban`. |
| `betterpunish.unban` | OP | Use `/unban` and confirm unbans in the dashboard. |
| `betterpunish.mute` | OP | Use `/mute`. |
| `betterpunish.tempmute` | OP | Use `/tempmute`. |
| `betterpunish.unmute` | OP | Use `/unmute` and confirm mute or shadowmute removal in the dashboard. |
| `betterpunish.shadowmute` | OP | Use `/shadowmute`. |
| `betterpunish.tempshadowmute` | OP | Use `/tempshadowmute`. |
| `betterpunish.warn` | OP | Use `/warn`. |
| `betterpunish.kick` | OP | Use `/kick`. |
| `betterpunish.offend` | OP | Use `/offend`. |
| `betterpunish.offend.exempt` | False | Prevent an online player from being targeted by `/offend`. |

## Records, dashboard, and evidence

| Permission | Default | Purpose |
| --- | --- | --- |
| `betterpunish.checkban` | OP | Use `/checkban`, `/check`, and `/pinfo`. |
| `betterpunish.history` | OP | Use `/history` and the dashboard's player-history action. |
| `betterpunish.gui` | OP | Open `/punishgui` or `/punishments`. |
| `betterpunish.edit` | OP | Use `/punishedit` and dashboard edit actions. |
| `betterpunish.proof` | OP | Add, remove, and list punishment evidence. |

## Moderation and reports

| Permission | Default | Purpose |
| --- | --- | --- |
| `betterpunish.report` | Everyone | Submit a report against an online player. |
| `betterpunish.reports` | OP | Open the reports queue. |
| `betterpunish.note` | OP | Manage staff notes. |
| `betterpunish.freeze` | OP | Freeze/unfreeze players and receive freeze alerts. |
| `betterpunish.clearchat` | OP | Clear chat. |
| `betterpunish.mutechat` | OP | Toggle global chat mute. |
| `betterpunish.slowmode` | OP | Change global slow mode. |
| `betterpunish.staff` | OP | Receive several staff alerts and bypass chat controls; accepted by the note, freeze, clear-chat, mute-chat, and slow-mode handlers. |
| `betterpunish.sus` | OP | Open the suspects dashboard. |

## Chat and logging

| Permission | Default | Purpose |
| --- | --- | --- |
| `betterpunish.chatreview` | OP | Open `/chatreview`. |
| `betterpunish.chatfilter.bypass` | False | Bypass the configured chat filter when explicit bypass is required. |
| `betterpunish.chatfilter.notify` | OP | Receive filter, shadow activity, report, alt, and suspicion alerts. |
| `betterpunish.commandlogs` | OP | Query stored commands and private-message commands. |
| `betterpunish.live` | OP | Monitor commands or private messages live. |

The following runtime nodes are checked by the code but are not declared as standalone entries in the current `plugin.yml`. Permission plugins may still grant them:

| Runtime node | Effect |
| --- | --- |
| `betterpunish.spam.bypass` | Bypass duplicate-message anti-spam when that is the configured bypass node. |
| `betterpunish.chat.bypass` | Bypass global chat mute and slow mode. |
| `betterpunish.clearchat.bypass` | Keep chat history when `/clearchat` is used without `all`. |

## Administration and integrations

| Permission | Default | Purpose |
| --- | --- | --- |
| `betterpunish.reload` | OP | Reload configuration and local services. |
| `betterpunish.migrate` | OP | Upload local punishment rows to Supabase. |
| `betterpunish.pull` | OP | Replace local history from Supabase. |
| `betterpunish.cleardb` | OP | Clear local and configured remote punishment data. |

## Wildcard and legacy nodes

`betterpunish.*` defaults to operators and grants the child nodes listed in the plugin descriptor. The current child list does **not** include every later moderation permission, including `betterpunish.note`, `betterpunish.freeze`, `betterpunish.clearchat`, `betterpunish.mutechat`, and `betterpunish.slowmode`. Grant role permissions explicitly instead of relying on the wildcard.

The code also accepts legacy `tggpunish.*` forms for offense, staff, and several runtime checks. Only these legacy nodes are declared:

- `tggpunish.offend`
- `tggpunish.offend.exempt`
- `tggpunish.staff`

Use the `betterpunish.*` names for new setups.
