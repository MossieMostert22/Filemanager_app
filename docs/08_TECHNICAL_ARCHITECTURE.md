# 08_TECHNICAL_ARCHITECTURE.md

> **Project:** FilePilot
>
> **Document Version:** 1.0
>
> **Status:** Architecture Frozen (Version 1)
>
> **Last Updated:** 31 July 2026

---

# Technical Architecture

## Purpose

This document defines the complete technical architecture for FilePilot Version 1.

It establishes the engineering principles, project structure, technology stack, coding standards, dependency management, and architectural patterns that guide development.

This document exists to ensure that FilePilot remains:

* Maintainable
* Testable
* Scalable
* Modular
* Offline-first
* Easy to extend

Future features should be added by extending the architecture—not replacing it.

---

# Engineering Principles

Every technical decision must support the following principles:

1. Offline First
2. Clean Architecture
3. Feature First Organization
4. Modular Development
5. High Testability
6. Performance First
7. Security by Design
8. Simplicity Over Cleverness

---

# Technology Stack

## Framework

Flutter (Stable)

Reason:

* Android-first
* Cross-platform future
* Excellent performance
* Large ecosystem
* Strong community support

---

## Language

Dart

Modern language features:

* Null Safety
* Records
* Pattern Matching
* Extensions
* Sealed Classes

---

## State Management

Riverpod

Reasons:

* Compile-time safety
* Excellent testing support
* Dependency injection
* Scalable architecture
* Minimal boilerplate

---

## Navigation

GoRouter

Reasons:

* Official Flutter recommendation
* Deep linking
* Route guards
* Declarative navigation

---

## Local Database

SQLite

Primary usage:

* OCR indexing
* Metadata
* Search
* Recent history

Recommended package:

drift

Reasons:

* Type-safe SQL
* Reactive queries
* Migrations
* Excellent tooling

---

## Local Object Storage

Hive or Isar

Purpose:

* Settings
* Preferences
* Small cached objects
* Lightweight data

Selection should be based on benchmarking during implementation.

---

## OCR Engine

Google ML Kit

Requirements:

* Offline processing
* On-device recognition
* High accuracy
* No cloud dependency

---

## PDF Engine

pdf package

Responsibilities:

* PDF generation
* Page rendering
* Metadata
* Image insertion

---

## Image Processing

image package

Responsibilities:

* Resize
* Rotate
* Crop
* Compression

---

## File Access

path_provider

permission_handler

file_picker

---

# Architecture Style

FilePilot uses:

Clean Architecture

combined with

Feature First organization.

---

## High-Level Layers

```text
Presentation
       │
       ▼
Application
       │
       ▼
Domain
       │
       ▼
Infrastructure
```

Dependencies always point inward.

The Domain layer never depends on Flutter.

---

# Project Structure

```text
lib/

core/
    constants/
    errors/
    extensions/
    services/
    theme/
    utils/

features/

    home/
    files/
    search/
    ocr/
    pdf/
    settings/
    favorites/
    storage/

shared/

main.dart
```

Each feature owns its implementation.

---

# Feature Structure

Each feature follows:

```text
feature/

presentation/
    pages/
    widgets/
    providers/

application/
    usecases/
    services/

domain/
    entities/
    repositories/

infrastructure/
    datasources/
    models/
    repository_impl/

```

This structure keeps business logic isolated from UI.

---

# Core Layer

Contains reusable components.

Examples:

* Logger
* Theme
* Error Handling
* Permissions
* Utilities
* Constants
* Analytics interfaces
* Background task helpers

No feature-specific code belongs here.

---

# Shared Layer

Shared UI components only.

Examples:

* Buttons
* Dialogs
* Cards
* Empty States
* Progress Indicators
* Search Field
* File Tile

Avoid business logic in shared widgets.

---

# Dependency Injection

Riverpod providers act as the dependency injection container.

Benefits:

* Lazy loading
* Test overrides
* Scoped dependencies
* Compile-time safety

No global service locators.

---

# Data Flow

```text
UI

↓

Provider

↓

Use Case

↓

Repository

↓

Data Source

↓

Storage / OCR / File System
```

Each layer has a single responsibility.

---

# Repository Pattern

Every feature exposes interfaces.

Example:

```text
OCRRepository

SearchRepository

FileRepository

PDFRepository
```

Infrastructure implements these interfaces.

Presentation never accesses storage directly.

---

# Background Processing

Long-running operations must execute asynchronously.

Examples:

* OCR
* PDF generation
* Storage scanning
* Thumbnail creation
* Search indexing

UI must remain responsive.

---

# Task Pipeline

