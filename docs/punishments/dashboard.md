# Punishment dashboard

`/punishgui` and `/punishments` open the same 45-slot landing dashboard for in-game moderators.

## Dashboard categories

| Category | Records shown |
| --- | --- |
| Active Punishments | All currently active record types. |
| Recent Punishments | All records, newest first. |
| Expired Punishments | Temporary records whose expiration has elapsed. |
| Revoked Punishments | Records explicitly removed by staff or replaced by a newer punishment. |
| Bans | Every ban record, regardless of status. |
| Mutes | Every regular mute record. |
| Warns | Every warning record. |
| Shadowmutes | Every shadowmute record. |
| Search Player | Records whose stored player name contains the entered name, case-insensitively. |

Direct category syntax is also available:

```text
/punishgui active
/punishments revoked 2
/punishgui search
```

## List navigation

Lists contain up to 36 records per page.

- **Previous Page** and **Next Page** move through matching records.
- **Back to Dashboard** returns to the category screen.
- **Search Player** starts a chat-input search.
- Left-click a player head to open details.
- Right-click a player head to edit it when you have `betterpunish.edit`.

Search input must be a valid Minecraft-style name containing 1–16 letters, digits, or underscores. Type `cancel` to stop, or wait for the configured timeout.

## Punishment details

The detail screen shows:

- target and punishment ID;
- reason and original reason key;
- displayed moderator;
- creation, expiration, and remaining time;
- current status and revocation details;
- number of attached proofs;
- most recent edit and total edit count.

Available actions:

- **Proofs** closes the GUI and runs `/proof list <id>` when permitted.
- **Edit Punishment** opens the editor when permitted.
- **Player History** opens a filtered dashboard list when `betterpunish.history` is granted.
- **Unban Player** or **Unmute Player** appears for an active ban, mute, or shadowmute.
- **Back** returns to the prior list and page, or the dashboard.

## Removal confirmation

Active bans require `betterpunish.unban`; active mutes and shadowmutes require `betterpunish.unmute`. The action opens a separate confirmation inventory:

- **Confirm** revokes the record and returns to its details.
- **Cancel** returns without changing data.

The record is checked again at confirmation time so an already expired or removed punishment cannot be revoked twice.
