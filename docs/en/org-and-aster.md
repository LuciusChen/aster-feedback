<p align="right">🌐 <a href="../org-and-aster.md">简体中文</a> · <strong>English</strong></p>

# How Org Source Appears in Aster

[Back to English home](../../README.en.md)

This is the central semantic reference for Aster. Every example includes copyable Org source, its actual Aster classification, where it appears, and the use case it represents.

## Classification at a Glance

| Org feature | Aster classification | Primary location |
| --- | --- | --- |
| No Workflow keyword, with an active timestamp or recognizable date planning | Event | Agenda |
| Unfinished Workflow keyword, no concrete clock time | Task | TODOs and Perspectives |
| Unfinished Workflow keyword, with a concrete clock time | Timed Task | Agenda |
| Workflow keyword configured as Treat as Project | Project | TODOs and Perspectives, optionally with progress |
| Unfinished Workflow + `STYLE=habit` + repeating `SCHEDULED` | Habit | Agenda when eligible today by default; Perspectives can review future habits (iOS/iPadOS build 14), never TODOs, with habit history |
| Yearly `org-anniversary` Diary | Anniversary; an optional Property can select Day Counter presentation | Agenda and the optional Anniversaries Perspective |
| `org-cyclic` / `diary-cyclic` Diary cycle | Cyclic Event | Agenda and optional Perspectives |
| No Workflow or date, but visible body content | Note | Files and Search; Journal when stored in a Journal source |
| No Workflow or direct body, only child headings | Container | Structural hierarchy, not a fabricated Task |

A filename, directory name, tag, or English word does not determine type on its own. `PROJECT`, for example, is only a default convention; a user can configure `PRO` as a Project instead.

Container is an Agenda/TODO semantic classification; it does not make Journal hide an authored heading. Journal preserves the complete Org hierarchy below its date root so a journal section never loses its meaning.

## Parsing and Source Fidelity

For each file revision, Aster first builds a source-range-preserving Org syntax tree and then projects Preview, Agenda, Journal, and Search from that tree. The official default element and object families have distinct structure, including diary sexps, inline tasks, standard and special blocks, nested lists, inline and multiline footnotes, inline source/Babel calls, citation references, and regular, plain, and angle links.

- Anonymous footnotes cannot become multiline definitions that consume following prose; continuation lines and child lists stay owned by their real list item.
- `SCHEDULED`, `DEADLINE`, and Properties receive structural meaning only in a heading's metadata position. The same text in ordinary prose does not become Task planning.
- Unknown extension syntax remains exact source text. Aster does not execute Babel or arbitrary Diary Lisp; Agenda interprets only the documented safe subset.

### Reading Boundaries (iOS/iPadOS build 14)

The following reading example is corrected without changing its Org source:

```org
* Reader checks
2 * 3 * 4

1. [ ] Pending
  * [X] Complete

| Name | Count
| Apples | 2

-----

%%(org-calendar-holiday) Custom reminder
```

Preview keeps the operators and displays the checkboxes, table, and separator. The final line remains visible as source text; it is not executed and does not create an Agenda reminder.

## 1. All-Day Event

### Org source

```org
* Team offsite :team:
<2026-09-03 Thu>
```

### Actual Aster behavior

- There is no TODO keyword, so this is not a Task.
- It appears on 3 September 2026 in Agenda as an all-day Event.
- It uses a compact strip similar to a holiday or anniversary and has no Task state action.
- Aster writes a plain active timestamp like this when it creates an Event, not `SCHEDULED:`.

### Typical use

All-day meetings, trips, anniversaries, release dates, or other dated information that does not require completion state.

## 2. Event with a Time Range

### Org source

```org
* Design review :meeting:
<2026-09-03 Thu 14:00-15:00>
```

### Actual Aster behavior

- Agenda displays `14:00–15:00` above the title.
- There is no Workflow icon or TODO state.
- If only `14:00` is entered while creating an Event, Aster defaults to the one-hour range `14:00-15:00`.
- If `14:00-15:30` is entered explicitly, Aster preserves that range.

### Typical use

Meetings, classes, appointments, travel intervals, or any event with a start and end.

