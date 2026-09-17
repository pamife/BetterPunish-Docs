# Reports

Players with `betterpunish.report` can report another online player:

```text
/report Steve Combat logging near spawn
```

The target must be online. A successful report is saved locally, announced to the console and players with `betterpunish.chatfilter.notify`, sent to the configured Discord companion endpoint, and synchronized to Supabase when enabled.

## Reports queue

```text
/reports
/reports 2
```

The GUI contains all reports whose status is not `RESOLVED`, with 28 entries per page. Each entry shows the target, reporter, reason, time, status, and claiming moderator.

| Control | Result |
| --- | --- |
| Left-click | Teleport to the reported player when they are online. |
| Right-click | Change an `OPEN` report to `CLAIMED` and record the moderator. |
| Shift + right-click | Change a non-resolved report to `RESOLVED`. |
| Page arrows | Move between queue pages. |

Claimed reports remain visible until resolved. A report cannot be claimed twice, but any non-resolved report can be resolved.

!!! note "Known control limitation"
    The current GUI also displays a middle-click hint, but that path invokes an unregistered `/punish` command. It is intentionally not included as a working moderation action here.

## Storage

Reports are saved to `plugins/BetterPunish/data/reports.yml`. The local report ID counter is independent of punishment IDs. Supabase synchronization inserts new reports and patches status changes.

Reports can contain player names and free-form allegations. Limit access and establish a retention policy.
