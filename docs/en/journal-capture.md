<p align="right">🌐 <a href="../journal-capture.md">简体中文</a> · <strong>English</strong></p>

# Journal and Journal Entry Templates

[Back to English home](../../README.en.md) · [Quick Start](quick-start.md)

Journal is a calendar and timeline over daily Org files. A Journal Entry Template decides what one new entry inserts; its destination is always Today's Journal. Event and Task creation is a parallel flow that writes to the Event & Task Inbox, while Org Document creation writes a separately named file.

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

Journal and Agenda share one persisted Week/Month/Year calendar size and the same **Settings → Calendar & Agenda → Lunar calendar** switch. Turning that switch on or off adds or removes the same compact lunar annotations in both calendars; read-only holiday overlays also remain shared. The calendar stays fixed above the independently scrolling timeline. Each completed drag of the grabber moves only one adjacent size, while continued travel becomes progressively more resisted instead of stopping abruptly. Each committed Week swipe moves five calendar days, Month moves one month, and Year moves one year; all three use the same directional full-page transition, while Reduce Motion reaches the same result with less spatial movement. Journal initially highlights Today just like Agenda even when the newest entry is older; the timeline and wide-iPad detail still begin at that newest real entry. Dates containing prepared Journal entries have a marker. Tap a marked date to move the timeline to and select it; an unmarked date remains browse-only and never replaces the selected source-backed Journal day. After explicit selection or further timeline navigation, the selected date and visible calendar period stay synchronized, while a background refresh preserves a separately browsed period when the selected day is still available. The timeline uses Agenda's compact inline date headers and thin boundaries, then shows each time, title, and metadata directly; the daily filename no longer consumes a repeated row.

Week is not a conventional seven-day page; it is the five-column represented-date rail shared with Agenda. It follows prepared Journal dates and external all-day calendar dates, retains an adjacent empty date beyond either content boundary, and moves exactly five calendar days per committed release. That release uses the same single directional page transition as Month and Year without a second settle or spring rebound. An empty Week date also moves only the browsing position and never clears the last source-backed Journal selection. In iPad landscape, the left column uses this same rail, and the right side continues to show the previously selected day's real Journal content after browsing an empty date.

The Year overview uses those same inputs: Journal dates, traditional festivals, and selected read-only system holidays contribute density instead of losing lunar/holiday meaning outside Week or Month.

The similarly named Week/Month/Year choice under **Settings → Journal** limits only the Journal history window, prepared date markers, and Journal Search content. It does not change the visible calendar size or add a second range selector to the Journal page.

## Where a Journal Entry Starts

Tap Create in Journal to choose a configured Journal Entry Template. When none exist, Aster links to **Settings → Create & Storage → Journal Entry Templates**. The same template entrance also appears in Journal Settings.
The chooser keeps the Emacs-compatible template-first order and compact rows. The runtime composer uses the same native density: input begins at one line, previews and pending attachments add no nested cards, and photo, camera, and file actions keep full hit targets.

Holding the root Create action exposes Journal from another page. Agenda's Event/Task composer is intentionally separate and never uses Journal templates.

## A Minimal Journal Entry Template

For a timestamped note in today's Journal:

```org
** %<%H:%M> %?
```

Suggested configuration:

| Field | Value |
| --- | --- |
| Type | Entry |
| Destination | Today's Journal (fixed) |
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

## Destination

A Journal Entry Template always resolves **Today's Journal**: the configured Journal folder, daily filename, date root, and optional Journal section. It does not expose a workspace target path. If the daily file does not exist, the template may reference an **Org Document Template** to initialize it. The initializer runs once; the entry body expands every run.

## Org Document Templates and Placeholders

An Org Document Template generates the file header only when Aster first creates a `.org` file. Create or edit one under **Settings → Create & Storage → Org Document Templates**, then select it as the Journal Entry Template's **New file template**.

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

- **Inline**: stores the entry body directly in Aster's device settings.
- **Template File**: reads a workspace text template every time the entry runs, allowing Emacs and Aster to share the same insertion body.

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

Any Journal Entry Template that produces a heading can include photos, videos, or files. Aster will:

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

## Write-Back and Consecutive Entries

- A Journal entry commits one durable transaction to the daily file.
- After the first commit succeeds, an immediate second entry appends to the latest saved text even if the UI index has not yet published it.
- One accepted submission cannot be submitted repeatedly while its save is still running.
- Sync follows local write success. Journal, Files, and Search are projections over that same source file.
