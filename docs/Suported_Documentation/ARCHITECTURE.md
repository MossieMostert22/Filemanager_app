# FilePilot — Architecture Blueprint

**Document:** `docs/ARCHITECTURE.md`
**Product:** FilePilot
**Version:** 0.1
**Status:** Draft for Review
**Platform:** Android
**Framework:** Flutter / Dart

---

# 1. Architecture Purpose

This document defines the technical architecture of FilePilot.

The Product Requirements Document defines **what FilePilot must do**.

This document defines **how FilePilot will be structured and engineered to do it**.

The architecture MUST prioritize:

* Maintainability
* Reliability
* Testability
* Performance
* Security
* Modularity
* Offline operation
* Long-term extensibility

The architecture MUST allow FilePilot to grow substantially without requiring a complete rewrite.

---

# 2. Architectural Principles

FilePilot follows these principles.

## 2.1 Offline First

Core functionality MUST NOT depend on an internet connection.

Network-dependent capabilities, if introduced later, MUST remain isolated from the offline core.

---

## 2.2 Local Data First

Files and user-generated data SHOULD remain on the user's device unless the user explicitly chooses otherwise.

Application services MUST NOT assume cloud availability.

---

## 2.3 Feature Isolation

Major capabilities MUST be isolated into feature modules.

A feature should be able to evolve without unnecessarily modifying unrelated features.

---

## 2.4 Dependency Direction

Dependencies MUST flow toward stable abstractions.

UI code MUST NOT directly contain filesystem, database, OCR, PDF, encryption, or platform-specific business logic.

---

## 2.5 Platform Isolation

Android-specific functionality MUST be isolated behind interfaces or adapters wherever practical.

Flutter/Dart business logic should not become tightly coupled to Android implementation details.

---

## 2.6 Testability

Business logic MUST be testable without requiring a physical Android device whenever practical.

External dependencies SHOULD be replaceable with test doubles.

---

## 2.7 Explicit Boundaries

Each major subsystem MUST have a clearly defined responsibility.

The following rule applies:

> If two components have different responsibilities, they should not secretly share implementation details.

---

# 3. High-Level Architecture

FilePilot will use a modular layered architecture.

```text
┌───────────────────────────────────────────────┐
│                 Presentation                  │
│                                               │
│ Screens • Widgets • Controllers • Navigation │
└───────────────────────┬───────────────────────┘
                        │
                        ▼
┌───────────────────────────────────────────────┐
│                Application                    │
│                                               │
│ Use Cases • Coordinators • Application State │
└───────────────────────┬───────────────────────┘
                        │
                        ▼
┌───────────────────────────────────────────────┐
│                   Domain                      │
│                                               │
│ Entities • Contracts • Business Rules        │
└───────────────────────┬───────────────────────┘
                        │
                        ▼
┌───────────────────────────────────────────────┐
│                    Data                       │
│                                               │
│ Repositories • Data Sources • Persistence    │
└───────────────────────┬───────────────────────┘
                        │
                        ▼
┌───────────────────────────────────────────────┐
│             Infrastructure / Platform        │
│                                               │
│ Android • Filesystem • SQLite • OS APIs      │
│ PDF • OCR • Crypto • Media • Storage         │
└───────────────────────────────────────────────┘
```

The exact implementation may evolve, but these responsibilities MUST remain identifiable.

---

# 4. Application Layers

## 4.1 Presentation Layer

The presentation layer is responsible for:

* Screens
* Widgets
* User interaction
* Visual state
* Navigation
* Accessibility
* User-facing error presentation

The presentation layer MUST NOT perform raw filesystem operations.

For example, this is prohibited:

```dart
File('/storage/emulated/0/file.pdf').delete();
```

inside a UI widget.

Instead, the UI invokes an appropriate application operation.

---

# 5. Application Layer

The application layer coordinates user actions.

Examples include:

```text
CopyFile
MoveFile
DeleteFile
SearchFiles
AnalyzeStorage
CreatePdf
MergePdf
RunOcr
EncryptFile
AddBookmark
```

Application services/use cases SHOULD represent meaningful user operations.

The application layer coordinates domain rules and repositories.

---

# 6. Domain Layer

The domain layer contains business concepts that should remain independent of Flutter and Android wherever practical.

Examples:

```text
FileItem
Folder
StorageLocation
Bookmark
RecentItem
SearchQuery
PdfDocument
PdfPage
OcrDocument
OcrResult
EncryptionJob
StorageSummary
```

