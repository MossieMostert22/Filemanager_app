# FilePilot Coding Guidelines
**Version:** 1.0  
**Status:** Approved for Development  
**Owner:** FilePilot Project  
**Last Updated:** August 2026

---

# 1. Purpose

This document establishes the coding standards, architectural conventions, and development practices for the FilePilot project.

Its purpose is to ensure that all code is:

- readable
- maintainable
- scalable
- testable
- secure
- consistent across the entire application

Every contributor is expected to follow these guidelines.

---

# 2. Development Philosophy

FilePilot follows one simple principle:

> Write code that your future self will thank you for.

Prioritize:

- clarity over cleverness
- simplicity over complexity
- maintainability over shortcuts
- consistency over personal preference

Readable code always wins.

---

# 3. Architecture

FilePilot follows **Clean Architecture**.

```
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

Business logic must never depend on UI or external frameworks.

---

# 4. SOLID Principles

All code should follow SOLID design principles.

### Single Responsibility

Each class should have one reason to change.

---

### Open / Closed

Software should be open for extension and closed for modification.

---

### Liskov Substitution

Derived classes must behave as expected when replacing base classes.

---

### Interface Segregation

Keep interfaces small and focused.

---

### Dependency Inversion

Depend on abstractions rather than concrete implementations.

---

# 5. Project Structure

```
lib/

core/

features/

shared/

services/

data/

domain/

presentation/

widgets/

models/

repositories/

usecases/
```

Every feature should remain self-contained wherever practical.

---

# 6. File Naming

Use lowercase with underscores.

Examples:

```
file_service.dart

ocr_repository.dart

recent_files_page.dart

search_controller.dart
```

Avoid:

```
FileService.dart

myFile.dart

OCRRepo.dart
```

---

# 7. Class Naming

Use PascalCase.

Examples:

```
FileRepository

PdfGenerator

OcrController

SearchService
```

---

# 8. Variable Naming

Use camelCase.

Examples:

```
recentFiles

ocrResult

selectedFolder

processingQueue
```

Names should describe intent.

Avoid abbreviations unless universally understood.

---

# 9. Method Naming

Methods should describe actions.

Good:

```
loadFiles()

createPdf()

extractText()

sortDocuments()

deleteCache()
```

Avoid vague names:

```
run()

execute()

handle()

process()

doStuff()
```

---

# 10. Constants

Use lowerCamelCase for constants.

Group related constants into dedicated files.

Example:

```
appPadding

animationDuration

defaultRadius

maxPdfPages
```

---

# 11. Widgets

Widgets should remain focused.

Guidelines:

- one responsibility
- minimal state
- reusable where appropriate

Avoid widgets exceeding approximately **300 lines**.

Extract reusable components early.

---

# 12. Functions

Functions should:

- perform one task
- remain concise
- avoid deep nesting
- return predictable results

Target length:

20–40 lines where practical.

---

# 13. Comments

Code should explain **why**, not **what**.

Good:

```dart
// Prevent duplicate indexing after OCR completes.
```

Avoid:

```dart
// Increment i.
i++;
```

Dead code should be removed—not commented out.

---

# 14. Documentation

Every public class should include a Dart documentation comment.

Example:

```dart
/// Provides OCR functionality for locally stored images.
class OcrService {}
```

Public methods should be documented when behavior is not immediately obvious.

---

# 15. Error Handling

Never ignore exceptions.

Preferred pattern:

```dart
try {
  ...
} catch (e) {
  ...
}
```

Provide meaningful error messages.

Avoid empty catch blocks.

---

# 16. Logging

Use structured logging.

Development builds may log:

- execution flow
- timing
- warnings
- recoverable errors

Release builds must never log:

- filenames
- OCR text
- user documents
- personal information

---

# 17. Null Safety

Null Safety is mandatory.

Avoid forced null assertions (`!`) unless there is a documented guarantee.

Prefer:

```dart
value?.length
```

instead of:

```dart
value!.length
```

---

# 18. Asynchronous Code

Always use asynchronous APIs for:

- OCR
- PDF generation
- file scanning
- indexing
- storage access

Never block the UI thread.

Prefer:

```dart
async / await
```

over chained callbacks.

---

# 19. State Management

Use Riverpod consistently.

Business logic belongs in providers, controllers, or use cases—not inside widgets.

Widgets should focus on presentation.

---

# 20. Dependency Injection

Avoid directly instantiating services inside widgets.

Preferred:

```
Widget

↓

Controller

↓

Repository

↓

Service
```

Dependencies should be injected to improve testability and flexibility.

---

# 21. Magic Numbers

Avoid unexplained numeric values.

Instead of:

```dart
padding: 17
```

Prefer:

```dart
padding: AppSpacing.medium
```

Centralize design values.

---

# 22. UI Consistency

Use design tokens.

Never hardcode:

- colors
- typography
- spacing
- corner radius
- animation durations

Always reference the Design System.

---

# 23. Performance

Avoid unnecessary rebuilds.

Use:

- const constructors
- lazy loading
- pagination
- memoization where appropriate

Optimize only after measurement.

---

# 24. Testing Requirements

Every new feature should include:

- unit tests
- widget tests where applicable

Critical workflows require integration tests.

---

# 25. Linting

The project uses Flutter's recommended lint rules.

Warnings should be resolved before merging.

Do not suppress lints without documented justification.

---

# 26. Code Reviews

Every pull request should verify:

- readability
- correctness
- architecture compliance
- performance impact
- security considerations
- test coverage
- documentation updates

Reviewers should suggest improvements respectfully and constructively.

---

# 27. Git Commit Messages

Use concise, descriptive commit messages.

Examples:

```
Add OCR extraction service

Improve PDF generation performance

Fix search indexing bug

Refactor file repository
```

Avoid generic messages such as:

```
Update

Changes

Fix stuff
```

---

# 28. Branch Strategy

Recommended workflow:

```
main

develop

feature/*

bugfix/*

release/*

hotfix/*
```

Feature branches should remain focused on a single objective.

---

# 29. Code Quality Checklist

Before merging, verify:

- Code builds successfully
- Tests pass
- No analyzer warnings
- No unnecessary dependencies
- Documentation updated
- Naming conventions followed
- No commented-out code
- No debug prints in release code

---

# 30. Refactoring

Refactor when:

- duplication appears
- complexity increases
- readability declines
- architecture becomes inconsistent

Refactoring should preserve behavior while improving maintainability.

---

# 31. Security Practices

Never:

- hardcode secrets
- commit API keys
- expose sensitive logs
- bypass permission checks

Security is everyone's responsibility.

---

# 32. Accessibility

UI code must support:

- scalable text
- 48dp minimum touch targets
- proper semantic labels
- sufficient color contrast
- keyboard accessibility where applicable

Accessibility is part of quality, not an optional enhancement.

---

# 33. Continuous Improvement

Coding standards evolve with the project.

Any proposed change should:

- improve consistency
- reduce complexity
- strengthen maintainability
- preserve architectural integrity

Changes must be reviewed before adoption.

---

# 34. Definition of Done

A feature is considered complete only when:

- functionality is implemented
- tests pass
- documentation is updated
- code review is approved
- accessibility requirements are met
- performance impact is acceptable
- security requirements are satisfied

---

# 35. Vision

The quality of FilePilot is determined not only by what users see, but by the code that supports every interaction.

Clean code enables faster development, easier maintenance, fewer defects, and greater confidence in every release.

Every line of code should contribute to a product that is dependable, understandable, and built to last.