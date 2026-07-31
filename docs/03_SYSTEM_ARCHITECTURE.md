# FilePilot — System Architecture

**Document:** 03_SYSTEM_ARCHITECTURE.md
**Version:** 1.0
**Status:** Approved
**Last Updated:** 31 July 2026

---

# 1. Purpose

This document defines the technical architecture for FilePilot Version 1.

It establishes the engineering standards, architectural principles, project structure, and technology decisions that will guide implementation.

The architecture has been designed for long-term maintainability, scalability, performance, and testability.

---

# 2. Architecture Goals

FilePilot shall be:

* Modular
* Maintainable
* Testable
* Offline-first
* Privacy-first
* High performance
* Easy to extend
* Simple to understand

The architecture must support years of future development without requiring major restructuring.

---

# 3. Architectural Principles

The project follows these engineering principles:

* Clean Architecture
* SOLID Principles
* Separation of Concerns
* Dependency Inversion
* Feature-first organization
* Repository Pattern
* Immutable state where practical
* Explicit dependency injection

Business logic shall never depend on UI frameworks.

---

# 4. Technology Stack

## Framework

Flutter (latest stable)

---

## Language

Dart (latest stable)

---

## Architecture

Clean Architecture

---

## State Management

Riverpod

Reasons:

* compile-time safety
* minimal boilerplate
* excellent testing support
* dependency injection integration
* scalable provider model

---

## Routing

GoRouter

---

## Local Database

Isar Database

Reasons:

* extremely fast
* offline-first
* Flutter native
* excellent query performance

---

## Preferences

SharedPreferences

Used only for lightweight configuration.

Examples:

* theme
* grid/list preference
* onboarding status

---

## File Access

Android Storage Access Framework (SAF)

Where appropriate, the application shall use modern Android storage APIs.

---

## OCR Engine

Google ML Kit (On-device Text Recognition)

Requirements:

* offline
* on-device processing
* no cloud dependency

---

## PDF Engine

Flutter PDF ecosystem

Capabilities:

* PDF creation
* page management
* compression
* metadata

---

## Image Processing

Native Flutter image libraries where appropriate.

---

## Logging

Structured logging.

Production logging shall avoid exposing sensitive user information.

---

# 5. Clean Architecture Layers

The application shall be divided into four primary layers.

```
Presentation

↓

Application

↓

Domain

↓

Data
```

Each layer has one direction of dependency.

Outer layers depend on inner layers.

Inner layers never depend on outer layers.

---

# 6. Layer Responsibilities

## Presentation

Responsible for:

* UI
* Screens
* Widgets
* Navigation
* Animations
* User interaction

Contains no business rules.

---

## Application

Responsible for:

* Controllers
* State
* Coordinating use cases
* User workflows

This layer connects Presentation with Domain.

---

## Domain

Contains:

* business rules
* entities
* repository contracts
* use cases

The Domain layer contains no Flutter code.

It should compile independently of the user interface.

---

## Data

Responsible for:

* repositories
* local database
* storage
* OCR integration
* PDF generation
* Android services

Implements repository interfaces defined by the Domain layer.

---

# 7. Project Structure

```
lib/

├── app/
│   ├── application/
│   ├── routing/
│   ├── theme/
│   └── startup/
│
├── core/
│   ├── constants/
│   ├── errors/
│   ├── extensions/
│   ├── logging/
│   ├── permissions/
│   ├── services/
│   ├── utilities/
│   └── widgets/
│
├── features/
│
│   ├── home/
│   ├── files/
│   ├── search/
│   ├── ocr/
│   ├── pdf/
│   ├── favorites/
│   ├── recents/
│   ├── storage/
│   ├── settings/
│   └── premium/
│
└── main.dart
```

Every feature remains self-contained.

---

# 8. Feature Structure

Each feature follows the same internal structure.

```
feature/

├── presentation/
│
├── application/
│
├── domain/
│
└── data/
```

Example:

```
ocr/

presentation/

application/

domain/

data/
```

This keeps every feature independent.

---

# 9. Dependency Injection

Dependency Injection shall be implemented using Riverpod Providers.

Rules:

* no global singletons
* dependencies injected explicitly
* repositories injected
* services injected
* controllers injected

This enables unit testing without mocks scattered throughout the project.

---

# 10. Repository Pattern

