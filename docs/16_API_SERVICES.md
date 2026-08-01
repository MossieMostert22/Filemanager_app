# FilePilot API & Services Specification
**Version:** 1.0  
**Status:** Approved for Development  
**Owner:** FilePilot Project  
**Last Updated:** August 2026

---

# 1. Purpose

This document defines the internal service architecture used throughout FilePilot.

Although Version 1 is a fully offline application with no cloud backend, the application relies on a collection of internal services that provide clear separation of responsibilities.

Every service exposes a well-defined interface while remaining independent of presentation logic.

---

# 2. Service Architecture

FilePilot follows a layered service architecture.

```
UI

↓

Controllers

↓

Use Cases

↓

Repositories

↓

Services

↓

Platform APIs
```

Each layer has a single responsibility.

Business logic never communicates directly with Android APIs.

---

# 3. Design Principles

Every service must be:

- modular
- testable
- replaceable
- asynchronous
- dependency injected
- platform independent where practical

---

# 4. Core Services

Version 1 includes the following core services:

- File Service
- Storage Service
- OCR Service
- PDF Service
- Search Service
- Thumbnail Service
- Cache Service
- Settings Service
- Permission Service
- Share Service
- Logging Service

Each service owns one specific area of responsibility.

---

# 5. File Service

## Purpose

Provides a unified interface for working with user files.

### Responsibilities

- browse files
- rename
- move
- copy
- delete
- create folders
- retrieve metadata
- recent files
- favorites

The UI must never manipulate files directly.

---

# 6. Storage Service

## Purpose

Provides access to Android storage.

Uses:

- Storage Access Framework
- Scoped Storage
- MediaStore

Responsibilities:

- folder access
- URI resolution
- storage permissions
- directory selection

Direct filesystem paths should be avoided whenever Android recommends URI-based access.

---

# 7. OCR Service

## Purpose

Extract text from images completely offline.

Preferred implementation:

Google ML Kit (On-device)

Responsibilities:

- image preprocessing
- OCR execution
- language detection (future)
- confidence scoring
- structured text results

OCR requests execute asynchronously.

---

# 8. OCR Processing Pipeline

```
Image

↓

Resize

↓

Rotate

↓

Enhance

↓

OCR Engine

↓

Extract Text

↓

Return Result
```

Intermediate image buffers should be released immediately after processing.

---

# 9. PDF Service

## Purpose

Generate PDF documents locally.

Responsibilities:

- create PDF
- merge pages
- compress images
- page layout
- save document
- export

Generation should support progress reporting.

---

# 10. Search Service

## Purpose

Provide fast local search.

Search sources include:

- filenames
- folder names
- OCR text
- favorites
- metadata

Search remains fully offline.

---

# 11. Index Service

Responsible for:

- OCR indexing
- metadata indexing
- search updates
- incremental indexing

Large indexing operations execute in the background.

---

# 12. Thumbnail Service

Generates preview images.

Supported file types:

- images
- PDFs
- supported documents (future)

Requirements:

- memory efficient
- cached
- asynchronously generated

---

# 13. Cache Service

Responsible for:

- thumbnail cache
- OCR cache
- temporary images
- generated previews

The cache should automatically purge expired data.

---

# 14. Settings Service

Stores application preferences.

Examples:

- theme
- default sorting
- home layout
- onboarding status
- favorites
- accessibility options

Settings remain local.

---

# 15. Permission Service

Centralizes runtime permission handling.

Responsibilities:

- permission requests
- rationale dialogs
- permission state
- denial recovery

Permissions should only be requested when required.

---

# 16. Share Service

Provides integration with Android sharing.

Supports:

- share files
- share PDFs
- share extracted text

Uses Android Intent APIs.

---

# 17. Logging Service

Provides structured logging.

Development:

- verbose logging

Release:

- warnings
- recoverable errors
- critical failures

No personal data should appear in logs.

---

# 18. Notification Service

Version 1 provides lightweight notifications for long-running tasks.

Examples:

- OCR completed
- PDF completed
- processing failed

Notifications should remain optional where supported.

---

# 19. Background Processing Service

Heavy operations execute outside the UI thread.

Includes:

- OCR
- PDF generation
- indexing
- thumbnail creation

Use Dart isolates where appropriate.

---

# 20. Repository Layer

Repositories abstract services from business logic.

Example:

```
UI

↓

FileController

↓

FileRepository

↓

FileService

↓

Android Storage
```

This allows services to be replaced without affecting higher layers.

---

# 21. Native Android Integration

Platform channels should remain isolated.

Native integrations include:

- Storage Access Framework
- MediaStore
- Android Intents
- Document Provider APIs
- Share Sheet

Flutter widgets should never invoke platform channels directly.

---

# 22. Error Handling

Every service returns predictable results.

Preferred approaches include:

- Result<T>
- sealed classes
- typed exceptions

Avoid returning null where possible.

---

# 23. Service Communication

Services should not depend directly on each other unless necessary.

Preferred flow:

```
Controller

↓

Use Case

↓

Repository

↓

Single Service
```

Complex orchestration belongs in use cases.

---

# 24. Dependency Injection

All services are registered centrally.

Recommended approach:

Riverpod Providers

or

Service Locator (if justified)

Direct object creation inside UI code is prohibited.

---

# 25. Thread Safety

Services performing long-running work must:

- avoid shared mutable state
- support cancellation
- prevent duplicate execution
- remain safe for concurrent operations

---

# 26. Performance Requirements

Target goals:

- Service initialization <100ms
- OCR startup <300ms
- Search response <100ms
- File listing <200ms
- Thumbnail generation asynchronous
- No UI blocking

Performance should be measured on representative low- and mid-range Android devices.

---

# 27. Future Service Expansion

Future versions may introduce:

- Cloud Sync Service
- AI Assistant Service
- Backup Service
- Document Scanner Service
- Translation Service
- Voice Search Service

These services remain optional and must not compromise the offline-first philosophy.

---

# 28. Service Versioning

Service interfaces should remain stable.

Breaking changes require:

- documentation updates
- migration strategy
- regression testing

Backward compatibility should be preserved whenever practical.

---

# 29. Testing

Every service should include:

- unit tests
- mock implementations
- integration tests where applicable

Critical services (OCR, Storage, PDF) require high test coverage.

---

# 30. Monitoring

Development builds may collect:

- execution time
- memory usage
- processing duration
- cache statistics

Release builds should avoid unnecessary telemetry.

---

# 31. Security Requirements

Services must never:

- expose private file paths
- transmit user content
- bypass permission checks
- store sensitive information in logs

Security principles defined in **12_SECURITY_PRIVACY.md** apply to every service.

---

# 32. API Stability

Public service interfaces should evolve cautiously.

When introducing new methods:

- preserve existing contracts
- document behavior
- maintain backward compatibility where feasible

Internal refactoring should not affect consumers.

---

# 33. Documentation Requirements

Each service must document:

- purpose
- responsibilities
- public methods
- expected inputs
- expected outputs
- possible exceptions
- performance considerations

Service documentation should remain synchronized with implementation.

---

# 34. Service Governance

The Project Architect approves:

- new services
- interface changes
- platform integrations
- external SDK adoption

Architectural consistency takes precedence over short-term implementation convenience.

---

# 35. Vision

Services are the operational backbone of FilePilot.

By isolating platform functionality behind clear interfaces, the application remains modular, testable, and adaptable as Android evolves.

Every service should perform one responsibility exceptionally well, enabling the product to grow without compromising stability, performance, or maintainability.