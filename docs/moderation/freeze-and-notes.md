# Freeze and staff notes

## Freeze

```text
/freeze Steve Suspected client modifications
/unfreeze Steve
```

`/freeze` toggles the state for an online target. `/unfreeze` only removes it.

While frozen, the current implementation:

- prevents position changes while still allowing view direction changes;
- blocks public chat;
- blocks commands except those beginning with `/appeal` or `/helpop`;
- blocks block breaking and placement;
- blocks damage dealt by the player and damage received by the player;
- blocks item dropping and pickup;
- repeatedly displays a title, action bar, and configured support link.

Freeze state is memory-only and is cleared when the plugin stops. The current listener does not generally cancel inventory clicks.

## Freeze-log

When a frozen player disconnects:

1. freeze state is removed;
2. console and authorized staff receive a freeze-log alert;
3. if `freeze.auto-ban-on-disconnect` is true, BetterPunish creates the configured ban.

```yaml
freeze:
  auto-ban-on-disconnect: true
  auto-ban-duration: "14d"
  auto-ban-reason: "Freeze-Log / Support Refusal"
```

The clickable support link comes from `discord.invite-url`. Replace the shipped placeholder before using freeze in production.

## Staff notes

```text
/note add Steve Admitted using a prohibited client
/note list Steve
/note remove Steve 3
/note clear Steve
```

Notes contain a numeric ID, target UUID and stored name, moderator, timestamp, and free-form content. They are visible in `/check`, `/checkban`, and the `/pinfo` GUI.

Notes are stored in `plugins/BetterPunish/data/notes.yml`; they are not synchronized to Supabase by the current implementation.

!!! warning
    Notes are internal moderation data, not a place for passwords, IP addresses, or unrelated personal information.
