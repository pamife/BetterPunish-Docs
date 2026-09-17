# Storage and migration

## Local files

BetterPunish creates a `data/` directory inside its plugin folder:

| File | Contents |
| --- | --- |
| `punishments.yml` | Punishment history, pending warnings, global numeric ID counter, proofs, and edit history. |
| `notes.yml` | Staff notes and note ID counter. |
| `chatfilter-flags.yml` | Open and reviewed chat-filter flags. |
| `command-logs.yml` | Stored player commands and recognized private-message commands. |
| `player_ips.yml` | Raw IP-to-account associations used for linked-account detection. |
| `reports.yml` | Report queue and report ID counter. |
| `offenses.log` | Append-only offense command summaries. |

On startup, each storage service moves a same-named legacy file from the plugin root into `data/` when the new path does not already exist.

Most YAML saves are asynchronous through a single-thread executor. Shutdown waits briefly for punishment, report, chat-filter, command-log, and player-IP data before a final save. Keep reliable filesystem backups.

## In-memory state

These values do not survive restart:

- current freezes;
- global chat mute and its reason;
- slow-mode setting and cooldown timestamps;
- live log subscriptions;
- Vulcan flag counts, general kick counts, and alert cooldowns;
- active GUI chat-input sessions.

## Supabase migration and pull

`/punishmigrate` uploads core punishment rows. `/punishpull` replaces local punishment history from cloud rows. Neither command migrates every local file.

Recommended process:

1. Stop or restrict moderation changes.
2. Back up the entire `plugins/BetterPunish/` directory.
3. Verify the Supabase schema and policies in a staging project.
4. Run `/punishmigrate`.
5. Check the server console and remote row counts.
6. Test `/punishpull` on a copied server before using it in production.

## Clearing punishment data

```text
/punishcleardb
```

This immediately clears local punishment history and pending warnings, resets the local ID counter, and saves synchronously. If Supabase is configured, it separately sends asynchronous deletes for every `punishments` and `pending_warnings` row.

It does not clear reports, notes, IP associations, chat flags, command logs, or offense log lines.

!!! danger
    There is no confirmation prompt and no automatic backup. Restrict `betterpunish.cleardb` and back up before use.
