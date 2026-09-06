<p align="right">🌐 <a href="../files-preview-attachments.md">简体中文</a> · <strong>English</strong></p>

# Files, Preview, Editing, and Attachments

[Back to English home](../../README.en.md) · [Org classification and write-back](org-and-aster.md)

Files is the source-level entrance to the workspace. Agenda, TODOs, Journal, and Search provide semantic views; Files lets you inspect directories, open original files, read Preview, and edit Source when necessary.

![iOS/iPadOS Org Preview example](../../assets/screenshots/org-preview.png)

The screenshot comes from iOS/iPadOS. Android keeps the same Org semantic hierarchy with Android-native typography, navigation, and file-opening capabilities.

## File Tree

Files displays the real hierarchy of connected workspaces, including local files and remote items known to the sync index.

- Icons after a directory name only indicate an Agenda, Journal, or Event & Task Inbox role. They do not change the directory itself.
- Configure those roles in Settings. A folder context menu may also add or remove that folder from the root Files secondary menu; this is only a navigation shortcut and does not change the folder's role.
- A real empty directory stays visible without a meaningless disclosure action.
- Dropbox-backed files can expose version history on hold. Other providers do not show an inert history entry.

### Apple Shortcuts (next iOS/iPadOS build, unreleased)

Search for Aster in Shortcuts and choose **Open Org File** or **Open Org Heading**. File actions select a local Org file. Heading actions support title/path search and show the ancestor path. The heading action's **Focus** switch is off by default; enabling it opens the complete subtree.

For example, create a focused shortcut to a project's top heading and add it to your Home Screen. It opens that project subtree; Full Document returns to the original file. Querying options requires no AI, does not start sync, and never changes Org source. Sync new files locally in Aster before selecting them.

If several headings in one file share a title and lack unique IDs, use Copy Aster Link on the intended heading in Aster and confirm adding an ID first. Ambiguous headings are not offered as shortcut options, preventing an unintended match.

Adding an ID preserves existing title-based or CUSTOM_ID shortcut selections: Aster resolves their original links again. Renaming a title or file still requires updating selections that depend on that name. A heading that cannot form a valid link does not prevent other headings from appearing in the picker.

### Focus a heading (next iOS/iPadOS build, unreleased)

Long-press an Org Preview heading and choose **Focus** to read that heading and its complete subtree. The breadcrumb keeps the document and ancestors visible. Select an ancestor to widen the scope, or **Full Document** to restore the previous reading position and folding state.

Focus does not trim or rewrite the Org file. Edit still opens the complete source at the focused heading, and attachments and links retain the original document context. Add `&focus=true` to a heading link to open the same focused preview directly.

A plain heading tap does not enter editing; long-press and choose Edit Source instead. Disclosure arrows, tags and links retain their own tap actions. Returning from Edit to Preview preserves your current position within the subtree instead of returning to its top.

### Open a specific file or heading (next iOS/iPadOS build, unreleased)

Choose **Copy Aster Link** from a file's context menu or the Org document toolbar to open it from another app. Heading context menus provide the same action. Existing `ID` or `CUSTOM_ID` properties are reused; adding a missing standard Org `:ID:` asks for confirmation. Cancel leaves the source unchanged.

A file link looks like `aster://open?file=journal%2F2026-09-06.org`, relative to the current workspace. Heading links prefer `aster://open?id=your-heading-id`, which continues to locate a renamed heading or one moved within the workspace. Links do not upload content or grant another device access to files it does not have.

Handwritten links also support `file=<path>&custom-id=<CUSTOM_ID>` or `file=<path>&heading=<exact title>`. Title-based links need a unique match and must be updated after a rename. Missing, duplicate, and invalid targets produce an error rather than opening an approximate match.

### File Sorting (next iOS/iPadOS release, unreleased)

Expand a folder and tap the gray **⇅** at the far right of its name row to sort its files by name, document date, or modification time in either direction. No long press is needed. Buttons share the same trailing edge at every nesting level and disappear when a folder is collapsed. Empty folders have no sort button.

Expanded workspace roots have the same control: in the page header for a single root, or on each root's row when several roots are shown. Each folder remembers its own choice across app restarts; child folders keep their own settings.

Folders stay ahead of files and remain in name order. Files with missing dates stay last in both directions, and equal dates use name order. Sorting never changes files, the Journal timeline, or the root folder-shortcut menu.

For the `journal` folder in Files, choose **Document Date · Newest First**. This uses Journal's existing date rules, then an explicit Org `#+DATE` if no Journal date is available. For example, `#+DATE: [2026-09-03 Thu 09:00]` represents September 3; editing that file later does not move its document date to today. A file containing multiple Journal days uses its first dated root.

