# Punishment system

BetterPunish keeps every action as a record with an ID, target UUID and name, moderator, reason, timestamps, status, silent flag, evidence references, and edit history. Local records are stored even when Supabase is configured.

## Types

| Type | Active enforcement | History behavior |
| --- | --- | --- |
| Ban | Blocks login; an online target is kicked immediately. | Permanent or temporary; can be revoked with `/unban`. |
| Mute | Blocks public chat and recognized private-message commands. | Permanent or temporary; can be revoked with `/unmute`. |
| Shadowmute | Echoes public chat and private messages only to the sender while alerting console and authorized staff. | Permanent or temporary; removed with `/unmute`. |
| Warning | Sends immediately or queues delivery for the next join. | Recorded as a non-active history entry. |
| Kick | Requires an online player and disconnects them immediately. | Recorded as a non-active history entry. |

## Durations

Temporary commands accept one or more duration parts:

```text
30m
6h
7d
1d12h
2w
```

Units are weeks (`w`), days (`d`), hours (`h`), minutes (`m`), and seconds (`s`). `permanent` and `perm` create no expiration.

Applying a new punishment of the same type revokes the previous active record as **Replaced by newer punishment**. For regular mutes, a new temporary duration extends an existing temporary mute from its current expiration. A permanent active mute remains permanent.

## Configured and custom reasons

`/ban`, `/mute`, and `/shadowmute` treat a single reason argument as a configured key when it matches exactly. The configured duration and display name are then used. Multi-word text or an unknown single key becomes a custom permanent reason.

```text
/ban Steve Cheating
/mute Steve Advertising
/ban Steve Repeated exploit abuse
```

Temporary commands always use the duration and free-text reason supplied on the command line.

## Silent actions

Append `-s` or `--silent` to punishment commands:

```text
/mute Steve  Investigation in progress -s
```

For silent records, BetterPunish suppresses the normal public announcement and sends a staff alert to the console and players with `betterpunish.chatfilter.notify`. Persistence and configured integrations still receive the record.

## Offline players

Bans, mutes, shadowmutes, and warnings accept offline targets through Bukkit's offline-player lookup. Offline warnings are saved locally and, when Supabase is configured, inserted into `pending_warnings`. They are delivered shortly after the next join and then cleared.

## Revocation and expiration

- Expired records remain in history but are no longer enforced.
- `/unban` and `/unmute` mark the matching record revoked; they do not delete it.
- `/unmute` can remove either a regular mute or a shadowmute.
- Revocation records include the moderator, time, and generated reason.
- The dashboard separates active, expired, and revoked states.

## Mute bypass protection

Regular mutes block:

- Paper public chat;
- configured private-message command aliases;
- sign edits;
- book edits;
- anvil rename output.

Shadowmutes cancel those channels but imitate success where practical: public chat is echoed to the sender, recognized private messages are echoed to the sender, and sign content is restored client-side. Console and staff with the notification permission receive shadow activity alerts.

## History and lookup

```text
/history Steve
/history Steve 2
/checkban Steve
/checkban BAN-000001
```

History is sorted newest-first. The page size comes from `punishment-system.history-page-size`. A record lookup shows status, timeline, reason, moderator, source message when present, evidence links, and revocation details.
