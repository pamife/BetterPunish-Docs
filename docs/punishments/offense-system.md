# Offense system

`/offend` calculates the next level for a configured category and applies the corresponding warning, mute, or ban.

## Enablement

```yaml
punishment-system:
  mode: OFFENSE
  reset-timer-days: 90
  reason-format: "<category> (level <level>)"
```

`LEGACY` disables `/offend`. The current handler permits it in `OFFENSE` and `HYBRID`.

## Configure a category

```yaml
reasons:
  Spam:
    type: MUTE
    display: "Spam"
    duration: "1h"
    offenses:
      1: "Warning"
      2: "30m"
      3: "2h"
```

Use the category key with the command:

```text
/offend Steve Spam
/offend Steve Harassment/Trolling -s
```

Spaces, underscores, and hyphens are ignored when matching category names. A target who is online and has `betterpunish.offend.exempt` or the legacy exemption cannot be punished through this command.

## Level calculation

BetterPunish:

1. finds non-revoked history entries whose reason key or display text matches the category;
2. orders them oldest-first;
3. resets the running level after a gap longer than `reset-timer-days`;
4. also resets when the latest match is older than the timer;
5. selects the next level, or keeps using the highest configured level when the count exceeds the table.

A duration value of `Warning` records and delivers a warning. Otherwise, the configured reason type selects a ban or mute.

!!! note
    Revoking a matching punishment removes it from future offense counts. Ordinary expiration does not.

## Audit output

Each use writes a local punishment record and appends a summary line to `data/offenses.log`. The log includes the player UUID, moderator, category, calculated level, duration, type, and timestamp. Protect this file as moderation data.