**Modified** uses the available local file modification time, not a Journal day. Sorting does not download remote placeholders to obtain dates. Files move into date order when that information becomes available. Sort preferences stay on the current device.

## Preview and Edit

After opening a supported text file:

- **Preview** presents Org or Markdown with reading-oriented layout.
- **Edit** displays and edits the complete source text.

Preview never becomes a second content store. Edit writes to the original file. When Search, item detail, or a preview block opens Source, Aster can navigate to the corresponding source line.

Interface refreshes preserve newly typed text and active selections. Full-text replacement still writes the exact source you entered.

### Save conflict protection (next iOS/iPadOS build, unreleased)

Org and Markdown saves check the version originally opened. If sync or another operation has changed the file on disk, Aster keeps your edits instead of overwriting the newer version. A failed Org save blocks opening another Org file. Reopening a failed Markdown edit during the same app session restores its text.

After a save failure, the toolbar offers **Reload Saved File**. It asks before discarding unsaved changes; canceling keeps your text. Save or copy anything important before reloading or quitting. These temporary recovery buffers are different from persistent Journal drafts.

## What Org Preview Supports

Org Preview renders syntax nodes rather than scanning source with simple regular expressions, including:

- Nested headings, paragraphs, tags, and selected file metadata
- Tables, lists, and checkboxes
- Links, images, captions, and display attributes
- Property drawers, other drawers, and collapsible regions
- Source, Example, Quote, and related blocks
- Dynamic blocks and adjacent results
- Footnotes and inline LaTeX

Standard LOGBOOK state history starts collapsed behind each heading's **History** row, allowing a Habit file to show more habits at once. Tap the row to reveal the complete history. Preview does not repeat those same state records as ordinary body text, and the source remains unchanged.

Unknown or not-yet-rendered syntax remains intact in Edit.

### Dynamic Blocks

For example:

```org
#+BEGIN: dg-argument-gaps
/Updated: 2025-12-29 16:31/

| Claim        | Structural Gaps |
|--------------+-----------------|
| [[dg:c1][A]] | no-source       |

#+END:
```

In Preview, `#+BEGIN:` and `#+END:` are structural boundaries rather than duplicated body text. Generated content already present between them is rendered as paragraphs and tables. Aster does not run the Emacs function that generates the block; update it with the original Org/Emacs tooling.

### Babel

```org
#+begin_src python
print("hello")
#+end_src

#+RESULTS:
: hello
```

Aster can fold, color, and read Source and Results, but it never executes the code. This preserves the file while preventing a mobile preview from running user scripts.

## Non-Org Files

Markdown uses an independent Markdown reader. A remote file must finish downloading into the app workspace before preview: iOS/iPadOS uses native system document capabilities, while Android uses Aster's native readers and, when needed, a compatible system app. Files does not expose generic local Import as another workspace provider. If a supported type does not open, report the platform, extension, provider, and a minimal test file.

### Markdown Structure Corrections (Next iOS/iPadOS, Unreleased)

Indented code stays literal instead of becoming a heading or list. Nested lists retain their depth, while Setext underlined headings and explicit hard breaks preserve the source structure.

These reading changes do not rewrite Markdown. Opening Edit from Preview still targets the exact original block, including heading underlines and list indentation.

## Org Attach Mapping

Attachments use the standard Org Attach directory layout:

```text
<directory containing the Org file>/data/<first two ID characters>/<remaining ID>/<filename>
```

For example:

```org
* TODO Review contract :ATTACH:
:PROPERTIES:
:ID: 3d94a625-be28-4144-8158-05fbf1288d5c
:END:
[[attachment:contract.pdf]]
```

The physical file path is:

```text
data/3d/94a625-be28-4144-8158-05fbf1288d5c/contract.pdf
```

The `attachment:` link, heading `ID`, `ATTACH` tag, and `data/` file must travel together. Org Attach is not an Aster-specific attachment format limited to `.txt` files.

## Where Attachments Can Be Added

- New Event/Task: use **Attachments** directly on the creation surface.
- Event/Task detail: use **Attachments** after Content.
- Journal entry: choose media or files while running a heading-producing Journal Entry Template.
- Files: open the source document to inspect attachment links and physical files.

## Delete, Move, and Sync

- Deleting or moving a file or directory goes through the current provider rather than merely hiding it in the UI.
- Deleting a nonempty directory requires explicit confirmation of the recursive scope.
- The local tree and role settings update only after the remote operation succeeds.
- After moving or deleting an Agenda, Journal, or Event & Task Inbox path, check its role in Settings.
