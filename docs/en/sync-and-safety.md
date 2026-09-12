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

## Choose Subfolders to Sync (iOS/iPadOS, build 16)

Open **Settings → Workspace & Sync → Workspace root**. All four providers share the same browser: tap a folder to enter it, then use native Back or swipe back to its parent. Separate checkmarks choose what syncs. Browsing never changes the root, and there is no Parent Folder row.

Everything starts selected; a dash means partial selection. Changing a parent applies to its subtree, with later child overrides. New folders inherit their parent's choice; root files always sync. Dropbox's Include subfolders is on this page and must be enabled for individual choices.

Dropbox paths are case-insensitive, so `Journal` and `journal` appear as one choice and are excluded together. Other providers retain separate case-sensitive choices. This does not rename files.

Save can stage folder choices for the next run while a transfer is in progress. Changing the root or Include subfolders waits for that transfer to finish. Root changes retain each provider's existing flow: the Dropbox account browser, iCloud system picker, or Nutstore/WebDAV connection settings.

Save applies the selection to the next sync. Unchecking never deletes existing local or cloud files or changes Agenda and Journal sources. Existing local content remains available. Re-enabling compares against the retained sync checkpoints; edits made on both sides still produce a conflict instead of an overwrite.

Choices belong to this device. First connection retains the default full sync. Browsing the selection tree reads metadata without downloading content. Keep `data/` and other required attachment directories selected alongside their Org files so images remain available on other devices.

Explicitly confirmed create, move and delete actions in Files remain separate cloud operations, not protected by this automatic transfer policy. Re-enable an excluded cloud-only file's folder and sync before previewing it. Apple Reminders interoperability is configured separately.

## How a Normal Save Flows

1. Aster writes a complete, valid Org edit to the local file.
2. Parsing and UI projections refresh for the affected paths.
3. The sync engine compares local signatures with remote revisions.
4. Upload, download, or deletion completion records a checkpoint for that path.
5. Another client sees the same ordinary file.

A successful UI update must not be an in-memory-only record. Conversely, a temporary network failure should not undo source that was already saved locally.

After Files → Org source editor saves a document inside an Agenda source, Agenda, TODOs, Perspectives, Search, widgets, and reminders refresh incrementally from that local path. There is no need to wait for upload, switch workspaces, or relaunch. Cloud sync moves the saved source to other clients; it is not the trigger for refreshing local projections. Unsaved text entered while that refresh finishes is also preserved instead of being replaced by an earlier parsed revision.

### iOS/iPadOS Sync Safeguards (build 12)

If a local directory cannot be read completely, synchronization stops. Files omitted by that failed read are not treated as deletions on the server. Restore access and retry; do not delete cloud files to resolve a local read failure.

