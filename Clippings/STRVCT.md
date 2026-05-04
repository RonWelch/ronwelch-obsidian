---
title: "STRVCT"
source: "https://strvct.net/"
author:
published:
created: 2026-04-21
description: "A naked objects framework for JavaScript with automatic UI generation, persistence, and cloud sync."
tags:
  - "clippings"
---
A [[Naked Objects]] framework for JavaScript with automatic UI generation, persistence, and cloud sync.

## About

[[STRVCT]] implements the [naked objects](http://downloads.nakedobjects.net/resources/Pawson%20thesis.pdf) pattern for JavaScript. You write domain model classes with annotated properties — the framework automatically generates the user interface, handles persistence, and keeps everything synchronized.

The model layer is completely independent of the UI, allowing the same application code to run in a browser, on a command line, or headlessly in Node.js for testing and server-side processing.

For a detailed treatment of this approach, see [Closing the Usability Gap in Naked Objects](https://strvct.net/docs/Naked%20Objects/index.html).

## Key Features

- **Automatic UI** — Views are generated from the model graph using a Miller Column inspired navigation pattern. Custom views can override defaults when needed.
- **Slot System** — Properties are declared as slots with annotations that control type checking, persistence, UI generation, view synchronization, and other features.
- **Notification-Based Sync** — The model layer communicates to the views layer through deferred, deduplicated notifications with automatic sync loop detection.
- **Transparent Persistence** — No custom serialization code required. Declare which objects and slots to persist, and the framework handles dirty tracking, automatic commits, and garbage collection via IndexedDB.
- **Cloud Sync** — Object pools sync to Firebase Storage using a write-ahead log of delta files for efficient incremental updates.
- **Gesture System** — Unifies mouse and touch input gestures.
- **Internationalization** — AI-powered translation of UI text with no per-component wiring. The naked objects pattern means every slot value is translated at the model-to-view boundary automatically.
- **Accessibility** — Automatic ARIA roles, labels, and states derived from the model. Slot metadata maps to aria-required, aria-valuemin/max, and aria-description; view classes emit landmark roles, live regions, and focus-visible styling.
- **Content-Addressable Build** — Custom resource loading with content hashing for optimal caching and minimal network transfers.

### [Example App](https://strvct.net/Example%20App/index.html)

A complete contacts application in four classes, with zero view code.

View →

### [FAQ](https://strvct.net/FAQ/index.html)

Frequently asked questions about STRVCT.

View →

### [Documentation](https://strvct.net/docs/index.html)

Guides, architecture, and API reference.

View →

### [Project Timeline](https://strvct.net/Timeline/index.html)

Key milestones from the first commit through present day

View →

### [GitHub Repository](https://github.com/stevedekorte/strvct.net)

Source code, issues, and contributions.

View →

For AI agents: [llms.txt](https://strvct.net/llms.txt) (curated index) and [llms-full.txt](https://strvct.net/llms-full.txt) (full content).

See [[STRVT Technical Overview]] 