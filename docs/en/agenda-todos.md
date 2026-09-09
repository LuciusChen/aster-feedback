<p align="right">🌐 <a href="../agenda-todos.md">简体中文</a> · <strong>English</strong></p>

# Agenda and TODOs

[Back to English home](../../README.en.md) · [Org classification reference](org-and-aster.md)

Agenda and TODOs read the same Org files but answer different questions:

- **Agenda**: When does it happen? It shows Events, unfinished Tasks with a concrete clock time, and today's eligible Habits whether or not they have a clock time.
- **TODOs**: What remains unfinished? It shows non-Habit Workflow items with no date or only a date, but no clock time.

![Agenda example](../../assets/screenshots/agenda.png)

## What Appears in Agenda

| Item | In Agenda? | Presentation |
| --- | --- | --- |
| All-day Event without a Workflow keyword | Yes | Compact all-day strip |
| Time-range Event without a Workflow keyword | Yes | Time range above, title below |
| Date-only Task with a Workflow keyword | No | Remains in TODOs |
| Timed Task with a Workflow keyword | Yes | Exact time above, Workflow item below |
| Habit inside its actionable window | Yes | Workflow, Habit cadence, and history |
| Ordinary Note or Container | No | Available in Files and Search |

## What Appears in TODOs

![TODOs example](../../assets/screenshots/todos.png)

TODOs contains every unfinished non-Habit Workflow item without a concrete clock time, including:

- A completely undated Task.
- A date-only `SCHEDULED` Task.
- A date-only `DEADLINE` Task.
- A Workflow item configured as a Project.

Habit is the exception: by default, it appears in today's Agenda timeline only after reaching its actionable window, even without a clock time. It is never moved into or duplicated in TODOs.

The list follows real files and Org outline hierarchy. Visible child Tasks remain nested below a visible parent Task. Structural Containers are not misrepresented as Tasks.

### Reviewing Future Habits (iOS/iPadOS build 14)

Settings → Calendar & Agenda offers **Show all habits today**, off by default. Enabling it also shows not-yet-scheduled habits in today's list, labeled **Not due · Next date**, with the time when present.

This only changes visibility. It does not trigger early notifications, add badges or calendar markers, or edit Org dates and repeat rules. A habit completed today remains visible once, with the existing duplicate check-in guard.

To review habits separately, create a normal Perspective and select only the Habit type. It includes future habits regardless of this switch, while respecting Agenda sources, archive exclusions, and your filters. There is no additional Habit module.

iOS/iPadOS build 14 also fixes the iPad editor unexpectedly closing while entering a Perspective name. Showing or dismissing the keyboard no longer switches the page layout, and the active draft stays intact.

## Overdue Rules

In iOS/iPadOS build 14, changing the device's time zone or reopening Aster in a new zone reparses dates and refreshes Agenda, Journal, reminders, and badges. An ordinary Org `09:00` stays nine in the morning locally instead of becoming yesterday through a stale cache. Unsaved editor text is preserved; widgets do not use previous-zone data while awaiting a fresh snapshot.

