# FilePilot — Technical Implementation Plan

**Document:** `docs/TECHNICAL_IMPLEMENTATION_PLAN.md`
**Product:** FilePilot
**Version:** 0.1
**Status:** Draft for Approval
**Platform:** Android
**Framework:** Flutter / Dart

---

# 1. Purpose

This document translates the approved:

* Product Vision
* Product Requirements
* Architecture Blueprint

into a concrete implementation strategy.

It defines:

* What will be built.
* In what order it will be built.
* Which technical decisions must be made.
* Which modules will exist.
* Which files will be created.
* How the application will be tested.
* How implementation will be divided into milestones.

This document does not replace the Product Requirements or Architecture documents.

Those documents remain authoritative.

---

# 2. Implementation Philosophy

FilePilot will be built incrementally.

We will NOT generate the entire application in one step.

Each milestone must produce a coherent, buildable state.

The implementation sequence is:

```text
Foundation
    ↓
Application Shell
    ↓
Core Infrastructure
    ↓
File Engine
    ↓
Explorer
    ↓
Search
    ↓
Storage Analysis
    ↓
Preview / Thumbnails
    ↓
PDF Workspace
    ↓
Screenshot OCR
    ↓
Security
    ↓
Premium / Monetization
    ↓
Hardening
    ↓
Release
```

---

# 3. Development Rule

No production feature will be implemented before:

1. Its requirement is understood.
2. Its architectural location is known.
3. Its dependencies are identified.
4. Its testing strategy is defined.

---

# 4. Technology Baseline

The project will use:

* Flutter
* Dart
* Android as the initial target platform
* Material Design
* Local-first architecture

The exact Flutter/Dart SDK version will be determined from the development environment at bootstrap time.

The project MUST use a supported stable Flutter release.

---

# 5. Package Selection Strategy

Package selection will happen before the corresponding subsystem is implemented.

We will not blindly select packages merely because they are popular.

Each important package will be evaluated against:

* Current maintenance
* Pub.dev health
* GitHub activity where relevant
* License
* Commercial-use compatibility
* Android compatibility
* Flutter compatibility
* Null safety
* Performance
* API stability
* Security history
* Community adoption
* Migration risk
* Ability to replace the package later

Particular care will be applied to:

* Filesystem access
* State management
* Routing
* Persistence/database
* PDF processing
* OCR
* Encryption
* Background processing

---

# 6. Package Decision Categories

The following decisions must be finalized before implementation of the associated subsystem.

## 6.1 State Management

Requirements:

* Testable
* Predictable
* Minimal boilerplate
* Suitable for large applications
* Supports dependency injection
* Supports asynchronous operations
* Supports feature isolation

A final package will be selected and recorded in an ADR.

---

## 6.2 Routing

Requirements:

* Declarative routing
* Nested navigation
* Deep-link capability
* Authentication/security boundaries where required
* Testability

A final routing solution will be selected and recorded in an ADR.

---

## 6.3 Persistence

Requirements:

* Reliable local persistence
* Good performance
* Transaction support where appropriate
* Testability
* Suitable commercial license
* Long-term maintainability

The persistence technology will be selected before the metadata layer is implemented.

---

## 6.4 PDF

The PDF technology MUST be evaluated for:

* Viewing
* Rendering
* Creation
* Page manipulation
* Merging
* Splitting
* Rotation
* Extraction
* Licensing
* Android compatibility
* Performance

The PDF engine MUST remain behind an abstraction.

---

## 6.5 OCR

OCR technology MUST be evaluated for:

* Offline operation
* Accuracy
* Android compatibility
* Language support
* Performance
* Image preprocessing
* Commercial licensing
* Maintenance

The OCR engine MUST remain behind an abstraction.

---

## 6.6 Encryption

Encryption technology MUST be selected only after security review.

No custom cryptography will be developed.

Platform-backed security and established cryptographic libraries will be preferred.

---

# 7. Milestone M0 — Repository Foundation

## Objective

Confirm that the repository and documentation foundation is correct.

Already completed:

```text
docs/
├── PRODUCT_VISION.md
├── PRODUCT_REQUIREMENTS.md
├── ARCHITECTURE.md
├── ROADMAP.md
├── SECURITY.md
└── UI_GUIDELINES.md
```

