# FilePilot — Search Engine Architecture

**Document Version:** 1.0  
**Last Updated:** August 2026

---

# Purpose

This document defines FilePilot's intelligent search architecture.

Search is the core differentiator of FilePilot.

Unlike traditional file managers that search only filenames and folders, FilePilot builds an intelligent local index capable of searching:

- File names
- OCR text
- PDF content
- Metadata
- User tags
- Collections
- Bookmarks
- Recent activity

All search operations execute entirely on-device.

No search queries or indexed content are transmitted externally.

---

# Design Philosophy

Search must be:

- Fast
- Intelligent
- Offline
- Predictable
- Privacy First

The user should never need to remember where a document was stored.

Instead, they should remember what it contains.

---

# Search Architecture

```
User Query

        │

        ▼

Query Parser

        │

        ▼

Search Service

        │

 ┌──────┼────────┐
 │      │        │
 ▼      ▼        ▼

Filename Index

OCR Index

Metadata Index

        │

        ▼

Ranking Engine

        │

        ▼

Result Formatter

        │

        ▼

Search Results
```

---

# Indexed Content

FilePilot indexes the following content.

---

## File Names

Examples

```
Invoice.pdf

Holiday Photo.jpg

Passport Scan.png

Shopping List.pdf
```

---

## OCR Text

Example

Image contains

```
Meeting with John
Friday 10 AM
Cape Town Office
```

Searching

```
John

Friday

Cape Town
```

returns the screenshot instantly.

---

## PDF Content

All OCR-generated PDFs become searchable.

Future versions may optionally index imported PDFs.

---

## Metadata

Examples

Creation date

Modified date

File size

Resolution

Document type

Folder

File extension

Language

---

## User Metadata

Examples

Tags

Favorites

Collections

Bookmarks

Pinned files

---

## Search History

Recent searches are stored locally.

Purpose

Improve user productivity.

No analytics are collected.

---

# Search Pipeline

Every search follows the same pipeline.

```
Input

↓

Normalize

↓

Tokenize

↓

Remove unnecessary whitespace

↓

Apply ranking

↓

Merge results

↓

Return results
```

---

# Query Normalization

Normalization improves consistency.

Examples

```
Invoice

invoice

INVOICE
```

become

```
invoice
```

Whitespace is ignored.

Special characters are normalized where appropriate.

---

# Tokenization

Queries are split into searchable tokens.

Example

```
South Africa Passport
```

becomes

```
south

africa

passport
```

Each token is searched independently.

---

# Search Types

Version 1 supports

---

## Exact Search

```
Invoice.pdf
```

Matches

Invoice.pdf

---

## Partial Search

Searching

```
pass
```

matches

Passport.pdf

Passport Scan.png

---

## Phrase Search

Searching

```
bank statement
```

prioritizes exact phrase matches.

---

## Multi-word Search

Searching

```
john invoice
```

returns documents containing both terms.

---

## Metadata Search

Searching

```
PDF
```

may include

Document Type = PDF

---

# Ranking Engine

Results are ranked using weighted scoring.

Priority

1. Exact filename

2. Exact OCR text

3. Partial filename

4. Partial OCR

5. Tags

6. Metadata

7. Older matches

The best result should appear first.

---

# Search Filters

Version 1 supports optional filters.

File Type

- PDF
- Image
- Screenshot
- Document

Date

- Today
- Yesterday
- This Week
- This Month

Size

Small

Medium

Large

Location

Downloads

Pictures

Documents

Favorites

Collections

---

# Intelligent Suggestions

While typing

FilePilot may suggest

Recent searches

Frequently opened files

Matching filenames

Matching OCR words

Suggestions remain local.

---

# Empty State Behaviour

No results should produce helpful actions.

Example

```
No matching files found.

Try

• Different spelling

• Broader search

• OCR more screenshots
```

---

# OCR Search

OCR content becomes searchable immediately after indexing.

Workflow

```
Screenshot

↓

OCR

↓

Extract text

↓

Store text

↓

Update search index

↓

Available for search
```

No manual refresh is required.

---

# Index Updates

Indexes update automatically when

A file is added

A file is deleted

OCR completes

Metadata changes

User adds tags

Collections change

Favorites change

---

# Background Indexing

Large indexing jobs run in the background.

Examples

First application launch

Importing folders

Large screenshot libraries

Device upgrades

The UI remains responsive.

---

# Fuzzy Search

Version 1 includes lightweight typo tolerance.

Examples

Searching

```
invioce
```

returns

Invoice.pdf

Searching

```
pasport
```

returns

Passport.png

Future versions may introduce advanced fuzzy ranking.

---

# Language Support

OCR language selection influences indexing.

Version 1

Automatic language detection where supported.

Future versions

Multiple simultaneous languages

User language packs

Regional dictionaries

---

# Search Performance Goals

Search bar response

Instant

Query execution

<100 ms

Result rendering

<50 ms

Suggestion generation

<30 ms

Large OCR index lookup

<150 ms

Cold search startup

<300 ms

---

# Search Result Display

Each result displays

File icon

Filename

Highlighted match

Location

Modified date

Quick Actions

Examples

Open

Share

OCR Again

Create PDF

Favorite

---

# Highlighting

Matching text should be highlighted.

Example

Searching

```
passport
```

Displays

```
My **Passport** Scan
```

Highlighting improves usability.

---

# Search History

Recent searches

Stored locally

Maximum

50 entries

User may

Delete individual searches

Clear all history

Disable search history

---

# Privacy

Search history never leaves the device.

No telemetry

No cloud indexing

No search analytics

No advertising identifiers

---

# Future Search Features

Possible future enhancements

Semantic search

Natural language search

AI-assisted document discovery

Voice search

Image similarity search

Object recognition

Handwriting recognition

Vector search

Smart document clustering

Saved searches

Boolean search operators

---

# Engineering Principles

The search engine must always remain

✓ Offline

✓ Fast

✓ Accurate

✓ Predictable

✓ Private

✓ Incrementally scalable

✓ Memory efficient

✓ Battery conscious

If a search feature compromises user privacy or significantly impacts device performance, it must remain optional or be deferred to a future release.

---

# Relationship to Other Components

The Search Engine integrates with

- OCR Engine
- Storage Services
- Metadata Database
- Smart Collections
- Favorites
- Recent Files
- PDF Generator

Each component contributes searchable information while maintaining a single unified search experience.

---

# Success Criteria

The FilePilot Search Engine is considered successful when users can:

- Find files by name within milliseconds.
- Locate screenshots using text contained inside the image.
- Search PDFs generated by FilePilot.
- Filter results quickly using intuitive criteria.
- Receive relevant results without learning advanced search syntax.
- Trust that every search remains completely private and offline.

---

**End of Document**