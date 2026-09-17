# BetterPunish

<div class="bp-hero" markdown>
<div markdown>
<p class="bp-kicker">Moderation for Paper servers</p>

BetterPunish brings punishments, evidence, reports, chat protection, staff investigation, and moderation logs into one plugin. It works with local YAML storage out of the box and can optionally synchronize selected data with Supabase, LuckPerms, Vulcan, and a Discord companion service.

<div class="bp-actions" markdown>

[Install BetterPunish](getting-started/installation.md){ .md-button .md-button--primary }
[Open the quick start](getting-started/quick-start.md){ .md-button }

</div>
</div>

![BetterPunish logo](assets/logo.png)
</div>

!!! info "Version requirements"
    The current plugin build targets **Paper 1.21.11** and requires **Java 21**. It uses Paper APIs directly and is not documented as compatible with Spigot.

<div class="grid cards" markdown>

-   :material-gavel:{ .lg .middle } **Punishment management**

    ---

    Apply bans, mutes, shadowmutes, warnings, and kicks; revoke active sanctions; retain searchable history; and attach evidence.

    [Punishment overview](punishments/overview.md)

-   :material-view-dashboard:{ .lg .middle } **Moderator dashboard**

    ---

    Browse active, recent, expired, revoked, and type-specific records. Search players, inspect details, edit records, and confirm removals.

    [Dashboard guide](punishments/dashboard.md)

-   :material-account-alert:{ .lg .middle } **Reports and investigations**

    ---

    Process player reports, freeze online players, keep staff notes, inspect player profiles, find linked accounts, and review Vulcan signals.

    [Moderation tools](moderation/reports.md)

-   :material-message-alert:{ .lg .middle } **Chat protection**

    ---

    Block configured patterns after normalization, detect duplicate-message spam, review flags in a GUI, and enforce mute and shadowmute behavior.

    [Chat protection](chat/protection-and-review.md)

-   :material-text-box-search:{ .lg .middle } **Moderation logging**

    ---

    Store player commands and recognized private-message commands locally, query recent entries, or monitor them live in game.

    [Logging guide](logging/commands-and-messages.md)

-   :material-cloud-sync:{ .lg .middle } **Optional integrations**

    ---

    Synchronize selected records with Supabase, mirror regular mute state to LuckPerms, consume Vulcan flags, or send events to a Discord companion endpoint.

    [Integration guides](integrations/supabase.md)

</div>

## Start here

- [Installation](getting-started/installation.md) — requirements, compatibility, and first startup.
- [Quick start](getting-started/quick-start.md) — apply a first punishment and open the dashboard.
- [Configuration](getting-started/configuration.md) — the options that materially change behavior.
- [Commands](reference/commands.md) — complete command syntax and aliases.
- [Permissions](reference/permissions.md) — command and runtime permission nodes.

!!! warning "Sensitive moderation data"
    BetterPunish can store private messages, command text, staff notes, reports, and IP-derived account links. Restrict access to the plugin data directory, Supabase project, console logs, and logging commands. Define a retention and disclosure policy that fits your community and local law.