The implementation-plan document will be added after approval.

No application code is created during M0.

---

# 8. Milestone M1 — Flutter Bootstrap

## Objective

Create the first real FilePilot application.

The application must:

* Build successfully.
* Launch successfully.
* Display the FilePilot application shell.
* Use the central theme.
* Have centralized routing.
* Have dependency injection.
* Have error handling foundations.
* Have logging foundations.
* Pass static analysis.

---

# 9. M1 Project Structure

The initial structure will be approximately:

```text
lib/
├── app/
│   ├── app.dart
│   ├── bootstrap.dart
│   ├── router/
│   ├── theme/
│   └── localization/
│
├── core/
│   ├── constants/
│   ├── errors/
│   ├── logging/
│   ├── result/
│   ├── types/
│   ├── utils/
│   └── extensions/
│
├── domain/
│   ├── entities/
│   ├── repositories/
│   └── services/
│
├── data/
│   ├── models/
│   ├── repositories/
│   ├── datasources/
│   └── persistence/
│
├── infrastructure/
│   ├── android/
│   ├── filesystem/
│   ├── pdf/
│   ├── ocr/
│   ├── crypto/
│   └── storage/
│
├── features/
│   ├── explorer/
│   ├── search/
│   ├── bookmarks/
│   ├── recent/
│   ├── storage_analyzer/
│   ├── preview/
│   ├── pdf_workspace/
│   ├── screenshot_ocr/
│   ├── security/
│   └── settings/
│
└── main.dart
```

Not every directory will necessarily contain code immediately.

Empty architectural folders will not be created without a reason.

---

# 10. M1 Application Shell

The initial application shell will contain:

* FilePilot branding
* Application theme
* Navigation shell
* Home/dashboard placeholder
* Settings placeholder
* Error boundary
* Loading state foundation

The first application is intentionally simple.

The goal is to establish the framework, not to pretend the product is already finished.

---

# 11. M1 Theme System

The theme system will provide:

* Light theme
* Dark theme
* System theme
* Typography
* Spacing
* Shapes
* Component styling

Theme values MUST be centralized.

Feature widgets MUST NOT invent unrelated styling.

---

# 12. M1 Testing

Initial tests will verify:

* Application bootstrap
* Theme creation
* Routing
* Basic application launch
* Core utilities where applicable

---

# 13. Milestone M2 — Core Infrastructure

## Objective

Build the foundations required by every future feature.

Components:

```text
Error system
Logging
Result types
Configuration
Dependency injection
Persistence abstraction
Job abstraction
File metadata models
```

---

# 14. M2 Error System

Create structured application errors.

Initial categories:

```text
FileAccessError
PermissionError
StorageError
OperationCancelledError
ProcessingError
SecurityError
UnknownError
```

Errors will have:

* Internal classification
* User-facing explanation
* Optional technical information
* Optional recovery action

---

# 15. M2 Result System

Create a consistent result pattern for operations that may fail.

Conceptually:

```text
Result<T>
├── Success<T>
└── Failure
```

This system will prevent every feature from inventing its own error-handling model.

---

# 16. M2 Logging

Create structured logging infrastructure.

Logging must support:

* Debug
* Info
* Warning
* Error
* Critical

Sensitive information MUST be excluded.

---

# 17. M2 Job Framework

Create the generic long-running operation model.

Initial states:

```text
Queued
Running
Cancelling
Completed
Failed
Cancelled
```

The framework will later support:

* File operations
* OCR
* PDF processing
* Encryption
* Storage scanning

---

# 18. Milestone M3 — File Engine

## Objective

Build the core capability upon which FilePilot depends.

The File Engine will provide:

* Directory listing
* File metadata
* Folder creation
* Copy
* Move
* Rename
* Delete
* Existence checks
* Open/share support
* Batch operations

---

# 19. M3 File Abstractions

Initial domain concepts:

```text
FileItem
Folder
StorageLocation
FileMetadata
FileOperation
```

Repositories/interfaces will be created before concrete filesystem implementations.

---

# 20. M3 Android Filesystem Integration

The Android implementation will be isolated behind infrastructure adapters.

The implementation MUST respect modern Android storage rules.

We will not assume unrestricted filesystem access.