> Compatibility: existing keyword-free headings that use a date in `SCHEDULED:` or `DEADLINE:` remain readable as Events. Events created by Aster use an active timestamp so they do not acquire Task-planning semantics.

### Multi-day Events (iOS/iPadOS display/edit fix is unreleased)

```org
* Trip
<2026-09-10 Thu 10:00>--<2026-09-13 Sun 11:00>
```

This is one active-timestamp range, not Scheduled through Deadline. Org Agenda includes it on September 10, 11, 12, and 13; the Aster fix follows the same date coverage. The hour-based timeline clips actual occupancy to each day, so a midnight endpoint does not invent occupied time on its last date.

Detail retains both full dates and clocks. Editing only the title should leave this timestamp unchanged. Enabling All day should produce `<2026-09-10 Thu>--<2026-09-13 Sun>`. A point Event without an end remains valid and does not gain a fabricated endpoint.

Creation and detail use the same localized format for both endpoint dates, including the full year. Tapping a date opens a calendar in place, not a child page. Each clock remains beside its date and follows the system's 12/24-hour preference.

### Daily windows within a date range (unreleased)

```org
* Play CS2 with 周末侠
<2026-09-11 Fri 19:00-22:00>--<2026-09-13 Sun 19:00-22:00>
```

This means 19:00–22:00 on September 11, 12 and 13, not uninterrupted occupancy. Agenda shows `(1/3)`, `(2/3)` and `(3/3)`, and the timeline draws only each evening's three hours. Detail still edits the single source heading and preserves both endpoint windows. Title-only edits leave the timestamp untouched.

This follows Org's official [daily time range syntax](https://orgmode.org/manual/Timestamps.html). It does not require a repeater or three separate Events.

For a daily window of `22:00-01:00`, Aster also shows the previous night's early-morning portion. Middle dates show both the morning and evening portions, and the final morning remains visible. A midnight endpoint does not add an empty segment.

These display portions still belong to one Org heading. Progress counts the three written start dates, so the final morning still belongs to `(3/3)`. Saving a window with equal start and end clocks does not move its written ending date forward by another day.

## 3. Undated Task

### Org source

```org
* TODO Write release notes :release:
```

### Actual Aster behavior

- It appears in TODOs and matching Perspectives.
- It does not appear on the dated Agenda timeline.
- It displays the current Workflow keyword, configured icon, tags, and priority.
- A date can be added in detail, or the Task can remain undated.

### Typical use

Inbox items, next actions, or work that matters but has no date yet.

## 4. Date-Only Task

### Org source

```org
* TODO Submit expense report :finance:
DEADLINE: <2026-09-03 Thu>
```

### Actual Aster behavior

- It remains in TODOs because there is no concrete clock time.
- Its Deadline appears in item metadata.
- Once the date passes while the state remains unfinished, it receives overdue styling.
- It does not become an all-day Event merely because it has a date.
- It creates no notification. To alert at a specific time, enable `At a time`, write that time into the standard `SCHEDULED` or `DEADLINE` value, and save.

### Typical use

Work that should be completed by a date but does not occupy a specific time slot.

### Both a scheduled date and a deadline

```org
* TODO Submit expense report :finance:
SCHEDULED: <2026-09-07 Mon> DEADLINE: <2026-09-11 Fri -2d>
```

This Task is planned for September 7 and due on September 11. Neither field has a clock time, so it remains in TODOs. The `-2d` cookie starts due-status presentation two days before the deadline.

Opening the Deadline row in iPhone or iPad detail loads September 11 independently. Changing it to September 12 keeps `SCHEDULED: <2026-09-07 Mon>` and updates only Deadline to `<2026-09-12 Sat -2d>`.

Turning off the Deadline switch removes only that date, leaving Scheduled and the body unchanged.

iOS/iPadOS build 15 shows both inline sections when creating a Task with +. Both switches can be enabled together, and their clocks may differ, such as Scheduled at `09:00` and Deadline at `17:00`. Saving writes one Task with both fields on the same planning line immediately after its heading. Splitting them across two lines would prevent Emacs from recognizing both.

