# Discord webhook logging (Components V2)

BetterPunish posts moderation logs directly to an ordinary Discord incoming webhook. No bot, application token, or Discord library is required. Notifications use Components V2 exclusively: a colored Container, an emoji heading, separators, Text Displays, and an optional Minecraft-head thumbnail. Player text is escaped and mentions cannot ping users or roles.

## Setup

1. In Discord, open **Server Settings → Integrations → Webhooks**, create a webhook, select a moderation channel, and copy its URL. Creating webhooks requires Manage Webhooks permission.
2. Start BetterPunish once. It creates `plugins/BetterPunish/discord.yml` with delivery disabled.
3. Set `enabled: true` and paste the full URL into `webhook.url`. Keep this URL private: it includes the webhook token.
4. Run `/betterpunish webhook reload`, then `/betterpunish webhook test`. `/tggpunish` is an alias.
5. A successful test reports Discord's confirmation. It creates a real message in the selected channel and does not create a punishment record.

For a forum/media channel, use an existing thread and append `?thread_id=YOUR_NUMERIC_THREAD_ID` to the webhook URL.

## Supported events and routing

| Event | Default accent |
| --- | --- |
| Ban / Tempban | `#ED4245` |
| Mute / Tempmute | `#FEE75C` |
| Kick | `#F47B67` |
| Unban / Unmute | `#57F287` |

Temporary versus permanent events are determined from the stored expiration, including configured reasons and offense escalation. Logging uses the successful punishment/removal path shared by commands, GUI removals, chat-review mutes, and automatic bans. Cancelled kicks and unsuccessful removals do not produce notifications. Silent actions still reach the private moderation webhook. Ordinary expiration, edits, warnings, and shadowmute applications are outside these seven events; removing a shadowmute is reported as Unmute.

When the direct webhook is enabled, these seven event types use it instead of the legacy companion's punishment broadcast, even when an individual event is filtered or delivery fails. This avoids a second Discord notification through the companion. Other companion alerts and Supabase synchronization continue as configured. With the webhook disabled, legacy companion behavior remains available through `config.yml`.

## Example configuration

```yaml
enabled: true
webhook:
  url: "https://discord.com/api/webhooks/WEBHOOK_ID/WEBHOOK_TOKEN"
  username: "TGGPunish"
  avatar-url: ""
language: de
title: "{emoji} {event}"
footer: "TGGPunish • Moderationsprotokoll"
timestamp:
  format: "dd.MM.yyyy HH:mm:ss z"
  timezone: "Europe/Berlin"
thumbnail:
  enabled: false
  url: "https://mc-heads.net/avatar/{uuid}/128"
fields:
  player: true
  uuid: false
  moderator: true
  reason: true
  duration: true
  id: true
  timestamp: true
  footer: true
events:
  ban:
    enabled: true
    color: "#ED4245"
  tempban:
    enabled: true
    color: "#ED4245"
  mute:
    enabled: true
    color: "#FEE75C"
  tempmute:
    enabled: true
    color: "#FEE75C"
  kick:
    enabled: true
    color: "#F47B67"
  unban:
    enabled: true
    color: "#57F287"
  unmute:
    enabled: true
    color: "#57F287"
delivery:
  timeout-seconds: 10
  max-rate-limit-retries: 3
```

Replace the URL placeholders with the copied Discord URL; placeholders are intentionally not valid webhook URLs. See the bundled `discord.yml` for English defaults. Existing files are preserved on startup.

- `language`: `en` or `de`, for event names and field labels. Customize the footer separately.
- `title`: plain text with `{emoji}` and `{event}` placeholders. Markdown characters in configured text and player values are escaped; the formatter supplies the layout.
- `fields`: hide individual fields. Duration is omitted for kicks and removals. Temporary durations show the total from creation to expiration, including remaining time carried over when extending a regular mute.
- `timestamp`: Java `DateTimeFormatter` pattern and timezone. Examples: `yyyy-MM-dd HH:mm:ss z`, `dd.MM.yyyy HH:mm`, `UTC`, `Europe/Berlin`.
- `thumbnail`: disabled by default. The HTTPS provider URL supports `{uuid}` and `{name}`. Discord fetches the image; enabling it shares that identifier with Discord and the image provider. Invalid player UUIDs fall back to text.
- `delivery.timeout-seconds`: 1–30 seconds for connection and request timeouts.
- `delivery.max-rate-limit-retries`: 0–5 retries after explicit HTTP 429 responses. The test command bypasses individual event filters, but respects the global enabled switch.

