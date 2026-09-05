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

## Preview and Edit

After opening a supported text file:

- **Preview** presents Org or Markdown with reading-oriented layout.
- **Edit** displays and edits the complete source text.

Preview never becomes a second content store. Edit writes to the original file. When Search, item detail, or a preview block opens Source, Aster can navigate to the corresponding source line.

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