## 5. Task at a Specific Time

### Org source

```org
* NEXT Call supplier :phone:
SCHEDULED: <2026-09-03 Thu 09:15>
```

### Actual Aster behavior

- It remains a Task because it has an unfinished Workflow keyword.
- It appears on the Agenda timeline because its planning contains a clock time.
- `09:15` is one exact moment, not an Event start–end range.
- If it remains unfinished after that moment, the time itself turns red.
- Saving a timed Task schedules one local alert from the standard Org planning time; it does not require a second Notify switch.

### Typical use

An action that should happen, or alert you, at one specific time.

## 6. Project

Project identity is not hard-coded to a filename or the literal word `PROJECT`. It comes from **Treat as Project** in Workflow settings.

### Org source

```org
#+TODO: TODO NEXT PROJECT | DONE CANCELLED

* PROJECT Release 1.0 [2/3] :release:
:PROPERTIES:
:COOKIE_DATA: todo
:END:
** DONE Prepare metadata
** DONE Upload build
** NEXT Invite testers
```

### Actual Aster behavior

- The main heading uses the Project identity configured for that keyword.
- It displays `2/3 · 67%` and a progress bar.
- Child Tasks keep their own Workflow states and open independent detail views.
- A Project remains in TODOs or a matching Perspective. Aster has no hard-coded Projects destination; the optional Projects built-in template simply creates a normal Perspective from the Treat as Project role.

### Progress source

| Choice | Org write-back | Counted content |
| --- | --- | --- |
| Automatic | No `COOKIE_DATA` | Used only when the source is unambiguous |
| Checklist | `:COOKIE_DATA: checkbox` | Checkboxes under the current heading |
| Direct subtasks | `:COOKIE_DATA: todo` | Direct child Tasks |
| All subtasks | `:COOKIE_DATA: todo recursive` | All descendant Tasks |

If both checkboxes and subtasks exist without an explicit source, Aster asks instead of guessing.

## 7. Habit