The domain layer SHOULD NOT depend on:

* Flutter widgets
* Android APIs
* UI packages
* filesystem implementation details
* database implementation details

---

# 7. Data Layer

The data layer implements domain/application contracts.

Responsibilities include:

* Repository implementations
* Local persistence
* File metadata storage
* Search indexes
* Bookmarks
* Recent items
* Preferences
* Cached information

Repositories provide stable interfaces to the application layer.

---

# 8. Infrastructure Layer

Infrastructure contains implementations that communicate directly with external systems.

Examples include:

* Android filesystem APIs
* Storage Access Framework
* SQLite
* Platform channels
* Native libraries
* PDF libraries
* OCR engines
* Cryptographic APIs
* Media APIs

Infrastructure MUST NOT leak unnecessary implementation details into the presentation layer.

---

# 9. Proposed Project Structure

The initial Flutter project should follow a feature-oriented structure with shared architectural layers.

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
│   ├── utils/
│   ├── extensions/
│   └── types/
│
├── domain/
│   ├── entities/
│   ├── repositories/
│   └── services/
│
├── data/
│   ├── repositories/
│   ├── datasources/
│   ├── models/
│   └── persistence/
│
├── infrastructure/
│   ├── filesystem/
│   ├── android/
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

This structure is intentionally modular.

The structure MAY evolve as implementation reveals better boundaries, but changes MUST be deliberate.

---

# 10. Feature Structure

Each substantial feature SHOULD follow a consistent internal structure.

Example:

```text
features/explorer/
├── presentation/
│   ├── screens/
│   ├── widgets/
│   └── controllers/
│
├── application/
│   └── use_cases/
│
├── domain/
│   ├── entities/
│   └── repositories/
│
└── data/
    ├── repositories/
    └── datasources/
```

Small features may use a simplified structure where appropriate.

Architecture should not become ceremony for its own sake.

---

# 11. File Engine

The File Engine is one of the most important components in FilePilot.

It provides a consistent abstraction over file operations.

Responsibilities include:

* Listing files
* Listing folders
* Reading metadata
* Creating folders
* Copying
* Moving
* Renaming
* Deleting
* Opening
* Sharing
* Batch operations
* File existence checks

The File Engine MUST NOT be implemented directly inside UI components.

---

# 12. File Repository

The application layer should communicate with files through a repository abstraction.

Conceptually:

```text
UI
 ↓
Use Case
 ↓
File Repository
 ↓
Filesystem Adapter
 ↓
Android / Storage API
```

This allows the underlying filesystem implementation to change without rewriting the UI.

---

# 13. Android Storage Architecture

FilePilot MUST respect modern Android storage restrictions.

The architecture MUST NOT assume unrestricted access to the entire filesystem.

Android-specific storage access should be isolated behind an abstraction.

Potential implementation mechanisms include:

* Android Storage Access Framework
* MediaStore
* App-specific storage
* Platform APIs
* Appropriate permission mechanisms

The final implementation will be selected based on current Android requirements at implementation time.

---

# 14. File Identity

FilePilot MUST NOT rely solely on filenames to identify files.

Where practical, file identity should consider:

* URI/path
* Provider identity
* Size
* Modification timestamp
* Stable identifiers where available

The architecture MUST accommodate storage providers where traditional filesystem paths are unavailable.

---

# 15. Batch Operations

Batch operations are a core capability.

The architecture should support:

```text
Batch Copy
Batch Move
Batch Delete
Batch Rename
Batch Share
Batch Encrypt
Batch PDF processing
Batch OCR
```

Long-running batch operations SHOULD be represented as jobs.

---

# 16. Job Architecture

FilePilot should use a common job model for long-running operations.

Conceptually:

```text
Job
├── queued
├── running
├── paused
├── cancelling
├── completed
└── failed
```

A job SHOULD expose:

* Identifier
* Type
* Progress
* Status
* Current item
* Total items
* Error information
* Cancellation capability

This allows the UI to display consistent progress information across different engines.

---

# 17. Search Architecture

Search should not depend on repeatedly scanning the entire filesystem for every query.

FilePilot SHOULD maintain a local searchable metadata index.