Where Android APIs require user-granted access, FilePilot will guide the user through the correct Android workflow.

---

# 21. M3 File Operation Safety

Copy and move operations must account for:

* Existing destination
* Name collisions
* Insufficient storage
* Permission failures
* Interrupted operations
* Partial operations

Delete operations must account for:

* Confirmation
* Multiple selections
* Failure reporting
* Undo/recovery where technically feasible

---

# 22. M3 Testing

Tests will cover:

* Listing
* Metadata
* Create folder
* Copy
* Move
* Rename
* Delete
* Collision handling
* Permission failures
* Batch operations

Tests should use temporary/test storage rather than real user data.

---

# 23. Milestone M4 — File Explorer

## Objective

Build the first genuinely useful FilePilot interface.

The Explorer will provide:

* Folder navigation
* File list
* Grid view
* Sorting
* Selection
* Multi-selection
* File actions
* Folder actions
* Metadata display

---

# 24. M4 Explorer UX

The explorer should provide:

* Current location
* Navigation
* Breadcrumbs or equivalent navigation aid
* Contextual actions
* Selection toolbar
* Empty states
* Loading states
* Error states

The explorer must remain usable on small screens.

---

# 25. M4 Explorer Testing

Test:

* Folder navigation
* Selection
* Multi-selection
* Sorting
* File actions
* Empty directories
* Permission errors
* Large directories

---

# 26. Milestone M5 — Search Engine

## Objective

Build fast local file search.

Architecture:

```text
Filesystem
    ↓
Scanner
    ↓
Metadata Extractor
    ↓
Local Index
    ↓
Search Engine
    ↓
Explorer / Results
```

---

# 27. M5 Search Capabilities

Initial search:

* File name
* Folder name
* Extension
* File type
* Location

Filtering and sorting will be supported.

---

# 28. M5 Indexing

The index should support incremental updates.

The application should avoid rebuilding the entire index unnecessarily.

The index must remain local.

---

# 29. M5 Search Testing

Test:

* Exact matches
* Partial matches
* Case handling
* Extensions
* Filters
* Large indexes
* Missing/inaccessible files
* Index updates

---

# 30. Milestone M6 — Bookmarks and Recent Files

## Objective

Add persistent organization features.

Components:

```text
BookmarkRepository
RecentRepository
```

Capabilities:

* Add bookmark
* Remove bookmark
* View bookmarks
* Open bookmark
* Record recent item
* Clear recent items

All data remains local.

---

# 31. Milestone M7 — Storage Analyzer

## Objective

Give users a clear understanding of storage usage.

Capabilities:

* Total storage
* Used storage
* Available storage
* Category breakdown
* Largest files
* Large folders
* File counts

The analyzer will reuse scanning infrastructure where possible.

---

# 32. M7 Storage Visualization

The UI SHOULD include:

* Summary
* Category visualization
* Largest files
* Largest folders
* Drill-down

Destructive cleanup will NOT be automated.

---

# 33. Milestone M8 — Preview and Thumbnail Engine

## Objective

Create a unified preview system.

Supported initial categories:

* Images
* PDFs
* Text
* Video/audio where practical

The system will determine whether FilePilot can:

1. Preview internally.
2. Generate a thumbnail.
3. Open externally.

---

# 34. M8 Thumbnail Caching

Thumbnail generation will be asynchronous.

Thumbnails will be cached using stable file identity information.

Cache invalidation will account for changed files.

---

# 35. Milestone M9 — PDF Engine

## Objective

Introduce the PDF workspace.

Initial capabilities:

* Open
* Preview
* Create
* Merge
* Split
* Reorder
* Extract
* Delete pages
* Rotate
* Rename
* Share

---

# 36. M9 PDF Architecture

The application will communicate with:

```text
PdfRepository
      ↓
PdfService
      ↓
PdfAdapter
      ↓
Selected PDF Technology
```

The UI MUST NOT directly depend on the selected PDF library.

---

# 37. M9 PDF Testing

Test:

* PDF loading
* PDF creation
* Merge
* Split
* Page extraction
* Page deletion
* Rotation
* Reordering
* Large PDFs
* Corrupt PDFs
* Failure recovery

---

# 38. Milestone M10 — Screenshot OCR

## Objective

