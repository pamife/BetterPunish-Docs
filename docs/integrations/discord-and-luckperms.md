# Discord and LuckPerms

For direct incoming Discord webhooks with Components V2, see [Discord webhook logging](discord-webhooks.md). While enabled, it replaces the companion punishment broadcast for bans, temporary bans, mutes, temporary mutes, kicks, unbans, and unmutes.


## Discord companion endpoint

The legacy companion uses a separate HTTP service. Direct incoming webhooks are configured independently in `discord.yml`.

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

## LuckPerms permission editor

In Minecraft, run `/betterpunish perm setup` and click **Choose a group** or **Choose a player**. Enter the LuckPerms group or player name. Tab suggests loaded groups and online players. You can enter the name of an offline player known to LuckPerms.

- **✓ Allow** grants the permission. **✗ Deny** sets an explicit denial.
- **Reset** removes the direct permanent global assignment. Inheritance, defaults and contextual or temporary rules may still affect access.
- **Skip** moves to the next permission. **Cancel** closes the editor.

Each permission is saved as you go. The editor shows the target's permanent global assignment. It edits individual BetterPunish permissions and requires `betterpunish.perm` (OP by default).

## LuckPerms mute-group synchronization

BetterPunish uses LuckPerms to synchronize the mute group when enabled:

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
