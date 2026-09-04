<p align="right">🌐 <a href="../workflow-project-habit-perspective.md">简体中文</a> · <strong>English</strong></p>

# Workflow, Project, Habit, and Perspective

[Back to English home](../../README.en.md) · [Org classification reference](org-and-aster.md)

Aster separates state, project identity, habit behavior, and saved views into four semantic layers. Workflow comes from Org, Project is a role assigned to selected active states, Habit is a feature of a repeating Task, and Perspective is only a saved filter and presentation rule.

## Workflow Follows Org

A minimal Workflow:

```org
#+TODO: TODO | DONE
```

A more complete example:

```org
#+TODO: TODO(t) NEXT(n) PROJECT(p) WAITING(w@/!) | DONE(d!) CANCELLED(c@)
```

- States before `|` are Process; states after it are Terminal.
- Parentheses may contain a fast key and entry/exit logging rules.
- Aster preserves complete custom keywords instead of collapsing them into fixed TODO/DONE values.
- A file-local `#+TODO:` or `#+SEQ_TODO:` takes precedence over the app default.
- `#+STARTUP:` and the nearest subtree `LOGGING` property may further override completion logging.

Under **Settings → Tasks & Workflow → Workflow**, configure order, Process/Terminal role, symbol, color, shortcut, and state history. The Color row uses the native circular color well, so each state can use any RGB color instead of a fixed set of swatches. Every valid change to an existing state saves automatically and Back flushes the latest valid value, so editing has no Done action; only a new state requires **Add**. These native controls still map to standard Org declarations.

### Parallel Task Flows and Direct Switching

Org allows one file to define several distinct state sequences in parallel:

```org
#+TODO: TODO(t) | DONE(d)
#+SEQ_TODO: REPORT(r) BUG(b) | FIXED(f)
#+TODO: | CANCELED(c)

* TODO Prepare release
* BUG Crash when opening preview
* CANCELED Retired experiment
```

- A keyword belongs to only one sequence; the keyword on a heading identifies the flow currently used by that heading.
- Either side of `|` may be empty. `TODO |` is a Process-only flow and `| CANCELED` is a Terminal-only flow; both are standard Org syntax.
- For these one-sided flows, the ordinary complete/reopen control leaves the source unchanged when the opposite side has no declared keyword. Aster never invents `DONE` or `TODO`; use the status picker to switch explicitly to another flow instead.
- When one flow applies, Aster keeps the compact Process/Terminal picker. With multiple flows, the same picker groups them as **Task Flow 1 / 2 / 3**, so one tap can change `TODO` directly to `REPORT`, `FIXED`, or `CANCELED`.
- A switch replaces only the current keyword on the headline—for example, `* TODO Title` becomes `* REPORT Title`. Aster neither deletes the former flow nor changes other headings, and it writes no private sequence ID.
- Selecting a Terminal state from another flow still derives completion, `CLOSED:`, and entry/exit logging from the corresponding Org definitions.
- For a repeating heading, completing `TODO` directly through another flow's `FIXED` advances the repeat date and returns to the original flow's `REPEAT_TO_STATE`, configured repeat target, or first Process state, matching `org-todo`; Aster does not silently change it to `REPORT`.

File-local declarations affect only their file. In their absence, Aster uses the global Task Flows under **Settings → Tasks & Workflow → Workflow**. Advanced Org Syntax edits each complete definition token-for-token, while the structured editor covers common state, appearance, and logging changes.

### Timeline Shortcuts

Each timeline item has two compact status triggers: tap either the leading Workflow icon or the disclosed keyword capsule to open the complete state list; tap the title or remaining row to open detail. Touch and hold retains only item actions such as Schedule, Move/Archive, Note, and Delete, without duplicating the keyword list. Horizontal swipes remain unassigned to Workflow so they cannot conflict with Week navigation.

