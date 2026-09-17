# Configuration

BetterPunish reads `plugins/BetterPunish/config.yml`. Run `/punishreload` or restart the server after editing it.

!!! warning "Keep credentials private"
    Never paste real Supabase keys or private service URLs into public issue reports, screenshots, or documentation. The examples below intentionally use placeholders.

## Network and support links

```yaml
network-name: "Example Network"

discord:
  invite-url: "YOUR_DISCORD_INVITE_URL"
  display: "YOUR_DISCORD_DISPLAY_TEXT"
  bot-url: "YOUR_COMPANION_BASE_URL"
```

`invite-url` and `display` appear in freeze messages and the ban screen. `bot-url` is the base URL of a companion HTTP service; it is not a Discord webhook URL.

## Punishment mode

```yaml
punishment-system:
  mode: OFFENSE
  reset-timer-days: 90
  reason-format: "<category>"
  history-page-size: 10
```

| Option | Effect |
| --- | --- |
| `mode` | `LEGACY` disables `/offend`; `OFFENSE` and `HYBRID` allow it. Other punishment commands remain available. |
| `reset-timer-days` | Resets the counted offense level when the gap since the last matching offense exceeds this many days. |
| `reason-format` | Display text for offense-generated records. Supports `<category>` and `<level>`. |
| `history-page-size` | Number of entries shown per `/history` page. |

## Reasons and escalation

Each entry under `reasons` defines a type, display label, default duration, and optional offense levels:

```yaml
reasons:
  Advertising:
    type: MUTE
    display: "Advertising"
    duration: "7d"
    offenses:
      1: "Warning"
      2: "7d"
      3: "permanent"
```

Supported types in this configuration are `BAN` and `MUTE`. A level value of `Warning` creates a warning; duration values create the configured ban or mute. After the highest configured level, BetterPunish continues using that highest level.

Durations accept `w`, `d`, `h`, `m`, and `s`, including combinations such as `1d12h`. The values `permanent` and `perm` never expire. `double current mute` requires an active temporary mute.

## Dashboard and chat input

```yaml
gui:
  dashboard-title: "<gradient:#f97316:#fb7185><bold>BetterPunish Dashboard</bold></gradient>"
  list-title-format: "<gradient:#f97316:#fb7185><bold><title></bold></gradient> <dark_gray>[<page>/<pages>]</dark_gray>"
  details-title-format: "<gradient:#f97316:#fb7185><bold>Punishment Details</bold></gradient> <dark_gray><id></dark_gray>"
  input-timeout-seconds: 60
```

The input timeout is clamped to a minimum of 10 seconds. GUI labels use Adventure MiniMessage formatting.

## Chat filter

```yaml
chat-filter:
  enabled: true
  bypass-permission: "betterpunish.chatfilter.bypass"
  require-explicit-bypass-permission: true
  notify-permission: "betterpunish.chatfilter.notify"
  log-to-console: true
  rules:
    example-rule:
      enabled: true
      reason-key: "InappropriateLanguage"
      fallback-duration: "6h"
      fallback-reason-display: "Chat rule violation"
      plain-regex:
        - '\\bexample\\b'
      joined-regex:
        - 'example'
      squeezed-regex:
        - 'example'
```

The default file contains a ready-made rule set. Edit regexes carefully and test with non-production accounts. The three pattern groups receive different normalized views; see [Chat protection](../chat/protection-and-review.md).

The implementation also accepts an optional `anti-spam` section. It is enabled with a three-second duplicate-message window when the section is absent:

```yaml
anti-spam:
  enabled: true
  cooldown-seconds: 3
  bypass-permission: "betterpunish.spam.bypass"
  reason: "Spamming the same message"
  warning-message: "<red>Please do not repeat the same message so quickly.</red>"
```

## Command logging

```yaml
command-log:
  enabled: true
  max-stored-entries: 5000
  default-query-limit: 20
  autosave-interval-ticks: 200
  private-message-aliases:
    - "msg"
    - "tell"
    - "w"
    - "reply"
    - "r"
```

Namespaced Minecraft aliases are supported in the shipped configuration. Query limits are capped at 100 even when a larger number is requested.

## LuckPerms mute group

```yaml
mute-group-sync:
  enabled: true
  group-name: "mutet"
  reconcile-interval-ticks: 200
```

When enabled and LuckPerms is present, active regular mutes add the configured inheritance group. The node is removed when no regular mute remains. Shadowmutes do not drive this group.

## Supabase

```yaml
supabase:
  url: "YOUR_SUPABASE_URL"
  anon-key: "YOUR_SUPABASE_KEY"
```

Use a dedicated project and keep the key private. Database tables and synchronization limits are covered in the [Supabase guide](../integrations/supabase.md).

## Ban announcements

```yaml
ban-broadcast:
  enabled: true
  sound: "ITEM_MACE_SMASH_GROUND"
  volume: 10.5
  pitch: 0.8
  format: "<red><bold><player> was banned</bold></red>"
```

Non-silent, non-anticheat bans use this public broadcast. If the configured sound is invalid, BetterPunish logs a warning and attempts its built-in fallback sound.

## Freeze-log behavior

```yaml
freeze:
  auto-ban-on-disconnect: true
  auto-ban-duration: "14d"
  auto-ban-reason: "Freeze-Log / Support Refusal"
```

Freeze state is held in memory. Disconnecting while frozen removes that state and, when enabled, immediately creates the configured ban.
