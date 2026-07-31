# FilePilot Development Standards

**Document Version:** 1.0  
**Status:** Approved  
**Last Updated:** 31 July 2026

---

# 1. Purpose

This document defines the engineering standards governing the development of FilePilot.

Its purpose is to ensure that every contributor produces software that is:

- Maintainable
- Consistent
- Secure
- Performant
- Testable
- Scalable

These standards apply to every source file, feature, test, pull request, and future release.

No contributor is exempt from these standards.

---

# 2. Engineering Philosophy

FilePilot follows a long-term engineering philosophy rather than rapid feature accumulation.

Core principles:

- Build less, but build it exceptionally well.
- Quality before quantity.
- Every feature must solve a real user problem.
- Every screen must have a clear purpose.
- Simplicity always wins over complexity.
- Performance is a feature.
- Privacy is non-negotiable.
- Offline-first is the default.
- Technical debt must never become permanent debt.

Every engineering decision should support these principles.

---

# 3. Code Quality Principles

Every line of code should be:

- Readable
- Predictable
- Testable
- Reusable
- Self-documenting

Developers should optimize for readability before cleverness.

Avoid:

- Deep nesting
- Large classes
- Massive widgets
- Long methods
- Hidden side effects
- Duplicate logic

If code cannot be understood within a few minutes, it should be simplified.

---

# 4. SOLID Principles

All application architecture should follow SOLID.

## Single Responsibility

One class.
One responsibility.

## Open / Closed

Classes should be extendable without modification.

## Liskov Substitution

Implementations must behave consistently.

## Interface Segregation

Avoid oversized interfaces.

## Dependency Inversion

Depend on abstractions.

Never concrete implementations.

---

# 5. Clean Architecture

FilePilot follows Clean Architecture.

Application layers:

Presentation

↓

Application

↓

Domain

↓

Infrastructure

Each layer has clearly defined responsibilities.

Dependencies only point inward.

No shortcuts are allowed.

---

# 6. Folder Structure

Every feature follows the same layout.

```
lib/

core/
shared/

features/

feature/

presentation/

application/

domain/

infrastructure/
```

No feature may invent its own structure.

Consistency is mandatory.

---

# 7. Feature Isolation

Every feature is self-contained.

Example:

```
features/

ocr/

pdf/

search/

files/

settings/
```

A feature owns:

- UI
- Controllers
- Use cases
- Repositories
- Services
- Tests

Features should not directly access each other's internal code.

Communication occurs through interfaces.

---

# 8. Naming Standards

Names must describe intent.

Classes

```
PdfGenerator

OcrProcessor

FileRepository
```

Avoid:

```
Helper

Manager

Util

Data

Thing
```

Methods should be verbs.

```
generatePdf()

scanImage()

searchFiles()
```

Variables should use descriptive names.

```
recognizedText

storagePermission

selectedFiles
```

Never abbreviate unless universally understood.

---

# 9. File Naming

Use snake_case.

Examples:

```
ocr_service.dart

pdf_generator.dart

recent_files_card.dart
```

Never:

```
OCRService.dart

MyWidget.dart
```

---

# 10. Widget Standards

Widgets should remain small.

Recommended:

<200 lines.

Split widgets into reusable components.

Business logic never belongs inside Widgets.

Widgets display state.

They do not own business rules.

---

# 11. State Management

Riverpod is the official state management solution.

Rules:

Business logic belongs inside Providers.

Widgets observe Providers.

Never place application logic inside build().

Avoid mutable global state.

State should be:

- Predictable
- Testable
- Disposable

---

# 12. Dependency Injection

Dependency Injection is mandatory.

Dependencies must never instantiate themselves.

Correct:

```
constructor(repository)
```

Avoid:

```
repository = Repository()
```

Dependencies are provided by Riverpod.

---

# 13. Error Handling

Errors should never crash the application.

Expected failures include:

- Missing permissions
- Corrupt PDFs
- OCR failure
- Storage unavailable
- Missing files

Every failure should:

- Log internally
- Display friendly messaging
- Allow recovery where possible

Never expose stack traces to users.

---

# 14. Logging Standards

Logging should assist debugging.

Allowed:

Debug logs

Warning logs

Error logs

Never log:

Passwords

Private files

OCR text

Personal information

Production logs must avoid sensitive user data.

---

# 15. Exception Strategy

Never swallow exceptions.

Always:

Catch

Log

Handle

Recover

Unknown exceptions should reach centralized crash reporting.

---

# 16. Performance Standards

Performance is part of feature acceptance.

Application launch:

Target:

< 2 seconds

Search:

Instant