Integrate the proven Screenshot OCR concept into FilePilot as a self-contained module.

This is a major differentiating capability.

The module will provide:

```text
Screenshot Selection
        ↓
Image Preparation
        ↓
OCR
        ↓
Result Review
        ↓
Text / PDF Output
```

---

# 39. M10 OCR Requirements

The implementation should support:

* Selecting multiple screenshots
* Batch OCR
* Progress reporting
* OCR result review
* Text copying
* Text sharing
* PDF generation
* Screenshot ordering
* Limited editing

---

# 40. M10 OCR Independence

FilePilot OCR MUST NOT depend on the existing Screenshot OCR application.

The existing application remains:

* Standalone
* Independently deployable
* Unmodified unless separately decided

FilePilot receives its own implementation.

---

# 41. M10 OCR Migration Strategy

Before implementation, we will inspect the existing Screenshot OCR project and identify:

* Proven algorithms
* Existing workflows
* Useful Dart code
* Android integration
* OCR implementation
* PDF generation
* Image preprocessing
* UI lessons
* Known limitations

Only suitable components will be adapted.

We will NOT blindly copy the existing application into FilePilot.

---

# 42. M10 OCR Testing

Testing will include:

* Single screenshot
* Multiple screenshots
* Poor-quality images
* Different text layouts
* Large batches
* Empty OCR results
* OCR failure
* PDF creation
* Text export
* Sharing

---

# 43. Milestone M11 — Security

## Objective

Implement security capabilities only after the security architecture has been reviewed.

Initial scope:

* Secure key storage
* File encryption
* File decryption
* Protected workflows

Vault functionality may follow as a separate milestone.

---

# 44. M11 Security Rules

We MUST:

* Use established cryptographic primitives.
* Use secure platform storage where appropriate.
* Avoid plaintext key storage.
* Avoid custom cryptographic algorithms.
* Clearly communicate recovery limitations.
* Test interrupted operations.

---

# 45. M11 Security Testing

Security tests must include:

* Encryption/decryption round trips
* Wrong credentials
* Corrupted encrypted files
* Interrupted encryption
* Interrupted decryption
* Key-storage failures
* Permission failures

---

# 46. Milestone M12 — Premium Architecture

## Objective

Introduce commercial functionality without contaminating the core architecture.

Premium capabilities should be represented as capabilities layered over shared engines.

Examples:

```text
Core File Engine
       │
       ├── Free Explorer
       │
       └── Premium Batch Operations
```

---

# 47. Premium Entitlement Architecture

Premium entitlement logic should be centralized.

Feature code should ask:

```text
CanUsePremiumFeature?
```

rather than implementing its own purchase checks.

This makes monetization replaceable.

---

# 48. Monetization Decision

The exact commercial model will be selected after evaluating:

* Google Play requirements
* User experience
* Competitive positioning
* Feature value
* Development cost
* Support burden

Possible models remain:

* One-time purchase
* Subscription
* Free + premium upgrade

No monetization implementation will be created before the decision is documented.

---

# 49. Milestone M13 — Settings and Preferences

Settings will provide:

* Theme
* Explorer defaults
* Sorting
* Display mode
* Search preferences
* Preview preferences
* OCR preferences
* Security preferences
* Privacy preferences

Settings must be persisted locally.

---

# 50. Milestone M14 — Accessibility and UX Hardening

The application will undergo a dedicated usability pass.

Review:

* Touch targets
* Screen-reader labels
* Contrast
* Typography
* Loading states
* Empty states
* Error states
* Confirmation dialogs
* Navigation
* Dark mode
* Small-screen layouts

---

# 51. Milestone M15 — Performance Hardening

Performance testing will focus on:

* Thousands of files
* Large folders
* Large search indexes
* Large PDFs
* Large OCR batches
* Storage scanning
* Thumbnail generation
* Encryption
* Batch operations

Potential optimizations include:

* Incremental indexing
* Caching
* Background processing
* Pagination
* Lazy loading
* Parallel processing where safe

Optimization MUST be evidence-based.

---

# 52. Milestone M16 — Reliability and Recovery

The application will be tested against:

* Interrupted operations
* Low storage
* Permission changes
* Missing files
* Renamed files
* External storage removal
* Application restart
* Device restart
* Corrupt documents
* Failed background jobs