A system reminder with an explicit zone keeps its original instant. See the [time-zone source examples](org-and-aster.md#16-time-zones-and-travel-iosipados-build-14).

- Only unfinished Workflow items can be overdue.
- A date-only Task stays in TODOs and shows compact `1 day overdue` or `N days overdue` status; Aster does not move it to today.
- After a timed Task passes its clock time, it shows `Overdue`; on later calendar days it shows the elapsed overdue-day count, and its Agenda time line also turns red.
- `SCHEDULED` and `DEADLINE` use the same overdue-day rule. Presentation does not change according to whether Aster created the item or Apple Reminders synchronized it.
- Today's header may summarize historical overdue items. Tapping the summary animates to the nearest overdue item on its original date.
- Multiple overdue items stay on their respective dates so you can continue scrolling upward; Aster does not merge them into a fabricated task.
- Events without Workflow keywords do not use Task overdue semantics.

## Choosing Event or Task When Creating

The Agenda `+` opens quick creation. The two-position control determines Org type and does not switch automatically merely because a date or time was entered:

| Selection | Org meaning | Time precision |
| --- | --- | --- |
| Calendar icon | Event, no Workflow keyword | All-day or start–end range |
| Checkmark icon | Task, selected Workflow keyword | Undated; each planning date can include a time |

### Event

- No time: write an all-day active timestamp.
- An Event exposes independent start and end dates. One day remains one active
  timestamp; multiple days use the standard Org range:

  ```org
  * Trip
  <2026-08-28 Fri>--<2026-08-30 Sun>
  ```

- Only `9:00`: default to `09:00-10:00`.
- `9:00-10:15`: preserve the explicit range.
- A timed multi-day Event likewise uses one standard active-timestamp range,
  such as `<2026-08-28 Fri 09:00>--<2026-08-30 Sun 18:00>`.
- An implicit time already in the past moves to the next day; an explicitly selected date remains unchanged.

### Task

- Neither Scheduled nor Deadline set: write an undated Task that appears in TODOs.
- Set `SCHEDULED`, `DEADLINE`, or both. Without a clock time, the Task remains in TODOs.
- Enable At a time for the selected field to write its clock time, show it in Agenda, and alert at that time.
- A Task has no end time; it is not an Event interval.

A date-only Task can preview a full day during creation without appearing in Agenda. An undated Task has no timeline preview; see [Independent Scheduled and Deadline fields](#independent-scheduled-and-deadline-fields-next-build) for what happens when a date is removed.

## What the Creation Surface Exposes

Creation and detail keep shared fields in the same order. The title stays above the scrolling controls, followed by:

- Item Type, then Task Workflow state and progress when applicable
- Priority
- Tags
- Independent Task Scheduled and Deadline fields, edited through the same inline date, time, and repeat controls
- Event start/end dates and optional interval times
- Event and Task standard Org Repeat or Weekday Diary; each Task date retains its own rule
- Pending attachments
- One Note written to LOGBOOK
- The read-only Event & Task Inbox destination

The title stays above one scrolling region for the setting entry points, without Show More/Show Less. Scheduled and Deadline are edited separately in that form, without entering date subpages.

A long title wraps to the available phone width with no line-count cap and without storing a newline in the Org headline. The title inside the timeline preview stays on one line and tail-truncates within the strip instead of widening the page.

The timeline reduces tick-label density to fit the actual font and localized time text, including AM/PM at larger sizes. Endpoint labels remain fully visible, and the Event interval or Task time stays in the same position.

On daylight-saving transition days, automatic positioning follows the actual ticks, including in time zones with half-hour changes. Non-whole-hour ticks show their minutes, and a final segment shorter than an hour keeps its real width.

The creation timeline sits in the date area alongside the highlighted draft. Creating an Event shows only that day's Events; creating a Task shows only timed Tasks. Switching type or date refreshes the context immediately. Events occupy their actual intervals, Tasks are time points, and you can scroll horizontally to see other hours.

In Event capture, overnight Events show their overlap with that day and all-day Events use a separate compact strip. Task capture hides those Events; no-clock TODOs still do not occupy the time axis.

Displayed clocks follow the system's 12/24-hour preference, without another Aster setting. Org timestamps still use `HH:mm`, and literal titles and template text are unchanged. Returning to Aster after changing the system format also refreshes pending notification text without moving the reminders.

Long WebDAV URLs, file and folder names, Journal filename rules, Capture paths and prompt answers, Agenda tags, and Perspective filters follow the same wrapping rule. The timeline keeps its faint flat background to indicate the time range. Add still commits the headline, planning, properties, Note, links, and attachment copies as one transaction.

## Calendar and Timeline

- Month is the first-launch default. After that, Aster restores the last selected Week, Month, or Year size even after the app is terminated and relaunched. The grabber moves only one adjacent step at a time.
- Agenda and Journal Month use localized three-letter-equivalent weekday headers such as `Mon`, `Tue`, and `Wed` in both compact phone and wide iPad layouts instead of ambiguous single-letter labels.
- Agenda and Journal Week reuse the same five-column represented-date rail rather than a conventional seven-day page. Agenda supplies its selected date, dates containing non-Habit Agenda items, and external all-day calendar dates; Journal supplies its selected date, prepared entry dates, and that same external overlay. When no content date exists in one direction, one adjacent empty date remains available, so either calendar can continue into the past or future beyond its earliest or latest record. A long-running source contributes only the dates needed near the current browsing position instead of expanding its complete range. An overlapping ribbon receives a row only when that row is free across its complete span; overflow beyond the fixed display cap is omitted rather than overdrawn. An omitted gap uses one aligned torn edge, weekends retain calendar shading, and every ordinary boundary reaches the rail baseline. Single-day and cross-day source appears as unlabelled color ribbons; an Agenda Workflow day overlays its symbol beside the centered weekday without taking layout space. Every committed horizontal release moves exactly five calendar days regardless of drag distance and uses the same directional full-page transition as Month and Year. Only during the drag, content inside the leading column receives one crisp copy that progressively magnifies by up to 6% and shifts slightly beneath a clear, unblurred lens. The scale remains inside that column's fixed mask; only the lens's trailing optical edge is exposed, while the background, weekend shading, and date boundaries remain fixed. At rest there is no visible glass layer, tint, blur, gradient, edge, or duplicate.
- A Week release performs one five-day page transition with no second settle or spring rebound. In iPad landscape, the left column uses the same five-column rail as iPhone while the synchronized month calendar remains on the right. On a wide Android tablet, calendar and timeline likewise stay in the left content pane beside the Navigation Rail.
- The calendar remains fixed while the timeline scrolls independently and extends into both past and future.
- Today remains aligned to the top initially, with past dates already above it, so the first drag is native scrolling rather than an unlock gesture.
- As the timeline's leading day changes, calendar selection follows.
- On iPhone and Android phones, the date header, calendar, and timeline share fixed 18-point/18-dp margins on both sides. Custom fonts or a very long all-day item truncate inside the item's rounded chip instead of widening or shifting the page. After visiting Files, Journal, Search, or Settings and returning to Agenda, retained background pages can no longer shift either Agenda or root navigation.
- Week, Month, and Year use the same directional full-page transition. iOS Reduce Motion or Android's system animation accessibility setting reduces spatial movement without changing the result.
- iPad landscape places the full timeline and five-column Week rail on the left and a synchronized month calendar on the right. A wide Android tablet uses a leading Navigation Rail, then an Agenda left pane for the date, calendar, and timeline, with a linked month grid or selected detail on the right. TODOs and Perspectives do not retain an irrelevant large calendar.

## Item Actions

- Tap a Task's leading Workflow symbol or current keyword capsule to open the same complete state list directly. Neither guesses a Complete, Reopen, or Habit Check-in action; the user chooses the intended keyword explicitly.
- Tap the title or any other remaining area to open the shared detail view. The keyword hit shape follows the visible capsule instead of expanding into title space. iPhone uses a bottom sheet, iPad anchors a popover, and Android uses a scrollable native status sheet. A choice writes immediately and dismisses without another Done action. The current state is indicated by its own tint and fill, with no extra checkmark. After a non-repeating item enters a Terminal state, it stays in place briefly to confirm the new state and then leaves the active list.
- Hold a Task to open only compact item actions such as Schedule, Move/Archive, Note, and Delete; the keyword list is not duplicated there. Android anchors its native popup near the pressed row.
- Hold an Event to open its schedule, move/archive, Note, and delete actions.
- Agenda and TODOs do not use horizontal swipes to change Workflow, avoiding conflicts with scrolling and system gestures.
- Leaving detail writes the current valid draft back to source. There is no separate Save button.

The main detail order is: Item/History → Schedule/Repeat/Reminder → Content → Attachments → Notes → Subtasks → Delete. Delete is red and the page reserves enough bottom space above root navigation.

In the next iPhone/iPad build, the editable title uses a slightly larger semibold style to distinguish it from the settings below. Short and long titles share that hierarchy and follow the selected font and system Text Size; field order and saving are unchanged.

Titles wrap to the available width rather than switching controls at a character-count threshold. Light mode, dark mode, and larger text follow the same rule; visual wrapping does not change the Org headline source.

The same build removes the icons from Add Attachment and Delete in detail. Add Attachment stays left-aligned. Delete is centered, bold, and red, and still asks for confirmation.

In iPad landscape, Agenda detail keeps its source button in the right pane without requiring rotation. Back saves before restoring the month calendar; a failed save keeps the input and detail open.

### Independent Scheduled and Deadline fields (next build)

New and existing Tasks show Scheduled and Deadline as independent sections in the original form. Both switches can be on together, with each section displaying its own date, time, and repeat rule. There is no editing-mode selector.

Each switch enables or removes only its own field. Both sections use the same controls but retain separate values. At a time places the clock immediately after the date in that section's Date row. Its own row contains only the toggle, with no duplicate time picker.

During creation, enabling At a time defaults to the next whole hour: at 14:35, the selected day gets 15:00. Scheduled and Deadline apply this independently, matching Event creation. If Today crosses midnight at the next hour, the default is tomorrow at 00:00. Editing an existing clock does not round it again.

Scheduled is when you plan to start working; Deadline is when the work should be finished. These are independent planning fields, not different task types. Neither requires a clock time; a date-only Task remains in TODOs.

Dates use the system picker directly, without Scheduled or Deadline child pages. Add saves a new Task, and leaving detail saves an existing one.

When creating an item, switching Event/Task retains the unsaved Task dates and Event interval separately.

Repeat opens the shared rule sheet from the original form. Done or dismissing it returns to the same form without saving the Task.

When choosing a weekly repeat, trying other weekdays and returning to the original keeps the original date. Closing Repeat resolves the final selection once; intermediate taps do not advance the task to later weeks.

Weekdays, Weekends, and custom weekday combinations use a shared headline clock. If both fields use these rules, their times and all-day settings must agree. Conflicts show an explanation and retain your input without writing. Use ordinary timestamp repeaters or separate tasks when different clocks are needed.

Removing one date preserves the other. Browsing dates or editing only the title or tags leaves unchanged timestamps untouched. Scheduled later than Deadline shows a quiet hint without changing either value.

When creating a Task, the date summary and timeline follow the last edited enabled field. Removing it falls back to the remaining date, preserving its all-day or timed display. Merely viewing another section does not change the preview. The preview disappears only when both dates are removed.

When adding both dates, Aster writes them on the same standard Org planning line immediately after the heading. A new CLOSED timestamp joins that line too, so Emacs recognizes all the fields.

If source already contains multiple fields of the same kind, editing that date asks you to review the Org source first. Aster does not guess which conflicting value to delete. See the [two-date Org example](org-and-aster.md#both-a-scheduled-date-and-a-deadline).

## Saved View Entry

Open the three-line secondary menu immediately before Views. Its visible top-to-bottom order is:

- Agenda
- TODOs
- Journal
- A saved Perspective

Views remains the primary destination while the secondary menu changes only its content. Aster does not provide a hard-coded Projects screen because each workspace can use different Workflow keywords for projects. Projects and Anniversaries under **Settings → Tasks & Workflow → Views** are optional templates that create ordinary editable Perspectives.
