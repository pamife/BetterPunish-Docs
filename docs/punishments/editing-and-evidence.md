# Editing and evidence

## Edit a punishment

Use `/punishedit <id>` in game to open the edit GUI, or provide a property and value from either game or console:

```text
/punishedit BAN-000184 reason Client modifications
/punishedit BAN-000184 duration 30d
/punishedit BAN-000184 staff ModeratorName
```

| Property | Supported records | Validation |
| --- | --- | --- |
| Reason | All types | Required; maximum 180 characters; line breaks are replaced with spaces. |
| Duration | Ban, mute, shadowmute | A valid duration or `permanent`; maximum 64 input characters. |
| Staff | All types | Required; maximum 64 characters; line breaks are replaced with spaces. |

Duration edits set the **total duration from the original creation timestamp**. They do not add time from the moment of editing. A duration that already ends in the past makes the record expired; a longer duration can make an expired, non-revoked record active again. Revoked records remain revoked.

## GUI and chat input

The edit GUI offers reason, duration where applicable, moderator, proofs, and back-to-details actions. Selecting a text property closes the inventory and waits for the next chat message.

- Type the new value to apply it.
- Type `cancel` to return without a change.
- The session expires after `gui.input-timeout-seconds` (minimum 10 seconds).
- Invalid input returns the moderator to the edit screen.
- Permission is checked again when the chat value is processed.
- Disconnecting cancels the session silently.

Every successful change appends an edit-history entry containing the property, old and new values, editor, and timestamp. Local YAML is saved immediately; configured Supabase records receive a PATCH for the changed field.

## Active, expired, and revoked records

The editor does not restrict by status:

- active records can be changed normally;
- expired records can have metadata or duration changed;
- revoked records retain their revoked state even when their duration changes.

Changing a regular mute or shadowmute duration triggers a mute-group reconciliation. The LuckPerms group reflects active regular mutes only.

## Evidence references

```text
/proof add BAN-000184 https://evidence.example/case-184
/proof list BAN-000184
/proof remove BAN-000184 1
```

Proofs are strings stored on the punishment. BetterPunish extracts the first URL-like value, adds `https://` to a dotted value without a scheme, and trims common trailing punctuation. Removal accepts a one-based list index or exact stored value.

!!! warning
    BetterPunish does not validate ownership, availability, safety, or confidentiality of evidence links and does not upload evidence files. Use an access-controlled evidence store and avoid public links to private player information.