The architecture should support:

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
Search Results
```

The search index MUST remain local.

Indexing should be incremental wherever possible.

---

# 18. Storage Analyzer Architecture

The storage analyzer should build upon the same filesystem/indexing infrastructure where practical.

It should calculate:

* Total space
* Used space
* Available space
* File counts
* Folder sizes
* Category sizes
* Largest files

Scanning operations SHOULD avoid unnecessarily duplicating filesystem work already performed by other subsystems.

---

# 19. PDF Engine

PDF processing will be isolated behind a PDF service abstraction.

Potential operations include:

```text
Open
Create
Merge
Split
Reorder
Extract
Delete pages
Rotate
Export
```

The application layer should not depend directly on the selected PDF library.

This gives FilePilot the ability to replace the underlying PDF technology if licensing, performance, or compatibility requirements change.

---

# 20. Screenshot OCR Engine

The Screenshot OCR functionality will exist as an independent FilePilot module.

Conceptually:

```text
Screenshot Selection
        ↓
Image Preparation
        ↓
OCR Engine
        ↓
OCR Result
        ↓
Editing / Review
        ↓
Text Export / PDF Creation
```

The OCR engine MUST be isolated from the rest of FilePilot.

This is important because the existing Screenshot OCR application is already a mature product and its proven functionality will inform the FilePilot implementation.

However:

> FilePilot MUST NOT depend on the existing Screenshot OCR application at runtime.

FilePilot will contain its own implementation.

---

# 21. OCR Module Boundary

The OCR module SHOULD expose a stable interface similar to:

```text
OcrEngine
 ├── recognizeImage()
 ├── recognizeImages()
 └── recognizeDocument()
```

The actual OCR implementation remains an infrastructure concern.

This allows future OCR engines to be evaluated without rewriting the OCR workspace.

---

# 22. OCR and PDF Integration

OCR and PDF should cooperate through application-level workflows rather than direct coupling.

Example:

```text
Screenshot OCR
      ↓
OcrResult
      ↓
Document Builder
      ↓
Pdf Engine
      ↓
PDF File
```

This keeps the OCR engine independent of the PDF implementation.

---

# 23. Security Architecture

Security-sensitive functionality MUST have strict boundaries.

Security functionality includes:

* Encryption
* Secure storage
* Key management
* Authentication
* Vault operations

Cryptographic operations MUST use established cryptographic libraries or Android security mechanisms.

FilePilot MUST NOT invent cryptographic algorithms.

---

# 24. Key Management

Encryption keys MUST NOT be stored as ordinary plaintext application preferences.

Where appropriate, platform-backed secure storage SHOULD be used.

The final key-management design MUST be documented before encryption functionality is released.

---

# 25. Secure Vault Architecture

If the secure vault is implemented, it should be treated as a separate subsystem.

The vault MUST NOT simply rename or hide files.

The architecture should define:

```text
Vault
├── Authentication
├── Key Management
├── Secure Storage
├── Encryption
├── Decryption
└── Recovery / Failure Handling
```

Security review is required before release.

---

# 26. State Management

Application state should be separated into:

### UI State

Examples:

* Selected files
* Current folder
* Current sort order
* Current display mode
* Dialog visibility

### Application State

Examples:

* Active jobs
* Search state
* Current operations
* OCR processing state

### Persistent State

Examples:

* Bookmarks
* Settings
* Recent files
* User preferences

State management technology will be selected during implementation based on:

* Reliability
* Testability
* Maintainability
* Flutter compatibility
* Project complexity

The architecture MUST avoid unnecessary global mutable state.

---

# 27. Dependency Injection

Dependencies SHOULD be injected rather than constructed throughout the UI.

Conceptually:

```text
Application
    ↓
Dependency Container
    ↓
Repositories / Services / Engines
    ↓
