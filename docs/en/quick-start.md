<p align="right">🌐 <a href="../quick-start.md">简体中文</a> · <strong>English</strong></p>

# Quick Start

[Back to English home](../../README.en.md)

## 1. Before You Test

- Use iOS or iPadOS 17 or later.
- Back up the Org workspace you plan to connect.
- If you use Org Attach, include the adjacent `data/` directories in that backup.
- Start with a separate test folder. Connect your main workspace only after sync and write-back behave as expected.

## 2. Connect a Cloud Workspace

Open **Settings → Cloud Workspace** and choose a provider:

- Dropbox
- iCloud Drive
- Nutstore
- WebDAV

Choose a workspace root and whether nested directories are included. Aster mirrors the cloud content into its local workspace, then builds Agenda, TODOs, Journal, Files, and Search indexes from those real files.

After connecting, open **Files** and verify:

1. The directory hierarchy is complete.
2. `.org` files open in Preview and Edit.
3. Images, PDFs, and other document types supported by the device can open.
4. A remote-only file opens automatically after its download completes.

## 3. Configure Three Workspace Roles

The workspace root answers “where are the files?” The following three settings assign semantic roles and always show the selected relative path:

| Setting | Purpose | Example |
| --- | --- | --- |
| Agenda Sources | Org files included in Agenda, TODOs, Perspectives, and reminder indexing | `agenda/` |
| Journal folder | Location used to read and create daily Journal files | `denote/daily/` |
| Event & Task Inbox | Default Org file for new Events and Tasks | `agenda/inbox.org` |

These paths may belong to any connected workspace root. Agenda Sources can also select **Workspace root** directly: with **Include subfolders** off, only root-level Org files are indexed; turning it on indexes that root recursively. Fixed directory names are not required.

## 4. Confirm the Workflow

Open **Settings → Tasks & Workflow** and make sure the Process and Terminal keywords match your files.

For example:

```org
#+TODO: TODO NEXT WAITING PROJECT | DONE CANCELLED
```

`TODO`, `NEXT`, `WAITING`, and `PROJECT` are active states; `DONE` and `CANCELLED` are terminal states. Aster preserves each full keyword and lets you configure its icon, color, and whether it should be treated as a Project.

If a file declares `#+TODO:` or `#+SEQ_TODO:`, the file-local declaration takes precedence over global settings.

## 5. Create Your First Item

The root command bar has three parts:

- The primary tabs are **Views, Files, Settings**.
- The leading three-line button changes the current child. Under Views it contains Agenda, TODOs, Journal, and saved Perspectives. Under Files it contains Workspace, role folders, and folders you pinned from the file tree. Under Settings it contains Workspace & Sync, Appearance, Calendar & Agenda, Tasks & Workflow, Create & Storage, and Journal; About Aster remains on the Settings home. Choosing one switches the existing iPad detail or directly opens that category on iPhone without stacking repeated navigation layers.
- The trailing actions are Create and Search. Create uses the current context: Event in Agenda, Task in TODOs/Perspectives, Journal entry in Journal, and Org Document in Files. Hold Create to show all four types.
- Search focuses immediately. Cancel closes Search and returns to the page you came from.

On iPhone these groups share the bottom row. The primary tabs show symbols and titles only when their region is wide enough; tighter widths, including iPhone 15, show symbols only so the three destinations, selected state, and accessibility names remain separate instead of overlapping. On iPad the secondary button and primary tabs stay together at the top-left within the left content column, while Create and Search stay at the far right of the complete window. This does not add a global sidebar.

## 6. Verify Write-Back

Run one minimal round trip:

1. Create a `TODO` in TODOs.
2. Open its detail and edit the priority, date, or tags.
3. Return to Files and open the corresponding `.org` file in Edit.
4. Confirm that the headline, planning timestamp, properties, and tags were written to the real source.
5. After sync, open the same file in another client or Emacs and confirm the result.

## 7. If the First Sync Fails

- Do not repeatedly submit the same edit.
- Open Files and confirm whether the local source was already saved.
- Check the connection and sync status in Settings.
- If Aster requests reconnection, first confirm that the cloud account itself is reachable, then authorize it again in Aster.
- If a conflict exists, compare both copies and explicitly choose which one to keep. Do not treat a sync conflict as an ordinary network retry.

See [Sync, Data Safety, and Conflicts](sync-and-safety.md) for the complete guide.
