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

Under **Settings → Tasks & Workflow → Workflow**, configure order, Process/Terminal role, symbol, color, shortcut, and state history. These native controls still map to standard Org declarations.

## Project Is Not a Fixed English Keyword

Enable **Treat as Project** on a Process state to give headings in that state Project semantics. The default `PROJECT` is only an initial convention; `PRO`, `ACTIVE`, or a non-English keyword can serve the same role.

```org
* PROJECT Ship TestFlight [2/3]
:PROPERTIES:
:COOKIE_DATA: todo
:END:
** DONE Register identifiers
** DONE Upload build
** NEXT Invite testers
```

A Project remains a Workflow item, so it appears in TODOs and matching Perspectives rather than a hard-coded Projects screen.

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

- Include one or more of Task, Project, and Habit.
- Match any listed Workflow keyword.
- Match tags.
- Choose tag matching behavior.
- Filter by Priority.
- Filter by date state.
- Choose sorting.
- Preserve or flatten parent/child outline relationships.

Create one under **Settings → Tasks & Workflow → Perspectives**. It then appears alongside Agenda and TODOs when you hold the bottom Agenda item. The visible tab label and symbol follow the selected view.

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

## What a Perspective Does Not Own

- Org Repeat and Diary rules remain in source files.
- Notifications derive from each item's own standard timed planning; legacy persistent-reminder properties also remain outside Perspective ownership.
- The Apple Reminders bridge is a separate integration.
- A Perspective does not copy items, change Workflow, or create a proprietary project database.