---

# 53. Milestone M17 — Security and Privacy Audit

Before release:

* Review permissions.
* Review data handling.
* Review logging.
* Review dependencies.
* Review encryption.
* Review sharing.
* Review network usage.
* Review privacy documentation.

---

# 54. Milestone M18 — Release Preparation

Release preparation includes:

* Versioning
* App icon
* Splash/launch experience
* Store screenshots
* Store description
* Privacy policy
* Data Safety information
* Permission declarations
* Signing
* Release build
* ProGuard/R8 configuration where applicable
* Crash monitoring strategy
* Release checklist

---

# 55. Implementation Order

The mandatory initial implementation order is:

```text
M0 Documentation
        ↓
M1 Flutter Bootstrap
        ↓
M2 Core Infrastructure
        ↓
M3 File Engine
        ↓
M4 File Explorer
        ↓
M5 Search
        ↓
M6 Bookmarks / Recent
        ↓
M7 Storage Analyzer
        ↓
M8 Preview / Thumbnails
        ↓
M9 PDF Engine
        ↓
M10 Screenshot OCR
        ↓
M11 Security
        ↓
M12 Premium Architecture
        ↓
M13 Settings
        ↓
M14 UX / Accessibility
        ↓
M15 Performance
        ↓
M16 Reliability
        ↓
M17 Security / Privacy Audit
        ↓
M18 Google Play Release
```

Milestones may be subdivided into smaller implementation tasks.

---

# 56. File Creation Strategy

We will create files progressively.

We will NOT create hundreds of empty files in advance.

A file should be created when:

* Its responsibility is understood.
* Its owner is known.
* Its implementation is needed.

This keeps the repository meaningful.

---

# 57. Implementation Batch Size

Each implementation batch should be small enough to:

* Review
* Analyze
* Test
* Commit

A typical batch may contain:

* 1–10 production files
* Associated tests
* Required documentation changes

Large batches will be used only where the files are tightly coupled and cannot reasonably be separated.

---

# 58. Generated Code Policy

AI-generated code MUST be treated as production code.

It is subject to the same requirements as manually written code:

* Review
* Testing
* Static analysis
* Security review
* Documentation

No generated code is considered correct merely because it compiles.

---

# 59. Git Commit Strategy

Commits should remain focused.

Examples:

```text
feat: bootstrap FilePilot application
feat: add application theme
feat: add file repository abstraction
feat: implement local folder listing
test: add file operation tests
feat: add explorer screen
fix: handle inaccessible directories
```

---

# 60. Branch Strategy

For early development, the main branch may remain the integration branch.

As the project becomes larger, feature branches MAY be introduced.

We will avoid unnecessary Git complexity while the project is still small.

---

# 61. Static Analysis

Every implementation milestone must pass:

```text
flutter analyze
```

The project should maintain a strict analysis configuration.

Warnings MUST have a reason if they are intentionally suppressed.

---

# 62. Formatting

Dart formatting will be enforced.

The project will use:

```text
dart format
```

Code formatting MUST NOT be manually improvised.

---

# 63. Testing Gate

Before a milestone is committed:

```text
flutter test
```

must pass for all applicable tests.

Integration testing will be introduced as soon as the corresponding functionality exists.

---

# 64. Build Gate

The project must periodically verify:

```text
flutter build apk
```

and eventually:

```text
flutter build appbundle
```

The exact release configuration will be finalized before Google Play submission.

---

# 65. Documentation Gate

If implementation changes architecture:

* Update `ARCHITECTURE.md`.
* Add/update an ADR where appropriate.

If implementation changes requirements:

* Update `PRODUCT_REQUIREMENTS.md`.

Documentation and implementation must remain synchronized.

---

# 66. Technical Decision Gates

Before committing to major technologies, we will stop and evaluate.

Decision gates include:

### Gate 1

State management

### Gate 2

Routing

### Gate 3

Persistence/database

### Gate 4

Filesystem access

### Gate 5

PDF engine

### Gate 6

OCR engine

### Gate 7

Encryption

### Gate 8

Background/job execution

### Gate 9

Premium billing

Each significant decision will be documented.

---

# 67. External Project Knowledge

The existing projects available to the FilePilot development process may provide useful lessons.

