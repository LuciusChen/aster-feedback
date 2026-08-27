# Agenda and TODOs

[简体中文](../agenda-todos.md) · **English**

[Back to English home](../../README.en.md) · [Org classification reference](org-and-aster.md)

Agenda and TODOs read the same Org files but answer different questions:

- **Agenda**: When does it happen? It shows Events and unfinished Tasks with a concrete clock time.
- **TODOs**: What remains unfinished? It shows Workflow items with no date or only a date, but no clock time.

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

TODOs contains every unfinished Workflow item without a concrete clock time, including:

- A completely undated Task.
- A date-only `SCHEDULED` Task.
- A date-only `DEADLINE` Task.
- A Workflow item configured as a Project.
- A Habit that does not currently need a timeline position.

The list follows real files and Org outline hierarchy. Visible child Tasks remain nested below a visible parent Task. Structural Containers are not misrepresented as Tasks.

## Overdue Rules

- Only unfinished Workflow items can be overdue.
- A date-only Task remains on its original date in TODOs and receives compact overdue metadata; Aster does not move it to today.
- When a timed Task passes its time, the time line in Agenda turns red.
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
- Only `9:00`: default to `09:00-10:00`.
- `9:00-10:15`: preserve the explicit range.
- An implicit time already in the past moves to the next day; an explicitly selected date remains unchanged.

### Task

- Schedule off: write an undated Task that appears in TODOs.
- Date only: write a date-only `SCHEDULED` or `DEADLINE` value that remains in TODOs.
- At a time on: write one exact time that appears in Agenda and alerts at that time.
- A Task has no end time; it is not an Event interval.

## What Show More Contains

The default creation surface keeps only the title, date, type, and necessary timeline visible. **Show More** exposes:

- Task Workflow state
- Task-only planning type
- Date and time
- Priority
- Tags
- Standard Org Repeat or Weekday Diary
- Pending attachments
- One Note written to LOGBOOK
- The read-only Capture Inbox destination

Collapsing Show More does not discard entered values. Tapping Add commits the headline, planning, properties, Note, links, and attachment copies as one transaction.

## Calendar and Timeline

- Month is the default view. The grabber moves only one adjacent step among Week, Month, and Year.
- The calendar remains fixed while the timeline scrolls independently and extends into both past and future.
- As the timeline's leading day changes, calendar selection follows.
- Crossing a week, month, or year uses directional animation; Reduce Motion reduces movement.
- iPad landscape places the full timeline on the left and a synchronized month calendar on the right. TODOs and Perspectives do not retain an irrelevant large calendar.

## Item Actions

- Tap any Task or Event row to open the shared detail view.
- Hold a Task to open grouped Workflow and Actions choices.
- Hold an Event to open its schedule, move/archive, Note, and delete actions.
- Agenda and TODOs do not use horizontal swipes to change Workflow, avoiding conflicts with scrolling and system gestures.
- Leaving detail writes the current valid draft back to source. There is no separate Save button.

The main detail order is: Item/History → Schedule/Repeat/Reminder → Content → Attachments → Notes → Subtasks → Delete. Delete is red and the page reserves enough bottom space above root navigation.

## Saved View Entry

Hold the bottom Agenda item to select:

- Agenda
- TODOs
- A saved Perspective

The tab label and symbol follow the current view. Aster does not provide a hard-coded Projects screen because each workspace can use different Workflow keywords for projects.