Infrastructure
```

This improves:

* Testing
* Replacement of implementations
* Configuration
* Maintainability

---

# 28. Navigation

Navigation should be centralized.

Feature screens MUST NOT independently construct unrelated navigation systems.

The routing architecture should support:

* Home
* Explorer
* Search
* Bookmarks
* Recent
* Storage Analyzer
* PDF Workspace
* Screenshot OCR
* Security
* Settings

The navigation solution will be selected during implementation.

---

# 29. Error Architecture

Errors should be represented explicitly.

The architecture should distinguish between:

```text
Expected User Error
Operational Error
Permission Error
Filesystem Error
Security Error
Processing Error
Programming Error
```

Infrastructure exceptions SHOULD be translated into application-level errors.

The UI then decides how to communicate the error to the user.

---

# 30. Result-Based Operations

Operations that can legitimately fail SHOULD return structured results rather than relying exclusively on thrown exceptions.

Conceptually:

```text
Success
Failure
```

Failure information SHOULD contain:

* Error category
* User-facing message
* Technical details for logging
* Optional recovery action

---

# 31. Logging

FilePilot SHOULD provide structured application logging.

Logging levels should include:

```text
Debug
Info
Warning
Error
Critical
```

Logs MUST NOT contain sensitive user data unnecessarily.

Examples of data that should not be logged casually:

* File contents
* Encryption keys
* Passwords
* Authentication credentials
* OCR text
* Personal information

---

# 32. Performance Architecture

Performance-sensitive operations MUST be isolated from the UI thread.

Examples:

* Filesystem scanning
* Search indexing
* Large PDF processing
* OCR
* Encryption
* Large batch operations
* Storage analysis

The architecture SHOULD allow long-running work to continue while the UI remains responsive.

---

# 33. Caching

Caching SHOULD be used selectively.

Good candidates include:

* File metadata
* Folder information
* Search indexes
* Thumbnail information
* Storage calculations

Caching MUST NOT create unacceptable risks of displaying stale or incorrect information.

The architecture should define cache invalidation explicitly.

---

# 34. Thumbnail Architecture

Thumbnail generation should be centralized.

The thumbnail subsystem SHOULD support:

* Images
* Video
* PDF
* Other supported preview types

Thumbnails SHOULD be cached.

Thumbnail generation SHOULD be performed asynchronously.

---

# 35. Sharing Architecture

FilePilot should use Android's standard sharing mechanisms.

The application should expose files through appropriate content URIs rather than relying on unsafe direct filesystem paths.

Sharing functionality should remain isolated from individual feature implementations.

---

# 36. Preview Architecture

Previewing should use a common preview abstraction.

Conceptually:

```text
PreviewService
├── ImagePreview
├── PdfPreview
├── TextPreview
├── VideoPreview
└── ExternalOpen
```

A feature should ask the preview service to display a file rather than determining implementation details itself.

---

# 37. Persistence

Local application metadata SHOULD use a reliable local persistence mechanism.

Potential data includes:

* Bookmarks
* Recent files
* Search index
* User preferences
* Application state
* Job history where required

The persistence implementation should be replaceable.

---

# 38. Database Strategy

A local database MAY be used for structured metadata and indexing.

The database should NOT be used as the actual storage location for user files.

User files remain in their appropriate Android storage locations.

The database stores metadata about those files where useful.

---

# 39. Settings Architecture

Settings should be centralized.

Potential settings include:

* Theme
* Default view
* Sort order
* Search preferences
* Thumbnail preferences
* Security preferences
* Privacy preferences
* OCR preferences
* PDF preferences

Feature-specific settings SHOULD remain owned by their respective features while being exposed through a consistent settings experience.

---

# 40. Localization

The application should be designed for localization from the beginning.

User-visible strings MUST NOT be scattered as hard-coded text throughout widgets.

English will be the initial language.

Additional languages can be added later.

---

# 41. Accessibility Architecture

Accessibility must be part of component design.

Reusable components SHOULD provide:

* Semantic labels
* Meaningful roles
* Accessible actions
* Adequate touch targets
* Screen-reader compatibility

Accessibility requirements apply to both core file management and advanced tools.

---

# 42. Theme Architecture

The theme system should be centralized.

It MUST support:

```text
Light
Dark
System
```

Colors, typography, spacing, shapes, and component styles should be defined centrally rather than duplicated throughout feature widgets.

---

# 43. Dependency Policy

Every external package MUST have a reason to exist.

Before introducing a dependency, we should evaluate:

* Maintenance status
* License
* Security history
* Android compatibility
* Flutter compatibility
* Community adoption
* Performance
* API stability
* Whether the functionality can reasonably be implemented internally

A package MUST NOT be added merely because it saves a few lines of code.

---

# 44. Native Android Integration

Native Android code MAY be used when Flutter alone cannot provide the required capability adequately.

Native integrations MUST be isolated.

The preferred pattern is:

```text
Flutter
   ↓
Platform Interface
   ↓
Android Adapter
   ↓
