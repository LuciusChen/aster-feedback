<p align="right">🌐 <a href="../sync-and-safety.md">简体中文</a> · <strong>English</strong></p>

# Sync, Data Safety, and Conflicts

[Back to English home](../../README.en.md) · [Quick Start](quick-start.md)

Aster normally edits its app-local workspace and synchronizes it through the selected provider. Agenda, Journal, Files, Search, widgets, and notification indexes are rebuildable projections. The data that must be protected is the Org source plus its Org Attach files.

## Supported Workspace Providers

| Provider | iOS/iPadOS | Android | Connection and notes |
| --- | --- | --- | --- |
| Dropbox | Supported | Supported | OAuth authorization and remote directory selection; includes file version history |
| iCloud Drive | Supported | Not supported | Apple platforms use the system directory picker and security-scoped access |
| Nutstore | Supported | Supported | Account, third-party app password, and workspace directory through Nutstore WebDAV |
| WebDAV | Supported | Supported | HTTPS server URL with optional credentials; suitable for Nextcloud and other standard servers |

Android does not provide iCloud Drive and does not expose the private app working copy or generic local Import as another workspace provider.

Nutstore requires a third-party app password, not the account login password. Generic WebDAV accepts only an HTTPS workspace URL with a host and no embedded credentials; an HTTP URL is neither saved nor connected. Provide both username and password or leave both blank for an anonymous server. Passwords are stored in the system Keychain on iOS/iPadOS and protected by Android Keystore on Android; Basic Auth is sent only over TLS.

Even when a WebDAV server lists a nested file such as `agenda/work/review.org` without separate records for every collection, Aster restores the proven `agenda/` and `agenda/work/` parents from that path. A directory visible in Files should therefore also be available to the Agenda Sources, Journal folder, and Event & Task Inbox path pickers.

## How a Normal Save Flows

1. Aster writes a complete, valid Org edit to the local file.
2. Parsing and UI projections refresh for the affected paths.
3. The sync engine compares local signatures with remote revisions.
4. Upload, download, or deletion completion records a checkpoint for that path.
5. Another client sees the same ordinary file.

A successful UI update must not be an in-memory-only record. Conversely, a temporary network failure should not undo source that was already saved locally.

After Files → Org source editor saves a document inside an Agenda source, Agenda, TODOs, Perspectives, Search, widgets, and reminders refresh incrementally from that local path. There is no need to wait for upload, switch workspaces, or relaunch. Cloud sync moves the saved source to other clients; it is not the trigger for refreshing local projections. Unsaved text entered while that refresh finishes is also preserved instead of being replaced by an earlier parsed revision.

### Next iOS/iPadOS Sync Safeguards (Unreleased)

Uploads and sync deletions check the remote version that was compared. A changed version stops the operation instead of being overwritten. WebDAV login pages, incomplete listings, and out-of-scope resources are errors, not evidence of an empty remote directory.

Sync replacement or automatic deletion of an existing WebDAV file requires a strong ETag from the server. Missing reliable version information causes an explicit error, never an unconditional-write fallback. Explicit deletion from Files remains a separate user action. iCloud writes, moves, and deletions use system file coordination.

If local source changes after a conflict comparison opens, the old choice is rejected and the comparison refreshes to the latest local content. Review it again before choosing; an old snapshot must not overwrite later edits.

## Apple Reminders Interoperability File (iOS/iPadOS only)

After Apple Reminders is explicitly enabled in Settings, Aster synchronizes once the workspace has been validated and again whenever the app returns to the foreground. These automatic triggers never request access by themselves. Settings shows the current state beside the switch.

Aster maps system reminders only through the separate managed `apple-reminders.org` file at the workspace root. This file appears automatically in Agenda and TODOs; you do not need to select the workspace root as an Agenda source.

A system Reminder's due value always maps to Org `DEADLINE`. It never maps to `SCHEDULED`, which retains its meaning as the date when work is planned to start. A timed Reminder preserves its exact clock time, while a date-only Reminder stays date-only.

A simple unbounded daily, weekly, monthly, or yearly recurrence appears as a cadence-preserving Org `++` repeater. Recurrences with an end or more complex selectors remain system-owned instead of being reduced to an inaccurate Org rule.

When you complete a synchronized repeating item, Apple Reminders advances the recurrence and Aster imports the next item. Both sides never advance it independently.

Once synchronized, managed and ordinary Org items share Aster's presentation rules. Overdue `DEADLINE` and `SCHEDULED` items show the same `Overdue`, `1 day overdue`, or `N days overdue` status; the calculation does not change merely because Apple Reminders supplied the item.

Ordinary workspace Org tasks are never exported to the Reminders app. However, every concretely timed item available to Aster remains eligible for Aster Notifications, including one imported from Apple Reminders. Its notification subtitle includes the localized due date and time.

On iPhone and iPad, **Reminder Badge** is enabled by default under Aster Notifications and can be turned off independently. It shows the number of unfinished timed tasks whose effective reminder time has arrived. Each task counts once; calendar events and repeated follow-up alerts do not inflate the number, and turning the option off clears the app-icon badge.

If system notifications for the Reminders app are also enabled, both apps may notify you. This is intentional.

Only a managed Org heading with no Reminder identifier can create a new system Reminder. An invalidated identifier does not authorize creating another reminder with the same title.

