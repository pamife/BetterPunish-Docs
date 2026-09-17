# Command and private-message logging

When `command-log.enabled` is true, BetterPunish records every non-cancelled player command observed by the command preprocess listener.

Each entry contains:

- timestamp;
- player UUID and name;
- normalized full command;
- command label;
- whether it matches a configured private-message alias;
- parsed target name and online UUID when available.

Entries are stored locally in `data/command-logs.yml`, ordered oldest-first on disk, and trimmed to `command-log.max-stored-entries`. Dirty data is autosaved at the configured tick interval and again on shutdown.

## Query stored logs

```text
/playercommands Steve 25
/playermsgs Steve 25
/allmsgs 50
```

`/playermsgs` matches messages sent by or addressed to the player. `/allmsgs` returns recognized private-message commands across all players. Results are newest-first; the configured default applies when no limit is supplied, and all requests are capped at 100.

## Live monitoring

```text
/livecommands Steve
/livecommands all
/livecommands off

/livemsgs Steve
/livemsgs all
/livemsgs off
```

Each viewer can hold one command subscription and one message subscription. A player-filtered private-message monitor matches either sender or parsed target. Subscriptions are memory-only and are cleared when the viewer disconnects.

## Coverage limits

- Only player commands reaching the monitor listener without prior cancellation are recorded.
- Private messages are identified by configured command labels; messages sent through an unlisted plugin command are ordinary command logs.
- A reply command may not expose a concrete target UUID in the stored entry.
- Console commands are not recorded by this service.

!!! danger "Privacy and access control"
    Command arguments can include private messages, authentication strings typed into commands, or other sensitive content. Grant `betterpunish.commandlogs` and `betterpunish.live` narrowly, protect backups, set an appropriate entry cap, and disclose logging to players where required.
