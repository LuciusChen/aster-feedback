<p align="right">🌐 <a href="../journal-capture.md">简体中文</a> · <strong>English</strong></p>

# Journal and Capture

[Back to English home](../../README.en.md) · [Quick Start](quick-start.md)

Journal is a timeline over daily Org files. Capture is the general mechanism for inserting structured content into Org. They are often used together, but they are not the same thing: Journal decides how today's file and entries are presented, while a Capture Template decides what each run inserts and where it goes.

![Journal example](../../assets/screenshots/journal.png)

## Daily Journal Files

A daily file created by Aster contains a stable date marker and a level-one day heading:

```org
#+TITLE: Thursday, 27 Aug 2026
#+ASTER_JOURNAL_DATE: 2026-08-27

* Thursday, 27 Aug 2026
** 09:10 Morning notes
Today I want to finish the release checklist.
```

- `#+ASTER_JOURNAL_DATE` identifies the date independently of display language.
- Existing Journal files without that marker can still be recognized from their title, level-one date heading, or Denote filename.
- Journal year, month, and date use the Gregorian calendar in the current time zone. Lunar dates are an optional display annotation and do not change file identity.
- Configure history range, entry count, and filename behavior under **Settings → Journal**.

## Where Capture Starts

Tap the bottom `+` in Journal to choose a configured Capture Template. When none exist, Aster links to **Settings → Org & Capture → Templates → Capture templates**.

Holding the global `+` also exposes Capture from another page. Agenda's Event/Task composer is intentionally bounded and separate from general Capture templates.

## A Minimal Capture Template

For a timestamped note in today's Journal:

```org
** %<%H:%M> %?
```

Suggested configuration:

| Field | Value |
| --- | --- |
| Type | Entry |
| Destination | Today's Journal |
| Journal section | Blank, or `Notes` |
| Source | Inline |
| Prepend | Choose according to preferred reading order |

At runtime, `%?` is the main input position and `%<%H:%M>` expands to the current time. The result is ordinary Org source, never an Aster-only shadow record.

## Template Types

### Entry

Entry creates a heading tree. If the source has no leading star, Aster adds a heading. When writing below an existing outline path, it adjusts the heading level.

```org
*** TODO %? :inbox:
%U
```

### Plain

Plain inserts expanded text directly into the destination body without wrapping it in a heading. A blank Plain source acts like an ordinary text Capture buffer and accepts the full content at runtime.

## Destinations

Capture has two destination types:

- **Today's Journal**: resolves today's Journal file, date root, and optional Journal section at runtime.
- **Workspace Org file**: writes to a workspace-relative path such as `agenda/inbox.org`, with an optional outline path.

If the destination does not exist, Capture can reference an **Org File Template** to initialize it. A file template runs only once; the Capture body expands on every run. These are intentionally separate concepts.

## Org File Templates and Placeholders

An Org File Template generates the file header only when Aster first creates the target `.org` file. Create or edit one under **Settings → Org & Capture → Templates → File templates**, then select it as the Capture Template's **New file template**.

| Placeholder | Expanded value |
| --- | --- |
| `{title}` | The single-line document title supplied when the file is created |
| `{date}` | The current Gregorian date in `YYYY-MM-DD` format |
| `{created}` | An inactive Org timestamp with the current time, such as `[2026-08-28 Fri 09:30]` |
| `{identifier}` | A newly generated lowercase UUID |

For example, this adapts an Emacs Denote Journal front matter definition into an Aster file template:

```org
#+title:      {title}
#+date:       {created}
#+filetags:
#+identifier: {identifier}
#+startup: indent
#+ARCHIVE: journal.org::
```

Here, `#+filetags:` is an ordinary Org field left blank; Aster does not currently provide a `{filetags}` placeholder. Fixed tags may be written directly into the template. Emacs settings such as `org-download-image-dir`, `.dir-locals.el`, and Lisp expressions are not file-header content and cannot be placed in this template; media added by Aster uses Org Attach.

## Inline and Template File

- **Inline**: stores the Capture body directly in Aster's device settings.
- **Template File**: reads a workspace text template every time Capture runs, allowing Emacs and Aster to share the same insertion body.

A Template File defines “what to insert this time.” It does not replace the initializer for a missing destination file.

## Supported Runtime Fields

| Field | Meaning |
| --- | --- |
| `%?` | Primary edit position |
| `%i` | Initial text |
| `%a` | Source; prompts when no invoking source exists |
| `%t` / `%T` | Date-only / date-and-time active timestamp |
| `%u` / `%U` | Date-only / date-and-time inactive timestamp |
| `%<…>` | Current time in a chosen format, such as `%<%Y-%m-%d>` |
| `%^{Prompt\|default\|choice}` | Runtime question with an optional default and choices |
| `%%` | A literal percent sign |

Aster does not execute `%(...)`, arbitrary Emacs Lisp, `.dir-locals.el`, or external functions. Common dynamic behavior such as “Today's Journal” is implemented as a safe native destination instead.

## Media and Attachments

Any Capture that produces a heading can include photos, videos, or files. Aster will:

1. Write or reuse an `ID` for the heading.
2. Preserve the `ATTACH` tag.
3. Copy files into the standard Org Attach directory.
4. Insert relative attachment links in the heading body.

Attachments and source text are handled by the same accepted Capture. If the source commit fails, newly copied files from that attempt are not reported as a successful result.

## Common Scenarios

### Quick log

```org
** %<%H:%M> %?
```

Choose Today's Journal for ordinary chronological notes.

### Reading excerpt

```org
** %^{Book} — %?
%U
Source: %a
```

Enter the book and source at runtime, and optionally attach a cover or screenshot.

### Task Inbox

```org
* TODO %? :inbox:
%U
```

Choose `agenda/inbox.org` as the destination. This is general Capture; TODOs `+` remains the faster path for an ordinary Task.

## Write-Back and Consecutive Capture

- Capture commits one durable transaction to the target file.
- After the first commit succeeds, an immediate second Capture appends to the latest saved text even if the UI index has not yet published it.
- One accepted submission cannot be submitted repeatedly while its save is still running.
- Sync follows local write success. Journal, Files, and Search are projections over that same source file.
