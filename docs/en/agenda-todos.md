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

Habit is the exception: once it reaches its actionable window, it appears only in today's Agenda timeline even without a clock time. It is never moved into or duplicated in TODOs, and a Habit outside its actionable window appears in neither surface.

The list follows real files and Org outline hierarchy. Visible child Tasks remain nested below a visible parent Task. Structural Containers are not misrepresented as Tasks.

## Overdue Rules

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
| Checkmark icon | Task, selected Workflow keyword | Undated, date-only, or one exact time |

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

- Schedule off: write an undated Task that appears in TODOs.
- Date only: write a date-only `SCHEDULED` or `DEADLINE` value that remains in TODOs.
- At a time on: write one exact time that appears in Agenda and alerts at that time.
- A Task has no end time; it is not an Event interval.

The horizontal timeline remains visible after switching to Task. A date-only Task spans the day in the creation preview, but that visual feedback does not change its Org meaning; only enabling At a time places it in Agenda at an exact point.

## What the Creation Surface Exposes

The same scrollable creation surface directly exposes:

- Task Workflow state
- Task-only planning type
- Event start/end dates and optional interval times, or one Task date and optional exact time
- Priority
- Tags
- Standard Org Repeat or Weekday Diary
- Pending attachments
- One Note written to LOGBOOK
- The read-only Event & Task Inbox destination

The title and these fields now sit directly on one scroll surface without Show More/Show Less or extra custom card backgrounds. A long title wraps to the available phone width and shows as many visual lines as the complete value requires, with no line-count cap and without storing a newline in the Org headline. Its horizontal timeline preview remains one line and tail-truncates inside the strip instead of widening the page. Long WebDAV URLs, file and folder names, Journal filename rules, Capture paths and prompt answers, Agenda tags, and Perspective filters follow the same editing rule. The timeline keeps only its faint flat field so its time range remains legible. Tapping Add still commits the headline, planning, properties, Note, links, and attachment copies as one transaction.

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

## Saved View Entry

Open the three-line secondary menu immediately before Views. Its visible top-to-bottom order is:

- Agenda
- TODOs
- Journal
- A saved Perspective

Views remains the primary destination while the secondary menu changes only its content. Aster does not provide a hard-coded Projects screen because each workspace can use different Workflow keywords for projects. Projects and Anniversaries under **Settings → Tasks & Workflow → Views** are optional templates that create ordinary editable Perspectives.