Source editors also check the opened version so unsaved text cannot overwrite a newly downloaded version. See [save conflict protection](files-preview-attachments.md#save-conflict-protection-iosipados-build-12) for retaining edits and explicitly reloading saved content.

WebDAV lists folders one level at a time instead of requiring unlimited-depth listing. This addresses services such as InfiniCLOUD that do not support infinite-depth requests. The failure and fix were exercised against a real local Apache WebDAV service; the customer's InfiniCLOUD account has not been tested.

HTTP 401 means the server rejected the credentials, including an incorrect password on the first connection. Check the connection URL, username, and app password in Settings, then use Save & Sync; you do not need to delete local files. HTTP 403 instead points to folder permissions and server restrictions, without repeatedly requesting authorization.

Files keeps **Sync status** and **Last successful sync** beneath the provider name, including when the file tree is collapsed. Workspace & Sync shows the same information below the connection fields. An unavailable service shows paused/retry guidance; the green Configured label only means the connection details are saved.

Changing the Journal or Inbox location and importing fonts do not replace sync status. Saved file changes show **Local edits pending sync**, unless a transfer or paused/error/conflict message needs to remain visible.

The timestamp includes the full local date and time and survives relaunch. It advances after an entire conflict-free run succeeds, including a check with no file changes. Failure, interruption and conflicts leave the previous successful time intact. A connection without a successful run shows **Never synced**.

Each connection has its own record, so a different account or folder cannot inherit another connection's time. Disconnecting clears that provider's history on this device; reconnecting starts without a time until the next successful run. This is not a server file-modification date or proof that every other device has received the files.

WebDAV bypasses the system HTTP cache so an earlier read cannot interfere with deleting an opened or moved file. Sync still checks remote versions and never automatically overwrites a genuine conflict.

For InfiniCLOUD, use the WebDAV endpoint and credentials provided by My Page after enabling external-app connections, not the website's login URL.

Uploads and sync deletions check the remote version that was compared. A changed version stops the operation instead of being overwritten. WebDAV login pages, incomplete listings, and out-of-scope resources are errors, not evidence of an empty remote directory.

Sync replacement or automatic deletion of an existing WebDAV file requires a strong ETag from the server. Missing reliable version information causes an explicit error, never an unconditional-write fallback. Explicit deletion from Files remains a separate user action. iCloud writes, moves, and deletions use system file coordination.

If local source changes after a conflict comparison opens, the old choice is rejected and the comparison refreshes to the latest local content. Review it again before choosing; an old snapshot must not overwrite later edits.

### Verifying Upload Results (iOS/iPadOS, build 15)

Some WebDAV servers omit the version header after a successful upload. Aster reads the file back, checks that the bytes match the upload exactly, and records the version associated with those bytes. If another client has already changed the file, synchronization stops instead of treating that client's version as Aster's upload.

This verification does not relax overwrite protection. A weak ETag can identify a read snapshot but cannot authorize replacement or automatic deletion.

### Nutstore Repeated Saves and Deletion Protection (iOS/iPadOS, build 16)

Saving again after the first upload no longer produces a false version conflict. Existing files still use version checks. New files are published from private staging with a no-overwrite move, protecting same-named files created concurrently by another device. This has been exercised against a real Nutstore account; other WebDAV services keep their standard requests.

Nutstore does not enforce version conditions on deletion. When sync detects a missing local file, Aster keeps the remote copy and shows **Deletion needs confirmation**, without recording a successful deletion. Inspect its contents on Nutstore, then delete it there or long-press its Files row and confirm Delete File. Cloud-only files need no download before this explicit deletion; Cancel leaves the file intact, and repeated sync attempts do not force deletion.

Private staging is excluded from Files and document synchronization. An interrupted upload can leave temporary data; Aster does not sweep directories belonging to other attempts. This limitation concerns file-sync deletion, not edits to Org headings or deletion of individual Apple Reminders.

After a new file is published, failure to clean up its empty staging directory does not invalidate the upload. Aster still checks the final content and version before recording success, so cleanup failure does not turn the next edit into a false conflict.

## Apple Reminders Interoperability File (iOS/iPadOS only)

After Apple Reminders is explicitly enabled in Settings, Aster synchronizes once the workspace has been validated and again whenever the app returns to the foreground. These automatic triggers never request access by themselves. Settings shows the current state beside the switch.

Aster maps system reminders only through the separate managed `apple-reminders.org` file at the workspace root. This file appears automatically in Agenda and TODOs; you do not need to select the workspace root as an Agenda source.

A system Reminder's due value always maps to Org `DEADLINE`. It never maps to `SCHEDULED`, which retains its meaning as the date when work is planned to start. A timed Reminder preserves its exact clock time, while a date-only Reminder stays date-only.

iOS/iPadOS build 14 preserves a system reminder's explicit time zone. Managed Org stores the provider clock and zone properties, while Aster displays local time. Editing its title, completion state, or date exports the correct instant. Floating reminders stay local, and date-only values do not move to another day when the zone changes.

For example, September 8, 2026 at 15:00 in New York displays as September 9 at 03:00 in Shanghai. See [Time Zones and Travel](org-and-aster.md#16-time-zones-and-travel-iosipados-build-14) for source examples and the limitation in other Org readers.

A simple unbounded daily, weekly, monthly, or yearly recurrence appears as a cadence-preserving Org `++` repeater. Recurrences with an end or more complex selectors remain system-owned instead of being reduced to an inaccurate Org rule.

When you complete a synchronized repeating item, Apple Reminders advances the recurrence and Aster imports the next item. Both sides never advance it independently.

Once synchronized, managed and ordinary Org items share Aster's presentation rules. Overdue `DEADLINE` and `SCHEDULED` items show the same `Overdue`, `1 day overdue`, or `N days overdue` status; the calculation does not change merely because Apple Reminders supplied the item.

Ordinary workspace Org tasks are never exported to the Reminders app. However, every concretely timed item available to Aster remains eligible for Aster Notifications, including one imported from Apple Reminders. Its notification subtitle includes the localized due date and time.

On iPhone and iPad, **Reminder Badge** is enabled by default under Aster Notifications and can be turned off independently. It shows the number of unfinished timed tasks whose effective reminder time has arrived. Each task counts once; calendar events and repeated follow-up alerts do not inflate the number, and turning the option off clears the app-icon badge.

An ordinary timed task remains counted after it becomes overdue, until it is completed. A timed Habit contributes only from its reminder time until midnight on that day, so yesterday's Habit does not remain on the badge. Aster recalculates the number when Agenda crosses midnight in the foreground or the app is reopened on a later day.

If system notifications for the Reminders app are also enabled, both apps may notify you. This is intentional.

Only a managed Org heading without `APPLE_REMINDER_ID` and without an existing system match can create a new reminder. An invalidated identifier does not authorize creating another reminder with the same title.

The first synchronization may create the managed file when it is genuinely absent. If an existing file cannot be read, is not valid UTF-8, or EventKit returns no reminder list, synchronization stops with an error. Aster never treats that failure as an empty or older source.

A local edit made during synchronization is also preserved. Aster reports that the source changed instead of replacing it with an earlier generated result.

### Per-Item Comparison and Standard Org Output (iOS/iPadOS build 11 and later)

Aster compares each Reminder ID's local fields, system fields, and last successful baseline. It no longer treats the entire file's modification time as every item's edit time. Local-only changes export, system-only changes import, and unchanged content is not rewritten.

When the managed file is unchanged, that pass does not reload the workspace or schedule an extra cloud sync, including when only the device-local baseline changes. Normal foreground cloud checks still run.

Completing, reopening, editing, or deleting a managed item in Aster requests an Apple Reminders pass immediately when Full Access is available. If a pass is already running, Aster keeps one follow-up request instead of making you leave and reopen the app.

iOS/iPadOS build 15 fixes deleted items returning on import. Removing a synchronized heading from `apple-reminders.org` also removes its matching Apple Reminder, including completed or repeating reminders. Other reminders with the same title but different IDs are untouched. Ordinary Org files do not participate in this system sync.

If the system copy changed during deletion, Aster asks you to review the conflict instead of erasing that edit. Failed syncs can be retried using the existing baseline; you do not need to delete the system copy separately. Do not delete the entire managed file to clear Reminders: an absent file or missing first-sync baseline never authorizes bulk system deletion.

Divergent edits to the same item after the baseline exists stop sync and identify the reminder to review. Compare that item in `apple-reminders.org` and the Reminders app, make the fields you want to keep agree, and sync again; repeated retries alone cannot resolve it.

The first sync on a device may find the managed Org file but no private per-item baseline. IDs present in the current Apple Reminders snapshot then import that system state—including system-side completion—to establish their baseline; they are not treated as concurrent edits.

An existing system ID missing from that first snapshot requires review, not silent deletion or a replacement reminder. After the baseline exists, an unchanged local item may follow the system snapshot while a locally edited missing item is preserved for review. The baseline is device-local sync metadata, not another editable task file.

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

### Interrupted Creation and Literal Notes (iOS/iPadOS, build 15)

Before creating a system reminder, Aster saves a standard Org `:ID:` and a comparison baseline for the pending creation. The new reminder's URL carries the matching Aster link. If system creation succeeds but a later read or local save fails, a retry finds that same reminder by ID instead of guessing from its title or creating a duplicate. Recovery also works after restarting Aster.

Later edits on both sides still undergo comparison. Genuine conflicts require review, and historical duplicates are not automatically deleted. Recovery depends on retaining the Org ID and the Aster link in the system reminder; it does not guarantee exactly-once creation when two devices first create the same item simultaneously.

Org-like directives, drawers, and headings inside system notes stay literal instead of disappearing as metadata. Generated notes use standard Org fixed-width text, as in these two note lines. Readers retain their literal content; exporting to Reminders removes the leading `: ` prefix.

```org
: #+TITLE: This is a note, not a file directive
: * This is not another task
```

Trailing tags and progress text in titles, along with CRLF line endings in notes, are retained. Completing such an item should not produce a false “both copies changed” conflict solely because of text conversion.

The system title excludes Org workflow and priority prefixes whether separated by spaces or tabs. Changing only those separators is not a reminder edit; text within the remaining title stays unchanged.

Timestamped notes added through Add Note live in that item's Org `LOGBOOK`. In iOS/iPadOS build 15, sync preserves those notes and state history by item ID, even after a system-side title edit; it does not merge them into Apple Reminders' ordinary notes field.

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