The first synchronization may create the managed file when it is genuinely absent. If an existing file cannot be read, is not valid UTF-8, or EventKit returns no reminder list, synchronization stops with an error. Aster never treats that failure as an empty or older source.

A local edit made during synchronization is also preserved. Aster reports that the source changed instead of replacing it with an earlier generated result.

### Per-Item Comparison and Standard Org Output (Next iOS/iPadOS, Unreleased)

Aster compares each Reminder ID's local fields, system fields, and last successful baseline. It no longer treats the entire file's modification time as every item's edit time. Local-only changes export, system-only changes import, and unchanged content is not rewritten.

When the managed file is unchanged, that pass does not reload the workspace or schedule an extra cloud sync, including when only the device-local baseline changes. Normal foreground cloud checks still run.

Completing, reopening, or otherwise editing a managed item in Aster requests an Apple Reminders pass immediately when Full Access is available. If a pass is already running, Aster keeps one follow-up request instead of making you leave and reopen the app.

Divergent edits to the same item after the baseline exists stop sync and identify the reminder to review. Compare that item in `apple-reminders.org` and the Reminders app, make the fields you want to keep agree, and sync again; repeated retries alone cannot resolve it.

The first sync on a device may find the managed Org file but no private per-item baseline. IDs present in the current Apple Reminders snapshot then import that system state—including system-side completion—to establish their baseline; they are not treated as concurrent edits.

An ID missing from that first snapshot requires review rather than silent deletion. Only a heading without an ID creates a system Reminder. After the baseline exists, an unchanged local item may follow the system snapshot while a locally edited missing item is preserved for review. The baseline is device-local sync metadata, not another editable task file.

Generated files declare their own `TODO | DONE` workflow and place planning before the property drawer. This example illustrates the structure; identifiers are generated by sync, so do not copy the example ID into a real file:

```org
#+TODO: TODO | DONE
* TODO Review notes
DEADLINE: <2026-09-05 15:00>
:PROPERTIES:
:APPLE_REMINDER_ID: example-only
:APPLE_REMINDER_LIST: Reminders
:END:
```

## Android Notification Boundary

The Android build does not create `apple-reminders.org` and does not import or export ordinary Org Tasks through an invented Apple Reminders equivalent. **Aster Notifications** rebuilds local reminder plans from concretely timed items in the current Agenda sources. On Android 13 and later, Aster requests notification permission only after the user explicitly enables notifications in Settings.

Complete and Snooze actions return through the same workspace write boundary instead of changing a notification-only cache. Android uses the system's permitted inexact alarm capability and does not request exact-alarm access. Uninstalling, clearing app data, or disconnecting the workspace affects local notification plans but does not modify cloud Org files.

## Conflicts Are Not Silently Overwritten

If the same file changed locally and remotely since the last checkpoint, Aster preserves two copies and requires an explicit resolution instead of guessing which one is newer. New sync attempts and Dropbox version restoration may pause until conflicts are resolved.

Before resolving:

1. Read both source copies, especially recently edited headlines, attachment links, and property drawers.
2. Choose which copy should become current.
3. If necessary, merge them manually in another editor before syncing again.
4. Do not repeatedly retry or delete files before understanding the conflict.

## Interpreting Errors

- **Interrupted network, timeout, or temporary service failure**: retry on a stable connection; the local files remain available.
- **Failed or expired authorization**: only this class of error requires reconnecting in Settings.
- **Dropbox rate limit**: wait before syncing again; repeated reconnection will not help.
- **Remote-only file**: allow its download to finish before previewing or moving it.
- **Unresolved conflict**: resolve it before continuing sync or restoring a version.

Ordinary transient network failures should not all recommend “Reconnect.” If that suggestion still appears repeatedly for the wrong error class, report the build, network state, and exact time it occurs.

## Dropbox Version History

Hold a Dropbox-backed file to inspect its remote history, preview text or image revisions, and confirm a restore. Retention is controlled by the Dropbox plan; Aster displays up to the latest 100 revisions.

Before restoring:

- Current sync must be complete.
- No unresolved conflict may remain.
- The revision must still exist at the current Dropbox path and within its retention window.

Restored bytes return through normal download, parsing, and UI refresh. Aster does not create a proprietary history store.

## Sync Is Not Backup

Sync propagates correct edits, but it also propagates deletions, mistakes, and chosen conflict resolutions. Keep at least one backup outside real-time sync:

```text
workspace/
├── agenda/
│   ├── inbox.org
│   └── data/...
├── journal/
│   └── ...
└── notes/
    ├── research.org
    └── data/...
```

Backing up only `.org` files while omitting adjacent `data/` directories loses Org Attach files.

## Before Disconnecting a Workspace

- Confirm that the latest changes exist in the cloud or an independent backup.
- Record the relative paths for Agenda Sources, Journal folder, and Event & Task Inbox.
- Distinguish “remove the local mirror” from “delete remote content,” and read confirmation text carefully.
- After reconnecting, verify the role paths. Seeing some items on the home screen does not prove the complete directory is synchronized.

## Privacy in Public Reports

Do not upload:

- Dropbox, Google, or WebDAV tokens and authorization callbacks.
- WebDAV or Nutstore passwords.
- A complete private workspace or unredacted screenshots.
- Names, email addresses, absolute local paths, or original attachments.

Whenever possible, reproduce the problem in a test file containing only a few Org lines. If the data cannot be disclosed, describe the structure and behavior without posting it to a public GitHub issue.
