<p align="right">🌐 <a href="../sync-and-safety.md">简体中文</a> · <strong>English</strong></p>

# Sync, Data Safety, and Conflicts

[Back to English home](../../README.en.md) · [Quick Start](quick-start.md)

Aster normally edits its app-local workspace and synchronizes it through the selected provider. Agenda, Journal, Files, Search, widgets, and notification indexes are rebuildable projections. The data that must be protected is the Org source plus its Org Attach files.

## Supported Workspace Providers

| Provider | Connection | Notes |
| --- | --- | --- |
| Dropbox | OAuth authorization and a selected remote directory | Includes file version history |
| iCloud Drive | System directory picker | Uses security-scoped access to the selected directory |
| Nutstore | Account, third-party app password, and workspace directory | Uses Nutstore WebDAV |
| WebDAV | Server URL with optional credentials | Suitable for Nextcloud and other standard servers |

Nutstore requires a third-party app password, not the account login password. For WebDAV, provide both username and password or leave both blank for an anonymous server. Credentials are stored in the system Keychain.

Even when a WebDAV server lists a nested file such as `agenda/work/review.org` without separate records for every collection, Aster restores the proven `agenda/` and `agenda/work/` parents from that path. A directory visible in Files should therefore also be available to the Agenda Sources, Journal folder, and Capture Inbox path pickers.

## How a Normal Save Flows

1. Aster writes a complete, valid Org edit to the local file.
2. Parsing and UI projections refresh for the affected paths.
3. The sync engine compares local signatures with remote revisions.
4. Upload, download, or deletion completion records a checkpoint for that path.
5. Another client sees the same ordinary file.

A successful UI update must not be an in-memory-only record. Conversely, a temporary network failure should not undo source that was already saved locally.

After Files → Org source editor saves a document inside an Agenda source, Agenda, TODOs, Perspectives, Search, widgets, and reminders refresh incrementally from that local path. There is no need to wait for upload, switch workspaces, or relaunch. Cloud sync moves the saved source to other clients; it is not the trigger for refreshing local projections. Unsaved text entered while that refresh finishes is also preserved instead of being replaced by an earlier parsed revision.

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
- Record the relative paths for Agenda Sources, Journal folder, and Capture Inbox.
- Distinguish “remove the local mirror” from “delete remote content,” and read confirmation text carefully.
- After reconnecting, verify the role paths. Seeing some items on the home screen does not prove the complete directory is synchronized.

## Privacy in Public Reports

Do not upload:

- Dropbox, Google, or WebDAV tokens and authorization callbacks.
- WebDAV or Nutstore passwords.
- A complete private workspace or unredacted screenshots.
- Names, email addresses, absolute local paths, or original attachments.

Whenever possible, reproduce the problem in a test file containing only a few Org lines. If the data cannot be disclosed, describe the structure and behavior without posting it to a public GitHub issue.
