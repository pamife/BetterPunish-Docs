# Chat protection and review

BetterPunish applies chat controls in this order: punishment GUI chat input, freeze, global chat mute, slow mode, shadowmute, duplicate-message anti-spam, configured filter, then regular mute enforcement.

## Duplicate-message anti-spam

When enabled, sending the same trimmed, case-insensitive message again inside the cooldown cancels it and records a warning from Console. The built-in fallback is enabled with a three-second window even when no `anti-spam` block is present.

Grant the configured bypass permission—`betterpunish.spam.bypass` by default—to trusted roles only.

## Configured chat filter

For each enabled rule, the service can scan:

1. **Plain** — Unicode NFKC normalization, lowercase conversion, color-code removal, leetspeak mapping, and normalized spacing.
2. **Joined** — normalized token candidates and combined runs of short tokens, useful for spaced bypasses.
3. **Squeezed** — joined candidates with repeated characters collapsed.

The first matching rule cancels the message, stores a flag, tells the player that the message was blocked, and alerts the console and configured notification permission.

Rules provide:

- a reason key that can reuse a configured mute reason;
- a fallback display reason and duration;
- separate regular expressions for each scan view.

!!! warning
    Regex changes can create false positives or expensive matches. Test rules outside production and keep the review queue staffed.

## Review queue

```text
/chatreview
/chatreview 2
```

Open flags are stored in `data/chatfilter-flags.yml`. The 21-entry queue can be sorted newest-first, oldest-first, or by longest expected mute.

| Control | Result |
| --- | --- |
| Left-click a flag | Dismiss it. |
| Right-click a flag | Apply the rule's configured or fallback mute and mark the flag reviewed. |
| Shift-click a flag | Open its detail screen. |
| Detail: Mute Player | Apply the expected mute. |
| Detail: Dismiss Flag | Close without a punishment. |

The detail view shows the original blocked message, matched sample, detection layer, rule, suggested reason, fallback duration, and expected mute duration.

## Global chat controls

- `/mutechat [reason]` toggles an in-memory global mute. Staff and `betterpunish.chat.bypass` bypass it.
- `/slowmode <seconds|off>` applies an in-memory per-player cooldown. The same bypass rules apply.
- `/clearchat` sends 100 blank lines to regular players; staff and `betterpunish.clearchat.bypass` retain their view unless `all` or `-a` is specified.

These global states are not persisted across restart.

## Mutes and shadowmutes

A regular mute blocks Paper chat, recognized private-message commands, signs, books, and non-empty anvil renames. A shadowmute cancels the same paths while echoing chat/private-message content to the sender and alerting the console and `betterpunish.chatfilter.notify` viewers.

Shadow activity can contain private conversation text. Treat notification access as highly sensitive.