Repositories define interfaces in Domain.

Implementations exist only inside Data.

Example:

```
abstract class FileRepository

↓

LocalFileRepository
```

Presentation never communicates directly with storage.

---

# 11. State Management

State shall remain predictable.

Preferred state types:

* immutable models
* AsyncValue
* StateNotifier
* Notifier

Avoid unnecessary mutable state.

---

# 12. Navigation Architecture

Navigation uses GoRouter.

Primary routes:

```
/

home

files

search

ocr

pdf

favorites

settings

premium
```

Nested routes should remain shallow.

Avoid deeply nested navigation.

---

# 13. Error Handling

All errors shall be handled gracefully.

Categories include:

* file errors
* permission errors
* OCR failures
* PDF failures
* storage failures

Users should receive helpful messages rather than technical exceptions.

Errors shall be logged while protecting user privacy.

---

# 14. Background Processing

Long-running tasks shall execute asynchronously.

Examples:

* OCR
* PDF generation
* large file operations

Requirements:

* progress indicators
* cancellation support (where practical)
* responsive UI
* safe lifecycle handling

Foreground blocking is prohibited.

---

# 15. Performance Strategy

Performance objectives:

* launch under 2 seconds
* smooth scrolling
* lazy loading
* thumbnail caching
* efficient file indexing
* minimal rebuilds
* asynchronous IO

Optimize for both flagship and entry-level Android devices.

---

# 16. Memory Management

The application shall avoid loading unnecessary data.

Guidelines:

* stream large files
* dispose controllers correctly
* cache only when beneficial
* limit image memory usage
* process PDFs page-by-page
* avoid duplicate bitmap allocations

Target support includes devices with 3 GB RAM.

---

# 17. Security Architecture

The application shall:

* request minimum permissions
* use scoped storage where possible
* validate file operations
* sanitize filenames
* prevent path traversal
* avoid insecure temporary storage

No user content shall be uploaded automatically.

---

# 18. Offline Strategy

Core functionality must work entirely offline.

Offline modules include:

* File browsing
* Search
* OCR
* PDF creation
* Favorites
* Recent files
* Storage overview

Internet connectivity enhances the application but is never required for core functionality.

---

# 19. Testing Strategy

Testing levels:

### Unit Tests

* domain
* use cases
* repositories
* utilities

---

### Widget Tests

* screens
* widgets
* interactions

---

### Integration Tests

* OCR workflow
* PDF workflow
* file browsing
* navigation

---

### Manual QA

Before every release:

* accessibility
* permissions
* performance
* Android version compatibility
* Play Store compliance

---

# 20. Coding Standards

The project follows:

* Effective Dart Guidelines
* Flutter Lints
* Meaningful naming
* Small focused classes
* Small methods
* Comprehensive documentation

Magic numbers are prohibited.

Business logic inside widgets is prohibited.

---

# 21. Build Variants

Supported builds:

Development

* debugging enabled
* verbose logging

---

Staging

* beta testing
* production configuration
* limited diagnostics

---

Production

* optimized
* obfuscated
* minimal logging
* analytics (privacy compliant)

---

# 22. Scalability

The architecture is designed to support future modules including:

* AI-assisted search
* Cloud synchronization
* Duplicate detection
* Automation
* Document scanning
* File encryption
* Android tablets
* Desktop platforms

No architectural redesign should be required to support these additions.

---

# 23. Technical Debt Policy

Technical debt shall not accumulate unchecked.

Every release should include:

* dependency updates
* code cleanup
* refactoring
* performance improvements
* test improvements

Long-term maintainability has equal priority to new features.

---

# 24. Architecture Decision Records

Significant architectural decisions shall be documented using ADRs.

Examples include:

* selecting Riverpod
* choosing Isar
* OCR engine changes
* storage architecture
* synchronization strategy

ADRs shall be stored in:

```
docs/adr/
```

---

# 25. Engineering Principles

Every implementation should strive to be:

* readable
* predictable
* testable
* reusable
* performant
* privacy-respecting

Code should be written for future maintainers as much as for current developers.

---

# System Architecture Statement

> **FilePilot is built on a modular Clean Architecture that prioritizes maintainability, privacy, offline capability, and long-term scalability. Every architectural decision should support these principles while enabling continuous evolution without unnecessary complexity.**

---

**End of Document**
