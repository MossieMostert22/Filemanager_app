# FilePilot Dependency Strategy
**Version:** 1.0  
**Status:** Approved for Development  
**Owner:** FilePilot Project  
**Last Updated:** August 2026

---

# 1. Purpose

This document defines FilePilot's dependency management strategy.

Its objectives are to:

- maintain a lightweight application
- minimize technical debt
- maximize long-term maintainability
- ensure security
- preserve offline functionality
- reduce upgrade risk

Every dependency introduced into the project becomes part of the application's architecture and must therefore satisfy strict evaluation criteria before adoption.

---

# 2. Philosophy

FilePilot follows a **Minimal Dependency Philosophy**.

Every package must justify its existence.

A dependency is accepted only when it:

- solves a significant engineering problem
- is actively maintained
- improves stability
- reduces implementation complexity
- aligns with the project's long-term vision

Dependencies are never added for convenience alone.

---

# 3. Core Principles

Every dependency must satisfy the following principles.

### Simplicity

Prefer fewer packages over many small packages.

---

### Stability

Prefer mature, production-tested libraries.

---

### Offline First

Dependencies must not introduce unnecessary online services.

---

### Security

Dependencies must have a strong security history.

---

### Performance

Packages must not introduce unnecessary startup cost or memory usage.

---

### Long-Term Support

Dependencies should demonstrate consistent maintenance over multiple years.

---

# 4. Dependency Approval Criteria

Before adding any package, evaluate:

✓ Active maintenance

✓ Regular releases

✓ Community adoption

✓ Good documentation

✓ Null Safety support

✓ Flutter compatibility

✓ Android compatibility

✓ Open-source license

✓ No unnecessary permissions

✓ No telemetry by default

If any requirement fails, the dependency requires additional review.

---

# 5. Preferred License Types

Accepted licenses include:

- MIT
- BSD
- Apache 2.0

Licenses requiring source disclosure or incompatible with commercial distribution require legal review before adoption.

---

# 6. Dependency Categories

Dependencies are grouped into functional categories.

---

## Core Flutter

Examples:

- flutter
- flutter_test
- flutter_lints

These form the foundation of the application.

---

## State Management

Preferred:

Riverpod

Reasons:

- compile-time safety
- testability
- scalability
- minimal boilerplate

No additional state management libraries should be introduced without architectural review.

---

## Navigation

Preferred:

GoRouter

Reasons:

- official Flutter recommendation
- deep-link support
- declarative navigation
- maintainable routing

---

## Local Database

Preferred:

Isar

Reasons:

- extremely fast
- offline-first
- native Flutter support
- excellent search performance
- zero SQL management

---

## OCR

Preferred:

Google ML Kit (On-Device Text Recognition)

Requirements:

- completely offline
- local processing only
- no cloud APIs
- Android optimized

Cloud OCR providers are explicitly excluded from Version 1.

---

## PDF Generation

Preferred characteristics:

- local generation
- streaming support
- multi-page capability
- image optimization
- low memory consumption

---

## File Access

Use packages compatible with:

- Scoped Storage
- Storage Access Framework
- MediaStore

Deprecated storage methods are prohibited.

---

## Image Processing

Requirements:

- efficient decoding
- image resizing
- compression
- rotation handling
- memory efficiency

---

## Search

Search functionality should rely primarily on Isar indexing.

Avoid introducing dedicated search engine dependencies unless future requirements justify them.

---

## Logging

Preferred approach:

Lightweight structured logging.

Verbose logging must be disabled in release builds.

---

## Testing

Testing packages may include:

- unit testing
- widget testing
- integration testing
- mocking

Testing dependencies remain isolated from production code.

---

# 7. Packages to Avoid

Avoid packages that:

- duplicate Flutter functionality
- require cloud services
- are poorly maintained
- have frequent breaking changes
- introduce excessive permissions
- significantly increase APK size

---

# 8. Version Strategy

Always prefer:

Stable releases.

Avoid:

- beta
- alpha
- experimental
- release candidates

unless required for critical platform support.

---

# 9. Version Pinning

Use semantic version constraints responsibly.

Guidelines:

- pin major versions
- allow compatible patch updates
- avoid unrestricted version ranges

Example:

```
^3.2.1
```

Avoid:

```
any
```

or completely unbounded versions.

---

# 10. Dependency Updates

Update cadence:

- Monthly dependency review
- Quarterly maintenance update
- Immediate security patches

Major version upgrades require regression testing before adoption.

---

# 11. Security Review

Every new dependency must undergo:

- license verification
- maintenance review
- vulnerability scan
- permission audit
- repository activity review

Dependencies with unresolved critical vulnerabilities must not be adopted.

---

# 12. Performance Review

Before approval, evaluate:

- APK size increase
- startup impact
- memory usage
- CPU utilization
- battery impact

If measurable degradation occurs, the dependency should be rejected or replaced.

---

# 13. Dependency Lifecycle

Every dependency passes through:

1. Evaluation
2. Prototype
3. Security Review
4. Architecture Approval
5. Production Adoption
6. Maintenance
7. Replacement or Removal

No package remains permanently exempt from review.

---

# 14. Removal Policy

Dependencies should be removed when they become:

- abandoned
- insecure
- redundant
- incompatible
- replaced by Flutter core functionality

Reducing dependencies is considered a positive architectural improvement.

---

# 15. Dependency Documentation

Every adopted dependency should be documented with:

- purpose
- version
- license
- maintainer
- rationale for adoption
- alternatives considered

This documentation should be maintained throughout the project lifecycle.

---

# 16. Approved Technology Stack (Version 1)

| Category | Preferred Technology |
|----------|----------------------|
| Framework | Flutter |
| Language | Dart |
| State Management | Riverpod |
| Navigation | GoRouter |
| Local Database | Isar |
| OCR | Google ML Kit (Offline) |
| PDF Generation | Dart PDF Library |
| File Access | Scoped Storage APIs |
| Image Processing | Flutter-compatible image libraries |
| Logging | Lightweight structured logging |
| Testing | Flutter Test Framework |

---

# 17. Future Evaluation Process

Future dependencies must answer the following questions:

1. Why is this package needed?
2. Can Flutter already solve this?
3. Can existing project code solve it?
4. Is the package actively maintained?
5. Does it preserve offline-first principles?
6. Does it increase application size significantly?
7. Does it improve the user experience?
8. Will it still be appropriate in three years?

If any answer raises concern, the dependency should be reconsidered.

---

# 18. Architectural Principle

Dependencies are tools, not architecture.

The architecture must remain independent of individual packages wherever possible.

This allows dependencies to be replaced without requiring major redesign.

Business logic must never become tightly coupled to third-party libraries.

---

# 19. Dependency Governance

The Project Architect is responsible for approving new production dependencies.

No production package may be introduced without:

- documented justification
- architectural review
- security review
- performance assessment

This ensures consistency and protects the long-term health of the codebase.

---

# 20. Vision

A dependable application begins with dependable foundations.

FilePilot's dependency strategy favors stability over novelty, quality over quantity, and long-term maintainability over short-term convenience.

Every dependency should strengthen the application—not complicate it.

The best dependency is often the one that never had to be added.