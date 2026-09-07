<p align="right">🌐 <a href="../platforms.md">简体中文</a> · <strong>English</strong></p>

# Platforms and Differences

[Back to English home](../../README.en.md) · [Quick Start](quick-start.md)

Aster shares its Org parsing, classification, write-back, sync-conflict, Agenda, TODOs, Journal, Search, and attachment semantics across Apple and Android platforms. The UI is not a pixel copy of another platform: information hierarchy and outcomes stay aligned, while navigation, transient surfaces, permissions, and system integrations remain native to each platform.

## Current Release Status

| Platform | Minimum OS | Current baseline | Distribution status |
| --- | --- | --- | --- |
| iPhone / iPad | iOS / iPadOS 17 | 0.1 (13) | Uploaded; awaiting TestFlight processing |
| Android phone / tablet | Android 8.0 (API 26) | 0.1 (9) | Preparing Google Play testing; not publicly available yet |

Build/Version Code may advance independently on each platform. Include the platform and complete version in reports; matching marketing versions do not imply identical system capabilities or fixes.

### Date Editing and Journal Images (iOS/iPadOS build 13)

Task Scheduled and Deadline fields now load and save independently without replacing each other. Failed saves retain input during the running session. Metadata, repeating date ranges, and attachment handling during moves and archives are also corrected; see [Org source and results](org-and-aster.md).

Consecutive Journal images use fixed-width cards that sit side by side when space permits and scroll horizontally when needed. Position marks appear above them, and tapping opens the complete original. See [Journal and Capture](journal-capture.md). These fixes do not imply that Android build 9 includes the same behavior.

### iPad Search Correction (iOS/iPadOS build 12)

Selecting a different task in wide Search results refreshes the detail pane instead of retaining the first task's form. Edits made before switching save to the previous task, not the next one. The query and result list stay in place.

Per-folder sorting is also included in iOS/iPadOS build 12; see [file tree sorting](files-preview-attachments.md) for the controls and date rules. These changes do not imply that the Android version includes them.

### Classic Root-Bar Correction (iOS/iPadOS build 12)

Build 12 separates classic and Liquid Glass root-bar layout. iOS 17/18 uses a material capsule with its own bottom-safe-area handling to prevent the central icons from sitting too high. iOS 26 and later retains native Liquid Glass. This change does not alter button order or navigation.

Light/dark layout and navigation were checked on iPhone and iPad simulators running iOS 18.6. The reported physical device running iOS 18.7.8 still needs confirmation after updating.

## Shared Product Boundaries

- Org files and adjacent Org Attach `data/` directories remain authoritative.
- Event, Task, Project, Habit, Note, Container, and custom Workflow classification comes from Org semantics rather than a platform or fixed English keyword.
- Agenda calendar selection, timeline position, and Overdue navigation share one selected date. Journal keeps its source-backed selected date distinct from a browsing-only position.
- A status change writes only the complete keyword explicitly selected by the user. Agenda and TODOs do not assign horizontal swipes to Workflow.
- Local indexes, reminder schedules, and widget content are rebuildable projections, never a second source of truth.
- Files exposes only the connected cloud workspace. It does not present the private app working copy or generic local Import as another provider.

In iOS/iPadOS build 12, widgets expire an old Habit day instead of counting it as ordinary overdue work. Unfinished ordinary tasks keep their overdue count. Fresh Habit content still comes from Aster's source-derived refresh when the app runs.

## Platform Differences

| Capability | iOS / iPadOS | Android |
| --- | --- | --- |
| Phone navigation | iPhone uses the bottom root command bar | Android phones use bottom navigation and platform-native transient surfaces |
| Large-screen navigation | iPad uses a horizontal root command bar and master/detail content | Android tablets and wide windows use a leading Navigation Rail; Agenda calendar and timeline remain in the left content pane |
| Cloud providers | Dropbox, iCloud Drive, Nutstore, WebDAV | Dropbox, Nutstore, WebDAV; no iCloud Drive or generic local Import |
| Notifications | Aster Notifications, with optional Apple Reminders interoperability | Aster Notifications use Android notification and alarm capabilities; there is no invented Apple Reminders equivalent |
| System calendar | Reads user-approved system calendar overlays | Reads user-approved Android Calendar Provider overlays |
| File preview | Uses native iOS/iPadOS document capabilities | Uses Android-native readers and, when needed, a compatible system app |
| Credentials | System Keychain | Android Keystore |
| Accessibility | VoiceOver, Dynamic Type, Reduce Motion | TalkBack, system font scaling, and system animation accessibility settings |

## Why Reports Need a Platform

An inconsistent Org result should usually be checked across both platforms. Navigation placement, sheets, permissions, notification delivery, and file-opening failures may be platform-specific. Every report should identify the platform, device, OS version, Aster version, and workspace provider. For Android, also say whether the run used a physical device or emulator and a phone or tablet/wide layout.

See [Submit Feedback](feedback.md) for the complete fields.