Android API
```

Platform-specific implementation MUST NOT spread throughout the application.

---

# 45. Package Licensing

Every dependency used by FilePilot MUST have licensing compatible with commercial Google Play distribution.

License information SHOULD be documented.

Dependencies with unclear or unsuitable licensing MUST NOT be introduced.

---

# 46. Testing Architecture

Testing should mirror architectural boundaries.

## Unit Tests

Test:

* Domain logic
* Use cases
* Repositories
* Search
* Storage calculations
* File operation logic
* Encryption workflows
* OCR workflows

## Widget Tests

Test:

* Reusable widgets
* Screens
* Dialogs
* Forms
* Error states

## Integration Tests

Test critical workflows end-to-end.

Examples:

```text
Browse → Open File
Select → Copy → Verify
Search → Open Result
Select Screenshots → OCR → PDF
Encrypt → Decrypt → Verify
```

---

# 47. Testability Requirement

A component that cannot reasonably be tested because it is tightly coupled to Android, Flutter widgets, or a concrete third-party package SHOULD be redesigned behind an abstraction where practical.

---

# 48. Build Configuration

Environment-specific configuration MUST NOT be hard-coded throughout the application.

Development, testing, and production configurations SHOULD be clearly separated.

Secrets MUST NOT be committed to Git.

---

# 49. Secrets

The repository MUST NOT contain:

* API keys
* Signing passwords
* Encryption keys
* Authentication credentials
* Private certificates
* Personal access tokens

Secret management MUST be handled outside ordinary source files.

---

# 50. Git Workflow

The repository should follow small, meaningful commits.

Examples:

```text
docs: add architecture blueprint
feat: add file repository abstraction
feat: implement folder browsing
test: add file repository tests
fix: handle inaccessible storage locations
```

Commits SHOULD represent one logical change.

Large unrelated changes MUST NOT be mixed into a single commit.

---

# 51. Implementation Workflow

Development should follow this sequence:

```text
Requirement
    ↓
Design
    ↓
Architecture
    ↓
Implementation Plan
    ↓
Implementation
    ↓
Tests
    ↓
Manual Verification
    ↓
Documentation
    ↓
Commit
```

Implementation should not begin merely because an idea sounds useful.

---

# 52. Feature Development Rule

Before implementing a substantial feature, we MUST know:

1. What requirement it satisfies.
2. Which architectural layer owns it.
3. Which feature/module owns it.
4. Which dependencies it requires.
5. How it will be tested.
6. What failure conditions exist.

---

# 53. Definition of Done

A feature is complete only when:

* Requirements are satisfied.
* Architecture rules are followed.
* Code compiles.
* Static analysis passes.
* Appropriate tests pass.
* Error handling exists.
* Accessibility has been considered.
* Documentation is updated where necessary.
* No known critical defects remain.

---

# 54. Architecture Decision Records

Significant technical decisions MUST be documented as Architecture Decision Records.

Examples:

```text
docs/decisions/
├── ADR-0001-state-management.md
├── ADR-0002-navigation.md
├── ADR-0003-persistence.md
├── ADR-0004-pdf-engine.md
├── ADR-0005-ocr-engine.md
└── ADR-0006-security-architecture.md
```

Each ADR SHOULD document:

* Context
* Problem
* Options considered
* Decision
* Consequences

---

# 55. Architecture Evolution

The architecture is not immutable.

It may evolve when:

* Requirements change.
* Android platform requirements change.
* A dependency becomes unsuitable.
* Performance measurements reveal a problem.
* Security research reveals a weakness.
* A better solution becomes available.

Architectural changes MUST be deliberate.

Existing functionality MUST be protected by appropriate tests before significant architectural changes.

---

# 56. Avoiding Overengineering

FilePilot MUST NOT implement abstractions merely because they appear architecturally sophisticated.

Architecture should solve real problems.

The following principle applies:

> **Use the simplest architecture that can safely support the current requirement and foreseeable growth.**

If a feature is simple, its implementation may be simple.

If a subsystem is complex or security-sensitive, it deserves stronger boundaries.

---

# 57. Dependency Direction Rule

The following dependency direction is preferred:

```text
Presentation
     ↓
Application
     ↓
Domain
     ↑
Data
     ↑
