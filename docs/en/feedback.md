# Submit Feedback

[简体中文](../feedback.md) · **English**

[Back to English home](../../README.en.md)

A useful report lets another device reproduce the same result without exposing your real workspace.

## Choose an Entry

- For rendering problems, crashes, missing write-back, sync failures, or severe slowness, use the [English Bug report](https://github.com/LuciusChen/aster-feedback/issues/new?template=bug-en.yml).
- For a new use case, interaction improvement, or Org capability, use the [English Feature request](https://github.com/LuciusChen/aster-feedback/issues/new?template=feature-en.yml).

Search existing issues first so evidence for one problem is not fragmented across several reports.

## Minimum Information for a Bug

1. Aster version and build, such as `0.1 (1)`.
2. Device and operating system, such as `iPhone 17 Pro · iOS 26.0`.
3. Workspace provider: Dropbox, iCloud Drive, Nutstore, or WebDAV.
4. The starting page and exact sequence of actions.
5. Expected and actual results.
6. Minimal, redacted Org source.
7. A screenshot or short recording when needed.

## Providing a Minimal Org Reproduction

Prefer one independent heading over a complete file:

```org
#+TODO: TODO NEXT | DONE

* TODO Example :demo:
SCHEDULED: <2026-09-03 Thu 09:15>
:PROPERTIES:
:STYLE: habit
:END:
```

Keep the syntax that triggers the problem, but replace real titles, tags, links, IDs, paths, and body text. Add only the smallest parent/child outline needed to reproduce a hierarchy bug.

### Attachment Problems

Include:

- File extension and approximate size.
- Whether the heading has an `ID`, `ATTACH` tag, and `attachment:` link.
- Whether the failure occurs during creation, in detail, when opening from Files, or after sync.

You do not need to publish the original private attachment. Reproduce with a blank image or test PDF when possible.

### Sync Problems

Say whether the error occurs during connection, listing, upload, download, deletion, conflict resolution, or reauthorization. Include the network state and the last known successful sync. Never post account passwords, tokens, or complete authorization URLs.

### Performance Problems

Include:

- The operation and measured wait, such as “plain-text Capture takes about 10 seconds.”
- Approximate file count, directory count, and target file size.
- Whether it happens every time or only on first use or after sync.
- Whether the UI remains scrollable and cancellable during the wait.

## Writing a Feature Request

Describe the use case and Org semantics before proposing the control:

```text
Scenario: I have a date-only Task and want an alert at 09:00 that day.
Current behavior: The Task stays in TODOs, but reminder configuration is hard to discover.
Desired result: Turning on “Remind me on a day” in detail reveals the alert time.
Org mapping: Keep the DEADLINE date and represent the alert with Aster reminder properties.
```

This is easier to evaluate than “add a button,” because it exposes possible interactions with Event, Task, Repeat, Diary, and write-back rules.

## Data-Safety Priority

Stop editing the affected file and report immediately if:

- Unsupported Org text, a drawer, or a block is deleted.
- A state or date edit overwrites an unrelated subtree.
- An attachment is copied without a source link, or a source link is written without the file.
- A conflict is silently overwritten without preserving recoverable copies.
- The UI reports success while the source in Files remains unchanged.

Copy the affected file and adjacent `data/` directory before attempting recovery or reconnection.
