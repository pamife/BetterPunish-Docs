# Installation

## Requirements

| Component | Current requirement |
| --- | --- |
| Server | Paper |
| Minecraft / Paper API | 1.21.11 |
| Java | 21 or newer within the Java 21 release line |
| Required plugins | None |
| Optional plugins | LuckPerms, Vulcan |
| Optional services | Supabase, BetterPunish-compatible Discord companion endpoint |

BetterPunish is compiled against the Paper API and uses Paper's asynchronous chat event. A Spigot-only server is therefore not supported by the current code.

## Install the plugin

1. Stop the Paper server.
2. Place the BetterPunish `.jar` in the server's `plugins/` directory.
3. Start the server with Java 21.
4. Confirm that `plugins/BetterPunish/config.yml` and `plugins/BetterPunish/data/` are created.
5. Review `config.yml` before giving moderators access.
6. Run `/punishreload` after supported configuration changes, or restart the server.

!!! note
    The distributed file name may include a version. The plugin name and data directory are `BetterPunish`.

## Compatibility notes

- LuckPerms and Vulcan are soft dependencies. BetterPunish starts without them.
- LuckPerms is only required when `mute-group-sync.enabled` is enabled.
- Vulcan is detected at runtime. If neither supported Vulcan event class is present, the integration is skipped.
- Supabase and Discord are disabled while their shipped placeholder values remain unchanged.

## First-start check

After startup, verify:

- the console reports that BetterPunish enabled without an exception;
- `/punishgui` opens for an operator;
- `/punishreload` reports that configuration and data were refreshed;
- the default Discord invite and companion endpoint have been replaced or deliberately left unused;
- moderators have explicit permissions rather than broad operator access.

Continue with the [quick start](quick-start.md).