iOS/iPadOS build 14 adds ways to [review future habits in today's list or a Perspective](agenda-todos.md#reviewing-future-habits-iosipados-build-14). Visibility is separate from the real schedule; the setting does not change the Org content below.

A standard Habit requires all three: an unfinished Workflow keyword, `STYLE=habit`, and a repeating `SCHEDULED` timestamp.

### Org source

```org
* NEXT Daily walk :wellbeing:
SCHEDULED: <2026-09-03 Thu 07:30 .+2d/4d>
:PROPERTIES:
:STYLE: habit
:REPEAT_TO_STATE: NEXT
:END:
:LOGBOOK:
- State "DONE" from "NEXT" [2026-09-01 Tue 07:30]
:END:
```

### Actual Aster behavior

- It continues to display its real `NEXT` Workflow identity. Habit is a secondary classification, not a replacement keyword.
- It shows a Habit label, an `every 2–4 days` cadence, and the most recent 28-day completion graph.
- Blue means not yet due, green means actionable, yellow means due, and red means overdue.
- `*` marks a completion found in LOGBOOK; `!` marks today.
- Completing the item advances the source timestamp according to the Org Repeater and returns the state to the configured active state.

### Typical use

Recurring reviews, exercise, writing, Inbox cleanup, or other actions that benefit from completion history and a tolerance window.

## 8. Note and Container

### Note with body content

```org
* Reading notes :research:
Aster keeps prose, structure, and references in plain Org files.
```

Aster treats this as document content. It appears in Files, Org Preview, and Search, but not in Agenda or TODOs.

### Structural Container

```org
* Work
** Project A
Some notes.
** Project B
More notes.
```

`Work` has no direct body or task semantics; it only organizes child headings. Aster preserves its outline level without creating a fake Task named Work.

## 9. Priority, Tags, and Headline Order

The standard headline order is Workflow, Priority, title, Diary-only time, then tags.

```org
* WAITING [#B] Weekend review 10:00-11:30 :review:mobile:
```

- **Workflow**: the complete configured keyword is preserved.
- **Priority**: source values such as `[#A]` through `[#Z]` or numeric forms retain their literal meaning. Aster's common compact choices are None, A, B, and C.
- **Diary time**: ordinary times belong in a Timestamp. A Diary expression has no concrete time, so `HH:mm` or `HH:mm-HH:mm` may appear at the end of the title before tags.
- **Tags**: editing preserves the `ATTACH` tag used by Org Attach.

### Agenda Visibility and Planning Display Cookies

The following subtrees remain available in Files, Org Preview, Edit, and Search, but are excluded from Agenda, TODOs, Perspectives, widgets, and local reminder planning by default:

```org
* TODO COMMENT Not part of the current plan
** TODO This child is excluded too

* Archived material :ARCHIVE:
** TODO This child is excluded too
```

`COMMENT` is the standard headline marker after Workflow and Priority. `ARCHIVE` is the exact special tag. Aster neither deletes nor rewrites these subtrees.

Standard Deadline warning and Scheduled delay cookies are preserved too:

```org
* TODO Submit release
DEADLINE: <2026-09-10 Thu -5d>

* TODO Prepare environment
SCHEDULED: <2026-09-01 Tue -2d>

* TODO First setup only
SCHEDULED: <2026-09-01 Tue --2d>
```

- `DEADLINE ... -5d` begins compact due-status presentation five days before the deadline. `-0d` limits it to the due day; without an explicit cookie, Aster uses a 14-day warning window.
- `SCHEDULED ... -2d` delays every derived Agenda appearance by two days. `--2d` delays only the first occurrence of a repeating Task and stops after a standard completed Workflow log records that first completion.
- These cookies affect only derived Agenda presentation. They do not rewrite the Org date or postpone a local notification derived from a concrete clock time. Editing the date within the same planning kind preserves the cookie; changing to an incompatible planning kind removes it.
- Category comes only from a parser-confirmed `CATEGORY` Property, file-level `#+CATEGORY`, or the filename fallback. The same text inside a code block or ordinary prose cannot become Agenda metadata.

## 10. Standard Repeaters

```org
* TODO Daily review
SCHEDULED: <2026-09-03 Thu 08:00 ++1d>
```

| Org cookie | Meaning |
| --- | --- |
| `+1d` | Advance one interval from the source date |
| `++1d` | Keep cadence and catch up into the future |
| `.+1d` | Recalculate the next date from completion time |

Units are `h`, `d`, `w`, `m`, and `y`, and intervals must be positive. Completing a Task or Habit advances the source timestamp. A repeating Event produces virtual future occurrences without rewriting its original active timestamp.

The unreleased version follows Org's date overflow rather than clamping to month end. January 31, 2027 plus one month is March 3. Agenda expands each occurrence from the original anchor, so the next projected occurrence can still be March 31. Completion changes the stored anchor according to the repeat mode.

Date and Repeat edits change the original Org entry directly. They do not create a one-off or automatically advance the repeat by an extra interval. Habit edits preserve completion history, and explicit completion still follows the normal logging workflow.

## 11. Weekday Diary

```org
* Weekend Review 21:00-21:20 :review:
SCHEDULED: <%%(memq (calendar-day-of-week date) '(0 6))>
```

- `0` is Sunday and `6` is Saturday.
- Aster recognizes only explicitly supported, bounded forms. It does not evaluate arbitrary Lisp.
- The Diary expression determines the days; the time at the end of the title determines the clock time.
- Without a Workflow keyword, this is a repeating Event. Adding a Workflow keyword makes it a Task.

## 12. Anniversary, Elapsed Days, and Cyclic Events

Yearly anniversary:

```org
%%(org-anniversary 2020 8 22) Aster is %d years old
```

To show elapsed days in Aster's Anniversaries view, keep the standard Anniversary and add one optional Property:

```org
* Wedding anniversary
:PROPERTIES:
:ASTER_ANNIVERSARY_DISPLAY: elapsed-days
:END:
%%(org-anniversary 2022 11 2) Wedding anniversary
```

- `org-anniversary` takes `year month day`; `%d` is the occurrence year minus the source year.
- Aster always interprets those numbers as Gregorian in the current time zone. A Buddhist, Islamic, or other device display calendar does not change the source date or progress result.
- `ASTER_ANNIVERSARY_DISPLAY` accepts `years` and `elapsed-days`, and Aster enables derived calculation only when a supported value is written explicitly. `years` calculates completed years, `elapsed-days` calculates total elapsed days, and both calculate the days until the next anniversary. Org Preview retains the compact `N days · Next in M days` copy. In Anniversaries, a future source reads `Starts in N days · Month Day` and its source date reads `Starts today`; it is never mislabeled as a first anniversary. After a complete year, an ordinary entry uses its numbered milestone and next date, for example `4-year anniversary` and `In 65 days · November 2`. An explicit display retains its selected total and describes that same source or next numbered milestone on the second line. A keyword-free yearly Org timestamp also uses the shared date rail, but keeps ordinary yearly-repeat meaning and never invents an anniversary number. Every entry uses the same flat date rail and inset separator; the first entry has no featured card, gradient, or rounded background. A missing or unsupported Property enables no elapsed calculation. The Property does not duplicate the date, and Org Agenda still receives only the real annual occurrence.
- Preferred `org-cyclic` takes `interval-days year month day`. It always represents a cyclic Event; interval `1` appears every day, and `%d` is the completed cycle count.
- To appear only every 100 days, use `%%(org-cyclic 100 2023 8 28) Married for the %dth hundred days`. Here `%d` counts completed 100-day cycles.
- Aster remains compatible with `diary-cyclic` in `interval-days month day year` order. Editing preserves the original function and its corresponding argument order, while new examples prefer `org-cyclic` because it is independent of `calendar-date-style`.
- Both remain keyword-free source entries and never acquire a TODO state.
- Enable **Settings → Tasks & Workflow → Views → Anniversaries** to collect annual anniversaries in an optional Perspective. Only entries with the explicit Property above add completed years or elapsed days and the next-anniversary countdown.
- Aster never executes arbitrary Diary Lisp. Unsupported expressions remain lossless Org source.

## 13. Compatibility with Earlier Aster Reminder Properties

Current New and Detail screens create notifications only from a concrete time in standard Org `SCHEDULED` or `DEADLINE` planning. They no longer create private reminder properties. The properties below are read only for compatibility with files written by earlier builds:

```org
* TODO Submit report
DEADLINE: <2026-09-03 Thu>
:PROPERTIES:
:ID: 00000000-0000-0000-0000-000000000001
:ASTER_REMINDER: once
:ASTER_REMINDER_TIME: 09:05
:END:
```

Earlier date-only Task reminders remain readable. Opening a date-only Task with a valid `ASTER_REMINDER_TIME` places that clock in the ordinary `At a time` editor; saving writes standard timed planning and removes the obsolete private reminder properties. Loading a workspace alone never rewrites the source in the background.

Older files may also contain persistent-reminder properties:

```org
:ASTER_REMINDER: persistent
:ASTER_AUTO_SNOOZE: 15m
:ASTER_ALERT_UNTIL: done
```

Supported repeated-alert intervals are `5m`, `10m`, `15m`, `30m`, and `60m`.

## 14. Write-Back Boundaries

Aster's semantic edits do not reformat the entire file. A state, date, priority, tag, reminder, Note, or attachment operation modifies only the headline, planning line, properties, LOGBOOK, or attachment links it owns. Unknown properties, body text, subtrees, and other source content remain unchanged.

The Source button in the detail toolbar shows the complete saved Org subtree. Files → Edit shows and edits the complete source file.

The following metadata, date-range, attachment-move, and failed-edit corrections are included in iOS/iPadOS build 13. They do not imply the same coverage in Android build 9.

Adding a Note, completing a repeating task, or prepending plain-template content keeps planning and properties in the heading's metadata area. Literal examples of CLOSED, properties, or LOGBOOK inside code blocks are not edited as metadata. Date insertion supports LF, CRLF, and files without a final newline.

Adding an Event date or converting a Task to an Event places the new active timestamp after planning and properties, before the body. Existing IDs, custom properties, and attachment references remain readable. Later date edits replace only the corresponding timestamp.

Ordinary keyword-menu and detail state changes also update parent progress. Completing a repeating planning range advances both endpoints, not just its start. Moving an existing range's date in detail also preserves its duration; new Tasks still use point timestamps.

Moving or archiving across folders copies the parent and child headings' ID attachments and adjusts relative file links to keep their original destinations. Original attachments remain available for other references. Missing downloads, differing destination files, or write failures stop the operation without removing the original subtree.

Back leaves task detail only after a successful save. Failed saves retain input; reopening the same item after switching root pages also restores its failed edit within the running session. Canceling a completion note does not complete the task. This is not a persistent Journal draft: save or copy the content before quitting the app.

## 15. A Project Entry Point and Focused Reading (iOS/iPadOS build 12)

### Org source

```org
* Writing project
:PROPERTIES:
:ID: writing-project-example
:END:
Project notes.
** Draft
Draft text.
** References
Reading notes.
* Another project
Unrelated material.
```

### Actual Aster behavior and typical use

`aster://open?id=writing-project-example&focus=true` opens Writing project with its Draft and References children, excluding Another project from the reading scope. The standard ID survives renaming or moving within the current workspace. Do not reuse this example ID for several headings.

In Apple Shortcuts, add Open Org Heading, select Writing project, enable Focus, and add the shortcut to your Home Screen. This introduces no date, TODO state, or private property and does not change Agenda classification.

Full Document restores the complete reading scope. Edit always opens the whole file; it never removes the other project from source. See [file and heading links](files-preview-attachments.md) for the interaction guide.

## 16. Time Zones and Travel (iOS/iPadOS build 14)

### Ordinary Org follows local time

```org
* TODO Morning review
SCHEDULED: <2026-09-08 Tue 09:00>
```

This means 09:00 locally in Shanghai and remains 09:00 locally after moving to New York. Reparsing does not rewrite the source. Date-only values keep their calendar date as well. Use this example after travel to check refreshed Agenda, notification, and badge behavior.

### System reminders retain fixed instants

This shows the managed `apple-reminders.org` structure. Real IDs come from the system; the example ID is not a new reminder that can be synchronized.

```org
* TODO New York reminder
DEADLINE: <2026-09-08 Tue 15:00>
:PROPERTIES:
:APPLE_REMINDER_ID: example-provider-id
:APPLE_REMINDER_DEADLINE_TIME_ZONE: America/New_York
:END:
```

Aster shows September 9 at 03:00 in Shanghai and September 8 at 15:00 in New York. These represent the same instant. A title or completion edit does not move the reminder earlier or later. An independent start date uses `APPLE_REMINDER_SCHEDULED_TIME_ZONE`.

These properties apply only to identified Apple Reminders items, not to ordinary Org as general time-zone syntax. Readers such as Emacs that ignore them see the written New York clock, 15:00, without converting it to Shanghai time. System reminders without an explicit zone remain floating local times.

Removing and re-adding a date uses any retained zone for that field, even before sync completes. With the New York property above, choosing September 9 at 04:00 in Shanghai writes September 8 at 16:00 in New York, not the Shanghai clock misinterpreted as New York time.

Date-only values and range spans always follow the local calendar. Retained zone metadata cannot shorten November 7–9 to November 7–8. Restoring a clock uses the retained zone when saving, without reinterpreting the old range's day span.

Changing the source between a fixed zone and floating local time also refreshes notification rules, even if the displayed time is currently the same.

### Repeating ranges across daylight saving

```org
* TODO Two-day review
SCHEDULED: <2026-03-07 Sat 09:00 +1w>--<2026-03-09 Mon 09:00>
```

Completing this once in New York advances the range to March 14 at 09:00 through March 16 at 09:00. Crossing DST does not remove a day or move the end clock to 08:00.

Short ranges follow the same rule. A weekly 01:30–03:30 entry on March 8, 2026 repeats at 01:30–03:30 on March 15, not 01:30–02:30. Manually moving the date also preserves this wall-clock span.

Hourly repeats instead preserve elapsed duration. Each timestamp keeps its own repeat rule when a heading contains several timestamps. Aster does not guess corrections for ranges already damaged by an older build.
