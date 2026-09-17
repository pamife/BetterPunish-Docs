# Quick start

This walkthrough takes an operator from a clean installation to a first punishment and a first dashboard review.

## 1. Review the important defaults

Open `plugins/BetterPunish/config.yml` and set a network name:

```yaml
network-name: "Example Network"
```

If you are not using Discord or Supabase yet, leave their placeholder values unchanged. BetterPunish treats the shipped Supabase and local Discord endpoint placeholders as unconfigured.

Apply the changes:

```text
/punishreload
```

## 2. Apply a temporary ban

```text
/tempban Steve 7d Cheating
```

Durations accept combined week, day, hour, minute, and second units, for example `1d12h`.

For a permanent custom ban:

```text
/ban Steve Repeated chargeback fraud
```

When the last argument is `-s` or `--silent`, the public punishment announcement is suppressed and authorized staff receive a silent alert instead:

```text
/tempban Steve 24h Investigation pending -s
```

## 3. Inspect the record

Open the dashboard:

```text
/punishgui
```

Choose **Active Punishments**, then:

- left-click the player head to open details;
- right-click it to open the editor when you have `betterpunish.edit`;
- use **Player History** from the detail view to see all records for that name.

You can also look up the record directly:

```text
/checkban Steve
/history Steve
```

## 4. Attach evidence

Use the punishment ID shown by the command or dashboard:

```text
/proof add BAN-000001 https://evidence.example/case-123
/proof list BAN-000001
```

Proof values are stored with the punishment record. Treat them as moderator-visible references; BetterPunish does not upload or host files.

## 5. Remove the punishment

```text
/unban Steve
```

You may use the punishment ID instead of a player name. The dashboard also exposes a confirmation screen for active bans, mutes, and shadowmutes when the viewer has the matching removal permission.

## Next steps

- Grant staff the nodes in the [permissions reference](../reference/permissions.md).
- Customize punishment reasons and offense escalation in [configuration](configuration.md).
- Read the [dashboard guide](../punishments/dashboard.md) before onboarding moderators.