## Commands and permissions

| Command | Permission | Default |
| --- | --- | --- |
| `/betterpunish webhook test` | `betterpunish.webhook.test` | OP |
| `/betterpunish webhook reload` | `betterpunish.webhook.reload` | OP |

Both commands also work under `/tggpunish`. Tab completion respects these permissions. `/punishreload` reloads `discord.yml` along with existing services, using the existing `betterpunish.reload` permission. Invalid YAML or settings disable the webhook and leave punishments operational.

## Components V2 payload example

The service POSTs to `/api/v10/webhooks/WEBHOOK_ID/WEBHOOK_TOKEN?wait=true&with_components=true`. This representative shortened payload uses only non-interactive components supported by ordinary incoming webhooks:

```json
{
  "flags": 32768,
  "username": "TGGPunish",
  "allowed_mentions": { "parse": [], "users": [], "roles": [], "replied_user": false },
  "components": [{
    "type": 17,
    "accent_color": 15548997,
    "components": [
      { "type": 10, "content": "## ⏳ Player temporarily banned" },
      { "type": 14, "divider": true, "spacing": 1 },
      { "type": 10, "content": "**Player** · Steve" },
      { "type": 10, "content": "**Moderator** · Console\n**Reason** · Cheating\n**Duration** · 7d\n**Punishment ID** · BAN-000001\n**Timestamp** · 2026-11-10 18:00:00 UTC" },
      { "type": 14, "divider": true, "spacing": 1 },
      { "type": 10, "content": "-# TGGPunish • Moderation log" }
    ]
  }]
}
```

There is no top-level `content` or `embeds`. Optional thumbnails use a Section (9) containing one Text Display and a Thumbnail (11) accessory. Output is bounded below 40 components and 4,000 text characters; overlong values are truncated.

Protocol references: [Discord Components](https://docs.discord.com/developers/components/reference), [Execute Webhook](https://docs.discord.com/developers/resources/webhook#execute-webhook), [Rate Limits](https://docs.discord.com/developers/topics/rate-limits).

## Delivery behavior and troubleshooting

- HTTP and formatting run on a private background worker. A bounded queue holds at most 100 active/pending notifications. Queue overflow drops the new notification with a throttled warning; moderation still succeeds.
- Discord's `Retry-After`, `retry_after`, and exhausted-bucket reset headers govern serialized delivery. Retries are bounded. HTTP 401/403/404 or a requested delay longer than five minutes pauses delivery until reload.
- The last 2,048 accepted event IDs/types are deduplicated in memory. The Freeze-Log path emits only its existing creation event. Discord offers no webhook idempotency key: after an ambiguous network timeout or HTTP 5xx failure, delivery is not automatically retried to avoid duplicate messages. Check the channel before manually repeating a test.
- Reload and shutdown cancel pending requests and clear the in-memory queue/deduplication cache. There is no persistent outbox or replay of historical punishments.
- **No message:** check `enabled`, the event toggle, channel/thread selection, network access to Discord over HTTPS, and the result of the test command.
- **HTTP 401/403/404:** replace an invalid/deleted webhook or restore channel access, then reload. A failed delivery does not fall back to the companion.
- **HTTP 429:** wait for the reported rate limit; avoid repeatedly issuing tests. Check the retry setting if notifications are dropped after repeated rate limits.
- **HTTP 400:** verify the webhook/channel/thread and custom image URL. The test payload is a real Components V2 message, not a legacy embed.
- **Invalid configuration:** check YAML indentation, `en`/`de`, six-digit quoted hex colors, HTTPS URLs, timestamp pattern/timezone, and delivery bounds. Logs deliberately omit webhook URLs, response bodies, and exception details.
- **Need to disable:** set `enabled: false` and reload. Previously sent Discord messages remain in the channel.
