# FilePilot — Data Storage Architecture

**Document Version:** 1.0  
**Last Updated:** August 2026

---

# Purpose

This document defines how FilePilot stores, manages, protects and retrieves data throughout the application.

The storage architecture is designed around four principles:

- Offline First
- Privacy First
- High Performance
- Predictable Behaviour

No user files are uploaded to external servers.

All processing occurs locally unless the user explicitly enables future cloud integrations.

---

# Storage Philosophy

FilePilot treats storage as an application service rather than an implementation detail.

The storage layer must provide:

- Fast retrieval
- Safe persistence
- Efficient caching
- Automatic cleanup
- Low memory usage
- Strong data integrity

The application never duplicates files unnecessarily.

Instead, FilePilot stores references whenever possible.

---

# Storage Layers

FilePilot separates storage into logical layers.

```
Presentation Layer

↓

Business Logic

↓

Storage Services

↓

Android Storage APIs

↓

Device Storage
```

---

# Storage Categories

FilePilot stores four categories of information.

---

## 1. User Files

Examples

- PDFs
- Images
- Screenshots
- Downloads
- Documents

Storage

User device

Ownership

User

Persistence

Permanent

---

## 2. Application Database

Stores

- OCR index
- Search index
- User preferences
- Bookmarks
- Recent files
- Tags
- Smart collections

Storage

SQLite (Drift)

Ownership

Application

Persistence

Permanent

---

## 3. Temporary Processing

Examples

OCR processing

PDF rendering

Image compression

Thumbnail generation

Storage

Temporary Cache

Persistence

Automatic removal

---

## 4. Cached Assets

Examples

Icons

Generated thumbnails

Metadata cache

Search cache

Persistence

Disposable

Can be rebuilt.

---

# Database Technology

Version 1 uses

SQLite

through

Drift

Reasons

✓ Mature

✓ Fast

✓ Offline

✓ Reliable

✓ Excellent Flutter support

✓ Strong query language

---

# Database Structure

Major tables include

```
files

recent_files

ocr_documents

ocr_blocks

bookmarks

tags

settings

search_history

favorites

collections
```

Future versions may extend this schema without breaking compatibility.

---

# File References

The application stores

URI references

instead of copying files.

Example

```
content://media/external/images/12345
```

Advantages

No duplication

No wasted storage

Instant access

Native Android compatibility

---

# Thumbnail Storage

Generated thumbnails are cached.

Rules

Generate once

Reuse

Regenerate if missing

Automatic cleanup

Maximum cache size configurable

---

# OCR Storage

OCR text is stored separately from original files.

Each OCR record contains

Document ID

File URI

Recognized text

Language

Creation date

Version

This allows fast searching without reopening files.

---

# Search Index

Search uses an indexed database.

Indexed content

Filename

OCR text

Tags

Favorites

Collections

Metadata

The index updates automatically whenever a file changes.

---

# User Preferences

Preferences are lightweight.

Examples

Theme

Language

Home layout

Sort order

View mode

Default folders

Quick Actions

Storage

Shared Preferences

(or DataStore if adopted)

---

# Secure Storage

Sensitive values

(if introduced later)

will use

EncryptedSharedPreferences

or

Android Keystore.

Examples

Future cloud tokens

API keys

Encryption secrets

These values never reside inside SQLite.

---

# Cache Management

Caches improve performance.

Cache categories

Thumbnail Cache

OCR Cache

Image Cache

Search Cache

Temporary PDFs

Rules

Least Recently Used (LRU)

Automatic expiration

Maximum storage limits

Background cleanup

---

# Storage Limits

Version 1 defaults

Thumbnail Cache

250 MB

Temporary Files

500 MB

OCR Cache

Unlimited (text only)

Search Index

Unlimited

Users may adjust limits in Settings.

---

# File Access

Android Storage Access Framework (SAF)

is used whenever appropriate.

Benefits

Permission friendly

Future proof

Compatible with scoped storage

Play Store compliant

---

# Scoped Storage

FilePilot fully supports Android Scoped Storage.

The application only accesses

Files the user selects

Media libraries

Application storage

No unrestricted filesystem access is requested.

---

# Background Operations

Storage operations run asynchronously.

Examples

Index rebuilding

Thumbnail generation

OCR indexing

Cache cleanup

Database optimization

UI never blocks while storage tasks execute.

---

# Data Integrity

Every write operation follows

Validate

↓

Write

↓

Verify

↓

Commit

↓

Notify UI

Corrupted writes must never overwrite valid data.

---

# Backup Strategy

Version 1

Application settings

may participate in Android Auto Backup.

User files remain untouched.

Future versions may offer

Export Settings

Import Settings

Backup Profiles

---

# Migration Strategy

Database migrations follow semantic versioning.

Every migration must

Preserve user data

Be reversible where possible

Be tested independently

Be documented

No destructive migration is permitted without explicit user approval.

---

# Storage Performance Goals

Application launch

< 2 seconds

Database lookup

< 20 ms

Search query

< 100 ms

Thumbnail retrieval

< 30 ms

Preference loading

Instant

Large file indexing

Background only

---

# Failure Recovery

If storage errors occur

FilePilot will

Retry

Recover cache

Rebuild indexes

Restore database integrity

Notify the user only when required

Silent recovery is preferred.

---

# Privacy

Storage follows Privacy First principles.

The application

Never uploads files

Never scans automatically outside approved folders

Never stores personal information externally

Never transmits OCR content

All storage remains local.

---

# Future Expansion

Future storage capabilities may include

Encrypted vaults

Cloud synchronization

Version history

Cross-device synchronization

AI document embeddings

Vector search indexes

Backup archives

These additions must remain optional and preserve the Offline First philosophy.

---

# Engineering Principles

Every storage decision should satisfy

✓ Fast

✓ Safe

✓ Offline

✓ Private

✓ Reliable

✓ Scalable

✓ Maintainable

✓ Recoverable

If a storage solution sacrifices privacy or predictability for convenience, it should not be adopted.

---

**End of Document**