- The Workflow icon identifies the exact current state and acts as a second explicit trigger for the complete state list; it does not guess a Complete, Reopen, or Habit Check-in action. Choose the intended keyword explicitly from that list. Project guards, Habit same-day protection, and ordinary Org Repeaters retain their shared mutation ownership; repeating Apple Reminders exist only in the separate iOS/iPadOS interoperability source.
- Tapping the keyword opens a compact bottom sheet on iPhone, an anchored popover on iPad, or a scrollable native status sheet on Android. A choice writes immediately and dismisses without another Done action. Multiple flows remain grouped, while the current keyword is identified only by its own tint and fill, without an extra checkmark.
- After a non-repeating item successfully enters a Terminal state, its source is already saved while the row stays in place briefly to confirm the new state, then fades from the active timeline. A repeating item follows its normal advancement rule instead of showing a fabricated completed copy.
- The keyword hit shape follows the visible capsule instead of expanding into the title or remaining detail area, and it does not steal vertical scrolling. VoiceOver and TalkBack expose Open Details and Change Status separately.

## Project Is Not a Fixed English Keyword

Enable **Treat as Project** on a Process state to give headings in that state Project semantics. The default `PROJECT` is only an initial convention; `PRO`, `ACTIVE`, or a non-English keyword can serve the same role.

```org
* PROJECT Ship mobile beta [2/3]
:PROPERTIES:
:COOKIE_DATA: todo
:END:
** DONE Register identifiers
** DONE Upload build
** NEXT Invite testers
```

A Project remains a Workflow item, so it appears in TODOs and matching Perspectives rather than a hard-coded Projects screen. You can enable the built-in Projects template under **Settings → Tasks & Workflow → Views**; it only creates an ordinary Perspective based on the Treat as Project role, remains editable, and never changes Org source.

### Progress Source

Choose a data source in Project detail:

| Choice | Write-back | Use case |
| --- | --- | --- |
| Automatic | No `COOKIE_DATA` | Only one unambiguous source exists |
| Checklist | `:COOKIE_DATA: checkbox` | `[ ]` and `[X]` in the body |
| Direct subtasks | `:COOKIE_DATA: todo` | Direct child Tasks only |
| All subtasks | `:COOKIE_DATA: todo recursive` | Every descendant Task |

Checklist example:

```org
* PROJECT Publish article [1/3]
:PROPERTIES:
:COOKIE_DATA: checkbox
:END:
- [X] Draft
- [ ] Review
- [ ] Publish
```

If checkboxes and subtasks both exist and Automatic is not unambiguous, Aster asks you to choose instead of silently picking one.

## Habit Is Additional Task Semantics

```org
* NEXT Daily writing :writing:
SCHEDULED: <2026-09-03 Thu 07:30 .+1d>
:PROPERTIES:
:STYLE: habit
:REPEAT_TO_STATE: NEXT
:END:
```

A standard Habit requires:

1. An unfinished Workflow keyword.
2. A repeating `SCHEDULED` timestamp.
3. `:STYLE: habit`.

The Habit row therefore continues to show its real `NEXT`, `TODO`, or other keyword. The consistency graph, cadence, and completion history are supplemental information; they do not replace Workflow with a fake Habit state.

On completion, Aster advances the timestamp according to the Org Repeater, returns to `REPEAT_TO_STATE` or the configured active state, and writes standard LOGBOOK history.

## Perspective Is a Configurable View

A Perspective does not modify Org files or create a new item type. It stores filtering and presentation rules:

- Include one or more of Task, Project, Habit, Event, Anniversary, and Day Counter.
- Match any listed Workflow keyword.
- Match tags.
- Choose tag matching behavior.
- Match one Property by existence, equality, or inequality.
- Filter by Priority.
- Filter by date state.
- Choose sorting.
- Preserve or flatten parent/child outline relationships.