```text
Start Task

↓

Background Worker

↓

Progress Updates

↓

Completion

↓

UI Notification
```

Blocking the UI thread is prohibited.

---

# Error Handling

All failures return typed results.

Example:

```text
Success

Failure

Cancelled
```

Avoid throwing exceptions across architectural boundaries.

Use domain-specific failures.

---

# Logging

Logging levels:

DEBUG

INFO

WARNING

ERROR

Logging must never expose:

* File contents
* Personal information
* OCR text
* User documents

---

# Security

Version 1 principles:

* No cloud storage
* No telemetry by default
* No user tracking
* No hidden uploads

Files remain on-device unless the user explicitly shares them.

---

# Permissions

Only request permissions when required.

Examples:

Storage

Only when browsing files.

Notifications

Only when enabling reminders or background notifications.

Never request unnecessary permissions.

---

# Search Architecture

Search indexes:

* File names
* OCR text
* PDF metadata
* Folder names
* Favorites

Future semantic indexing is intentionally deferred.

---

# OCR Architecture

Pipeline:

```text
Image

↓

Pre-processing

↓

ML Kit OCR

↓

Extracted Text

↓

Index

↓

Save
```

OCR output should automatically become searchable.

---

# PDF Architecture

Pipeline:

```text
Images

↓

Optimize

↓

Generate Pages

↓

Compile PDF

↓

Save
```

Generation runs in a background isolate where practical.

---

# Storage Architecture

Version 1 provides:

* Internal Storage
* External Storage (where supported)
* SD Card (device dependent)

Cloud providers are excluded from Version 1.

---

# Caching

Cache only lightweight, reproducible data:

* Thumbnails
* Search indexes
* Metadata

Never duplicate large user files unnecessarily.

---

# Performance Targets

Cold Launch:

< 2 seconds

Warm Launch:

< 1 second

Search:

< 300 ms

OCR Start:

< 500 ms

Thumbnail Loading:

< 150 ms

Scrolling:

60 FPS

---

# Memory Management

Large operations must process data incrementally.

Requirements:

* Stream large files where possible
* Release image buffers immediately
* Avoid loading multiple full-resolution images simultaneously
* Use isolates for memory-intensive work

This reduces Out Of Memory (OOM) risk on lower-end Android devices.

---

# Accessibility Support

Architecture must support:

* Dynamic font scaling
* Screen readers
* High contrast themes
* Reduced motion preferences
* 48 dp touch targets

Accessibility is a platform requirement, not an enhancement.

---

# Testing Strategy

Testing pyramid:

Unit Tests

Repository Tests

Widget Tests

Integration Tests

End-to-End Tests

Every new feature should include automated tests appropriate to its complexity.

---

# Code Standards

Follow Effective Dart guidelines.

Requirements:

* Small focused classes
* Single Responsibility Principle
* Prefer composition over inheritance
* Immutable models where practical
* Avoid deeply nested widgets
* Avoid business logic in UI

---

# Branch Strategy

Git Flow:

```text
main

develop

feature/*

release/*

hotfix/*
```

Every Pull Request requires:

* Code review
* Passing CI
* Successful automated tests

---

# Continuous Integration

CI pipeline should validate:

* Flutter analyze
* Formatting
* Unit tests
* Widget tests
* Build verification

No code is merged if any mandatory check fails.

---

# Package Management

Only introduce external packages when they provide clear value.

Before adding a dependency, evaluate:

* Maintenance activity
* Community adoption
* License compatibility
* Performance
* Long-term support

Prefer mature packages over experimental alternatives.

---

# Future Scalability

This architecture is designed to support future additions without major restructuring.

Potential future modules include:

* AI document assistance
* Cloud synchronization
* Secure vault
* Batch processing
* Duplicate detection
* Automation rules
* Desktop support
* Tablet layouts
* Wear OS companion

Each feature should be implemented as an independent module following the same architectural principles.

---

# Version 1 Technical Scope

Included:

* Local file management
* OCR
* Search
* PDF generation
* Storage overview
* Favorites
* Settings
* Offline operation

Deferred:

* Cloud services
* AI-assisted workflows
* Collaboration
* Background synchronization
* Semantic search
* Automation engine

---

# Conclusion

The FilePilot architecture is intentionally designed for longevity rather than short-term convenience.

By combining Clean Architecture with a feature-first modular structure, strict separation of concerns, and an offline-first philosophy, the project establishes a robust engineering foundation that can evolve without sacrificing maintainability or performance.

Every future enhancement should integrate into this architecture through extension—not reinvention—ensuring FilePilot remains reliable, scalable, and easy to develop for many years.
