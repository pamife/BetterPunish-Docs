# Discord and LuckPerms

## Discord companion endpoint

The current integration is an HTTP companion service, not a direct Discord webhook.

```yaml
discord:
  invite-url: "YOUR_DISCORD_INVITE_URL"
  display: "YOUR_DISCORD_DISPLAY_TEXT"
  bot-url: "YOUR_COMPANION_BASE_URL"
```

BetterPunish considers the companion unconfigured when `bot-url` is blank, `none`, or still contains the shipped local placeholder.

When configured, it sends JSON with an HTTP POST to:

```text
<bot-url>/broadcast-punishment
```

The body contains the event type, player UUID and name, actor name, reason, duration, and timestamp. Events include punishments and removals, reports, alt alerts, and suspicion-threshold alerts.

!!! warning "Trust boundary"
    The current client does not add an authentication header. Put the companion behind a protected network path or reverse proxy that authenticates requests. Use HTTPS for non-local traffic and never place a credential in the public documentation.

`invite-url` and `display` are separate: they are shown to players on freeze messages and ban screens. Replace both shipped placeholders before publishing the plugin configuration to players.

## LuckPerms mute-group synchronization

LuckPerms is a soft dependency used only when mute-group sync is enabled:

```yaml
mute-group-sync:
  enabled: true
  group-name: "mutet"
  reconcile-interval-ticks: 200
```

Behavior:

- an active regular mute adds the configured inheritance group;
- expiration, revocation, or duration edits remove it when no regular mute remains;
- the plugin reconciles all UUIDs found in punishment history on reload and at the configured interval;
- a joining player is checked immediately;
- shadowmutes do not add the group.

If enabled without LuckPerms, BetterPunish logs a warning and continues without group updates. Create the group in LuckPerms before enabling synchronization and ensure it actually removes the communication abilities used by your server's chat plugins.