Scrolling:

60 FPS minimum

Animations:

Smooth

Avoid:

Blocking UI thread

Large synchronous loops

Unnecessary rebuilds

Heavy allocations

---

# 17. Background Processing

Heavy work executes asynchronously.

Examples:

OCR

PDF generation

Thumbnail generation

File indexing

Search indexing

These tasks must never block the interface.

Progress indicators are required.

---

# 18. Memory Management

Avoid unnecessary allocations.

Dispose:

Controllers

Streams

AnimationControllers

FocusNodes

Image resources

Never retain references unnecessarily.

---

# 19. Accessibility Standards

Accessibility is mandatory.

Requirements:

Minimum touch target:

48dp

Dynamic text scaling supported.

Proper semantic labels.

Screen reader compatibility.

Keyboard navigation where applicable.

Sufficient contrast ratios.

Accessibility testing forms part of Definition of Done.

---

# 20. Localization

Every user-visible string must be localizable.

Never hardcode text inside Widgets.

Example:

Correct:

```
context.strings.scanButton
```

Avoid:

```
Text("Scan")
```

Localization readiness is required from Version 1.

---

# 21. Theme Management

No hardcoded colors.

Use design tokens only.

Example:

```
AppColors.primary

AppColors.surface

AppColors.textPrimary
```

Spacing also uses constants.

Never magic numbers.

---

# 22. Dependency Management

Every package must satisfy:

Actively maintained

Well documented

Compatible license

Necessary

Packages require architectural justification.

Avoid dependency bloat.

---

# 23. Git Workflow

Branch strategy:

```
main

develop

feature/

bugfix/

hotfix/

release/
```

Never commit directly to main.

Every change passes Pull Request review.

---

# 24. Commit Message Standards

Use Conventional Commits.

Examples:

```
feat: add OCR workflow

fix: correct PDF export

docs: update architecture

refactor: simplify search provider

test: add OCR unit tests
```

Commit messages should describe intent.

---

# 25. Pull Request Requirements

Every Pull Request must include:

Purpose

Summary

Screenshots (UI)

Testing performed

Known limitations

Checklist completed

Small Pull Requests are preferred.

---

# 26. Code Review Checklist

Reviewers verify:

Architecture

Readability

Naming

Performance

Security

Accessibility

Testing

Documentation

No approval without review completion.

---

# 27. Documentation Standards

Every public class requires documentation.

Complex logic requires explanation.

Architecture changes require documentation updates.

Documentation is considered source code.

Outdated documentation is treated as a defect.

---

# 28. Testing Standards

Every feature requires testing.

Required:

Unit Tests

Widget Tests

Integration Tests (where applicable)

Bug fixes require regression tests.

Untested code should not be merged.

---

# 29. Security Standards

Never trust external input.

Validate:

Files

Paths

Names

Extensions

Permissions

Never execute unknown content.

Never expose filesystem internals.

---

# 30. Privacy Standards

Privacy is a core product principle.

Rules:

No user tracking.

No telemetry without consent.

No OCR data uploaded.

No file uploads without explicit user action.

Offline-first processing remains the default.

---

# 31. Performance Budget

Release targets:

Application size:

As small as practical.

Cold start:

<2 seconds

Memory usage:

Optimized for devices with 3 GB RAM.

Battery impact:

Minimal.

Background processing:

Efficient.

---

# 32. Definition of Done

A feature is complete only when:

✓ Requirements implemented

✓ Code reviewed

✓ Tests passing

✓ Documentation updated

✓ Localization complete

✓ Accessibility verified

✓ Performance verified

✓ No known critical bugs

Only then may the feature be merged.

---

# 33. Anti-Patterns

The following are prohibited:

- God classes
- Massive widgets
- Copy-paste code
- Hardcoded colors
- Hardcoded strings
- Business logic in UI
- Global mutable state
- Circular dependencies
- Silent failures
- Dead code
- Commented-out production code

These must be removed during review.

---

# 34. Future Contributions

Future contributors must preserve the architecture defined within this repository.

New features must:

- Follow Clean Architecture.
- Follow design tokens.
- Respect Version 1 philosophy.
- Maintain offline-first behaviour.
- Preserve privacy.
- Include testing.
- Include documentation.

Consistency is more valuable than individual coding style.

---

# 35. Engineering Commitment

FilePilot is intended to become a long-term product rather than a short-lived application.

Every engineering decision should prioritize:

- Stability
- Simplicity
- Maintainability
- User trust
- Performance
- Accessibility
- Privacy

We do not build software simply to release it.

We build software that users can depend on for years.

---

**End of Document**