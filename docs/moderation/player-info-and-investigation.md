# Player information and investigation

## Text check

```text
/check Steve
/checkban Steve
/check BAN-000001
```

A record ID lookup shows that record's status, reason, moderator, timeline, source message when present, evidence, and revocation details. A player lookup shows active ban, mute, and shadowmute status, freeze state, and staff notes.

## Player profile

```text
/pinfo Steve
```

In game, `/pinfo` opens a 54-slot profile showing:

- online/offline state and UUID;
- number of linked accounts and total stored sanctions;
- active ban, regular mute, and shadowmute cards;
- current in-memory freeze state;
- linked account names;
- up to five staff notes with an overflow count;
- a button that runs `/history <player>`.

From console, `/pinfo` falls back to the text check handler.

## Alt-account detection

On join, BetterPunish records the player's current IP association locally and optionally in Supabase. It checks other accounts associated with the same address. If one has an active ban, console and authorized staff receive an alert and the Discord companion endpoint is notified.

The player profile displays only linked account names, not raw addresses. Local raw associations are stored in `data/player_ips.yml`.

!!! danger "Privacy"
    IP addresses are personal data in many jurisdictions. Restrict filesystem and database access, define a lawful purpose and retention period, and do not expose the storage file in support tickets or public repositories.

Supabase failures fall back to locally known associations for that lookup.

## Vulcan and the suspects dashboard

BetterPunish detects supported Vulcan flag events at runtime. Each flag adds one point for its check name. A general kick observed outside BetterPunish contributes 20 points; recorded BetterPunish kick history also contributes 20 points per non-revoked kick.

The default suspicion alert threshold is 50. The implementation reads `vulcan.suspicion-threshold` when present, even though this key is not included in the shipped default file:

```yaml
vulcan:
  suspicion-threshold: 50
```

`/sus` and `/suspects` show suspects in score order. The working controls are:

- left-click to teleport to an online suspect;
- right-click to reset that in-memory suspect score;
- page arrows to navigate.

Suspect flags and reset state are memory-only. They are not restored after restart.

!!! note "Known control limitation"
    The current GUI displays a middle-click hint that invokes an unregistered `/punish` command. It is not documented as a working action.
