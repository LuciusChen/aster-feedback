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
| Unfinished Workflow + `STYLE=habit` + repeating `SCHEDULED` | Habit | Agenda/TODOs and Perspectives, with habit history |
| Yearly `org-anniversary` Diary | Anniversary; an optional Property can select Day Counter presentation | Agenda and the optional Anniversaries Perspective |
| `org-cyclic` / `diary-cyclic` Diary cycle | Cyclic Event | Agenda and optional Perspectives |
| No Workflow or date, but visible body content | Note | Files and Search; Journal when stored in a Journal source |
| No Workflow or direct body, only child headings | Container | Structural hierarchy, not a fabricated Task |

A filename, directory name, tag, or English word does not determine type on its own. `PROJECT`, for example, is only a default convention; a user can configure `PRO` as a Project instead.

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
* WAITING [#B] Weekend review 10:00-11:30 :review:ios:
```

- **Workflow**: the complete configured keyword is preserved.
- **Priority**: source values such as `[#A]` through `[#Z]` or numeric forms retain their literal meaning. Aster's common compact choices are None, A, B, and C.
- **Diary time**: ordinary times belong in a Timestamp. A Diary expression has no concrete time, so `HH:mm` or `HH:mm-HH:mm` may appear at the end of the title before tags.
- **Tags**: editing preserves the `ATTACH` tag used by Org Attach.

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
- `ASTER_ANNIVERSARY_DISPLAY` accepts `years` and `elapsed-days`; missing or unsupported values default to `years`. It changes only Aster's derived presentation and does not duplicate the date. `elapsed-days` shows “Elapsed N days” plus “Next anniversary in N days,” while Org Agenda still receives only the real annual occurrence.
- Preferred `org-cyclic` takes `interval-days year month day`. It always represents a cyclic Event; interval `1` appears every day, and `%d` is the completed cycle count.
- To appear only every 100 days, use `%%(org-cyclic 100 2023 8 28) Married for the %dth hundred days`. Here `%d` counts completed 100-day cycles.
- Aster remains compatible with `diary-cyclic` in `interval-days month day year` order. Editing preserves the original function and its corresponding argument order, while new examples prefer `org-cyclic` because it is independent of `calendar-date-style`.
- Both remain keyword-free source entries and never acquire a TODO state.
- Enable **Settings → Tasks & Workflow → Views → Anniversaries** to collect the next annual occurrence together with completed years or elapsed days in an optional Perspective.
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

## 13. Write-Back Boundaries

Aster's semantic edits do not reformat the entire file. A state, date, priority, tag, reminder, Note, or attachment operation modifies only the headline, planning line, properties, LOGBOOK, or attachment links it owns. Unknown properties, body text, subtrees, and other source content remain unchanged.

The Source entry at the bottom of item detail shows the complete saved Org subtree. Files → Edit shows and edits the complete source file.