Tag and Property rules use effective values inherited through the outline; a child's Property overrides the same Property on its parent. Preview and Edit still show and mutate only the heading's direct tags and Properties. Leave Property name blank to disable the rule. Does not equal also matches an item where that Property is missing. This is one deliberate mobile condition, not a second full Org query language.

Create one under **Settings → Tasks & Workflow → Views**. It then appears alongside Agenda, TODOs, and Journal in the three-line secondary menu immediately before the Views tab.

### Optional Built-in Templates

Views begins with two switches. They are neither new Org types nor fixed navigation destinations; enabling one creates an ordinary editable Perspective:

- **Projects** matches every active state marked Treat as Project and preserves the outline.
- **Anniversaries** matches standard yearly `org-anniversary` dates and keyword-free yearly Org timestamp repeaters, sorted by their next date; the optional Property changes completed-years or elapsed-days presentation only for `org-anniversary`.

Turning a switch off removes only the Perspective created by that template. It never deletes, moves, or rewrites an Org entry, and a user-created Perspective with the same visible name is not removed.

### Scenario: My Projects

- Include: Project
- Workflow: blank, matching every state configured as Project
- Preserve outline: on

This works without requiring every user to spell the state `PROJECT`.

### Scenario: Important Actions This Week

- Include: Task and Habit
- Priority: A
- Dates: the relevant date range
- Sort: by date

### Scenario: Waiting Work

- Include: Task and Project
- Workflow: `WAITING DELEGATED`
- Preserve outline: as needed

### Scenario: Work Owned by One Person

Declare shared context once on a parent:

```org
* Release :team:
:PROPERTIES:
:OWNER: Alice
:END:
** TODO Prepare release notes
```

Set tag to `team`, Property name to `OWNER`, matching to Equals, and value to `Alice`. The child matches without duplicating inherited metadata.

### Scenario: Anniversaries and Elapsed Days

First enable **Anniversaries** under Views. Use this for a yearly anniversary:

```org
%%(org-anniversary 2020 8 22) Aster is %d years old
```

To make Aster show the total days elapsed from the source date:

```org
* Wedding anniversary
:PROPERTIES:
:ASTER_ANNIVERSARY_DISPLAY: elapsed-days
:END:
%%(org-anniversary 2022 11 2) Wedding anniversary
```

Set `ASTER_ANNIVERSARY_DISPLAY` explicitly to `years` or `elapsed-days`. Only a supported explicit value enables Aster's derived summary: `years` calculates completed years, `elapsed-days` calculates total elapsed days, and both calculate the days until the next anniversary. Org Preview retains `N days · Next in M days`. In Anniversaries, a future source reads `Starts in N days · Month Day` and its source date reads `Starts today`; it does not become a first anniversary until a complete year has elapsed. Later occurrences use their numbered milestone and next date. An explicit mode retains its selected total and describes that same source or next numbered milestone. A keyword-free yearly Org timestamp uses the same continuous, unboxed date rail but keeps ordinary yearly-repeat meaning and receives no invented anniversary number. A missing or unsupported Property enables no elapsed calculation. Aster uses the original date source; it does not generate daily Agenda data, and Org Agenda still shows only the real occurrence. `org-cyclic` always remains a cyclic Event. It uses the stable `interval-days year month day` order: `1` appears daily, `100` appears only on hundred-day boundaries, and `%d` is the completed cycle count rather than total days. Aster never executes other Lisp, and editing replaces only the corresponding Diary source line.

## What a Perspective Does Not Own

- Org Repeat and Diary rules remain in source files.
- Notifications derive from each item's own standard timed planning; legacy persistent-reminder properties also remain outside Perspective ownership.
- The Apple Reminders bridge exists only on iOS/iPadOS as a separate system source synchronized through managed `apple-reminders.org`; ordinary Org tasks are not exported. Android creates no corresponding bridge. Aster local notifications on both platforms cover concretely timed items in the current Agenda sources.
- A Perspective does not copy items, change Workflow, or create a proprietary project database.