Infrastructure
```

More precisely, stable contracts belong closer to the domain/application layers while concrete implementations live toward the infrastructure boundary.

UI MUST NOT become the source of business rules.

---

# 58. Core vs Feature Ownership

Shared functionality belongs in `core` only when it is genuinely shared infrastructure.

A feature-specific implementation MUST remain inside its feature.

We MUST avoid creating a giant `core` folder containing unrelated business logic.

---

# 59. FilePilot Engines

The major engines are expected to include:

```text
File Engine
Search Engine
Storage Engine
Preview Engine
Thumbnail Engine
PDF Engine
OCR Engine
Security Engine
Job Engine
```

These engines MUST have clear responsibilities.

They should communicate through defined contracts rather than directly manipulating each other's internal state.

---

# 60. Product Ecosystem Boundary

FilePilot will incorporate capabilities proven in the separate Screenshot OCR application.

However:

```text
Screenshot OCR App
        │
        │ proven concepts / implementation knowledge
        ▼
   FilePilot OCR
```

FilePilot MUST NOT become dependent on the existing Screenshot OCR application's runtime, database, package, or application lifecycle.

The products remain independently deployable.

---

# 61. Future Expansion

The architecture should allow future modules such as:

* Cloud storage
* SMB
* FTP/SFTP
* Network discovery
* Advanced document scanning
* Notes
* QR tools
* Archive management
* Advanced image tools
* Additional OCR engines
* AI-assisted document processing

These capabilities MUST NOT be allowed to destabilize the offline core.

---

# 62. Commercial Architecture

Commercial functionality such as premium features MUST be separated from core file ownership and storage.

Purchasing a premium capability MUST NOT change the fundamental ownership or accessibility of the user's files.

The architecture should allow premium capabilities to be enabled without duplicating the core application.

---

# 63. Premium Feature Boundary

Premium functionality should be implemented as capabilities layered on top of shared engines.

For example:

```text
                    FilePilot
                        │
              ┌─────────┴─────────┐
              │                   │
            Core              Premium
              │                   │
        File Engine         Advanced PDF
        Search              OCR workflows
        Explorer            Encryption
        Basic Preview       Batch tools
```

This avoids creating separate versions of fundamental file-management logic.

---

# 64. Data Integrity

FilePilot must favor data integrity over speed when the two conflict during destructive or irreversible operations.

For important operations, the architecture SHOULD consider:

* Temporary files
* Atomic replacement
* Verification
* Rollback
* Recovery
* Interrupted-operation handling

The implementation details will be defined per operation.

---

# 65. Observability

FilePilot should provide enough diagnostic information to understand failures without compromising privacy.

Production diagnostics MUST avoid collecting file contents or sensitive user information.

Where crash reporting is eventually introduced, its privacy implications MUST be reviewed before implementation.

---

# 66. Security Review

Security-sensitive functionality requires additional review before release.

This includes:

* Encryption
* Secure vault
* Key storage
* Authentication
* Sensitive permissions
* External sharing
* Network communication
* Cloud synchronization

Security requirements MUST take precedence over convenience.

---

# 67. Architecture Quality Gates

Before a major milestone is accepted:

### Build

The project builds successfully.

### Analysis

Static analysis passes without unexplained warnings.

### Tests

Relevant automated tests pass.

### Security

No known critical security issues remain.

### Performance

No unacceptable regressions are identified.

### Documentation

Relevant architecture and product documents are current.

---

# 68. Architecture Governance

The architecture will be governed by the following hierarchy:

```text
Product Vision
      ↓
Product Requirements
      ↓
Architecture
      ↓
Architecture Decisions
      ↓
Implementation
```

A lower-level implementation MUST NOT silently contradict a higher-level requirement.

If a conflict is discovered, the conflict must be discussed and the relevant document updated.

---

# 69. What Architecture Does Not Decide

This document intentionally does NOT permanently lock down:

* Exact Flutter package choices
* Exact state-management package
* Exact routing package
* Exact database package
* Exact PDF library
* Exact OCR library
* Exact encryption library
* Exact Android API implementation

Those decisions require technical evaluation and, where significant, an ADR.

This prevents premature commitment to technologies before we have validated them.

---

# 70. Architectural North Star

FilePilot should ultimately feel simple to the user even though the technology underneath is sophisticated.

The user should see:

> **One application. One workspace. One trusted place for their files and documents.**

Underneath that experience should be:

* Clear boundaries
* Strong security
* Reliable storage operations
* Modular engines
* Testable code
* Replaceable infrastructure
* Controlled dependencies
* Long-term maintainability

---

# 71. Final Architectural Principle

The most important architectural rule for FilePilot is:

> **Complexity belongs inside the architecture, not in the user's experience.**

The user should never need to understand how FilePilot is constructed.

They should simply experience an application that is:

**Fast. Private. Reliable. Powerful. Simple.**

---

**End of Architecture Blueprint**