Particularly important:

* Screenshot OCR
* WiFi Survey Suite
* Bookmark App

However, FilePilot MUST NOT become architecturally dependent on those applications.

Knowledge may be reused.

Code may be adapted only when technically and legally appropriate.

---

# 68. Screenshot OCR Integration Rule

The Screenshot OCR project is already a separate application.

Therefore:

> **FilePilot will absorb the capability, not the application.**

We will inspect the existing implementation, identify reusable components, and rebuild/adapt the functionality into FilePilot's architecture.

The existing Screenshot OCR application remains untouched.

---

# 69. Five-Year Architecture Test

Before introducing a major dependency or architectural pattern, ask:

> Would we still be comfortable maintaining this decision five years from now?

If the answer is uncertain, evaluate alternatives before committing.

---

# 70. Avoiding Circular Development

If implementation discussion begins repeatedly revisiting a settled decision without new evidence, the issue should be identified as an architecture review.

The agreed project signal is:

> **Architect Check**

When this phrase is used, implementation pauses and the relevant decision is reassessed.

---

# 71. External Review Policy

External technical opinions are welcome.

If the developer receives advice from:

* Another AI
* A developer
* Documentation
* A forum
* A package maintainer
* Google/Android documentation

the information may be brought into the project for evaluation.

The FilePilot architecture remains the decision framework.

External advice is evaluated rather than automatically accepted or rejected.

---

# 72. Implementation Responsibility

The implementation responsibility is divided as follows:

### Architecture and Code

The assistant designs and generates the implementation.

### Local Execution

The developer executes commands, commits, pushes, builds, and runs the application locally.

### Testing Feedback

The developer reports actual runtime/build/test results.

### Technical Decisions

The assistant proposes decisions and explains trade-offs.

The developer retains final approval over project direction.

---

# 73. First Production-Code Milestone

The first production-code milestone is:

# M1 — Flutter Bootstrap

The objective is NOT to build the file manager.

The objective is to prove that:

```text
Flutter
  ↓
FilePilot Application
  ↓
Theme
  ↓
Navigation
  ↓
Dependency Injection
  ↓
Error Handling
  ↓
Logging
  ↓
Testing
```

all work together correctly.

---

# 74. M1 Acceptance Criteria

M1 is complete only when:

* Flutter project builds.
* Android application launches.
* FilePilot branding is visible.
* Theme works.
* Light/dark/system themes function.
* Navigation works.
* Dependency injection is operational.
* Error handling foundation exists.
* Logging foundation exists.
* Tests pass.
* `flutter analyze` passes.
* APK builds successfully.

---

# 75. M1 Expected Repository Structure

After M1, the repository should approximately resemble:

```text
Filemanager_app/
│
├── android/
├── lib/
├── test/
├── docs/
│
├── pubspec.yaml
├── analysis_options.yaml
├── README.md
└── ...
```

The exact Flutter-generated files will depend on the Flutter SDK version used.

---

# 76. M1 Completion

At the end of M1, FilePilot should be a small but genuine application.

It should not yet pretend to be a finished file manager.

It should instead be a **stable foundation upon which the file manager can safely be built.**

---

# 77. First Implementation Principle

We will build from the bottom upward:

```text
Infrastructure
      ↓
Domain
      ↓
Application
      ↓
Presentation
```

But we will validate the system from the user's perspective:

```text
User
 ↓
UI
 ↓
Use Case
 ↓
Repository
 ↓
Infrastructure
 ↓
Android
```

Both perspectives are required.

---

# 78. Quality Standard

FilePilot is intended to become a commercial product.

Therefore:

> **Prototype code is not the default.**

When a temporary implementation is unavoidable, it MUST be clearly identified and replaced before the corresponding milestone is considered complete.

---

# 79. Final Implementation Principle

The goal is not to produce the largest amount of code.

The goal is to produce the smallest amount of **well-structured, tested, maintainable code** that satisfies the requirements.

---

# 80. Implementation North Star

The final architecture should allow us to add:

```text
New Feature
    ↓
New Module
    ↓
Existing Engines
    ↓
Existing Infrastructure
```

without rewriting the application.

That is the measure by which the implementation architecture will ultimately be judged.

---

**End of Technical Implementation Plan**
