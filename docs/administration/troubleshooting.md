# Troubleshooting

## Plugin does not start

1. Confirm the server is a [supported Paper version](../getting-started/compatibility.md).
2. Confirm the runtime is Java 21 for Paper 1.21.11 or Java 25 for Paper 26.1 and newer.
3. Read the first BetterPunish exception in the server console.
4. Check that the server process can create `plugins/BetterPunish/data/`.
5. Restore the last known-good `config.yml` if YAML parsing fails.

The current build uses Paper-specific APIs and should not be expected to load on a Spigot-only server.

## A command says there is no permission

- Check the exact node in the [permissions reference](../reference/permissions.md).
- Do not assume `betterpunish.*` includes later moderation nodes; grant them explicitly.
- Remember that the command descriptor permission is checked before the Java handler.
- Run the command as an operator only as a temporary diagnostic step.

## A GUI does not open

- Dashboard, reports, chat review, suspects, and edit/profile GUIs require an in-game player.
- Confirm the command permission.
- For `/punishedit <id>`, verify that the ID exists.
- For `/pinfo`, the player must be online, known to Bukkit, or present in punishment history.
- Check the console for an inventory or material error caused by an incompatible server build.

## A punishment is missing

- Search by exact record ID with `/checkban <id>`.
- Use `/history <player>` and the dashboard's Recent, Expired, and Revoked views.
- A newer punishment of the same type automatically revokes the older one.
- Confirm that a recent `/punishpull` did not replace local history.
- Inspect the protected backup of `data/punishments.yml`; do not publish it.

## A mute is not reflected in LuckPerms

- Confirm LuckPerms is installed and enabled.
- Check `mute-group-sync.enabled` and the exact group name.
- Create that group in LuckPerms.
- Run `/punishreload` and read the console warning.
- Remember that only regular mutes drive the group; shadowmutes do not.

## Chat filtering or anti-spam behaves unexpectedly

- Run `/punishreload` after editing the rule set.
- Check explicit bypass permissions.
- Test plain, joined, and squeezed forms separately.
- Look in `/chatreview` before changing a duration.
- If duplicate-message warnings appear with no `anti-spam` block, add the block and set `enabled: false`; the code defaults it to enabled.

## Supabase requests fail

- Replace both placeholders with the project URL and anon key.
- Verify all four tables and required columns.
- Confirm grants and RLS policies permit the exact REST operation.
- Check server DNS, TLS, and outbound HTTP access.
- Read the HTTP status warning in the console.
- Keep local data as the recovery source until a staging migration and pull both succeed.

## Discord notifications do not arrive

- `discord.bot-url` must point to a companion HTTP service, not a Discord webhook.
- The service must accept `POST /broadcast-punishment`.
- The shipped local companion value deliberately disables the integration.
- Use HTTPS or a trusted private network.
- The plugin currently suppresses request exceptions, so inspect the companion service logs as well as server connectivity.

## Freeze support link is wrong

Replace both `discord.invite-url` and `discord.display`. The shipped values are placeholders, not a real support server. Run `/punishreload` or restart.

## Storage cannot be saved

- Confirm filesystem permissions and free disk space.
- Check whether antivirus or backup software is locking a YAML file.
- Stop the server cleanly before restoring a backup.
- If a legacy file remains in the plugin root, check the console for a failed move into `data/`.
