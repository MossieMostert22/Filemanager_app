# FilePilot Design System
## Version 1.0 (Design Freeze)

**Project:** FilePilot  
**Document:** 05_DESIGN_SYSTEM.md  
**Status:** APPROVED – DESIGN FROZEN  
**Last Updated:** July 2026

---

# Purpose

This document defines the official FilePilot design language.

The goal of the design system is consistency.

Every screen, component, interaction and future feature should feel like it belongs to the same product.

No UI should be created without following this document.

---

# Design Philosophy

FilePilot is built around one principle:

> **Simple first. Powerful underneath.**

The interface should never feel overwhelming.

Users should immediately understand what they can do.

Advanced functionality should reveal itself naturally without cluttering the interface.

---

# Design Principles

## 1. Clarity over Decoration

Every visual element must have a purpose.

Avoid unnecessary gradients, shadows, icons or animations.

Whitespace is preferred over visual noise.

---

## 2. Function before Beauty

Beautiful interfaces are useless if users cannot complete tasks.

The design should always optimize for:

- discoverability
- readability
- accessibility
- speed

---

## 3. Confidence

Users should always know:

• where they are

• what they are doing

• what will happen next

---

## 4. Consistency

The same action should always look identical.

The same colors should always communicate the same meaning.

The same components should behave identically everywhere.

---

## Brand Identity

### Personality

FilePilot is:

- Intelligent
- Calm
- Professional
- Private
- Helpful
- Efficient
- Trustworthy

It is never:

- childish
- flashy
- noisy
- aggressive
- gimmicky

---

# Brand Colors

## Primary Brand

FilePilot Amber

HEX

```
#FFB703
```

Purpose

- Brand identity
- Active navigation
- Highlight state
- Important buttons
- Primary actions (non-processing)
- Selection

---

## OCR / Intelligence

FilePilot Teal

HEX

```
#219EBC
```

Purpose

- OCR
- AI
- Search intelligence
- Processing indicators
- Recognition
- Technical operations

---

## Dark Background

Midnight Charcoal

HEX

```
#121417
```

Purpose

Primary dark theme background.

---

## Elevated Surface

HEX

```
#1F2328
```

Purpose

Cards

Dialogs

Sheets

Floating surfaces

---

## Light Background

HEX

```
#F8F9FA
```

---

## Card Background

HEX

```
#FFFFFF
```

---

# Accessibility Tokens

Accessibility is mandatory.

The following rules apply throughout the application.

### Minimum Tap Target

48dp × 48dp

(Material 3)

---

### Touch Spacing

Minimum 8dp

---

### Contrast

Minimum WCAG AA

Text:

4.5 : 1

Large Text:

3 : 1

Interactive Components:

3 : 1

---

### Light Mode Accent

Because pure Amber (#FFB703) does not provide sufficient contrast on white surfaces for small text, use:

```
Amber Dark
#D97706
```

for:

- text
- icons
- outlined controls
- thin strokes
- navigation indicators

Reserve pure Amber (#FFB703) for:

- filled buttons
- dark surfaces
- badges
- highlights
- illustrations

---

### Teal Usage

Teal should primarily be used for:

- OCR indicators
- processing states
- AI features
- search highlights
- progress indicators

Avoid using Teal as small body text unless verified to meet WCAG AA contrast requirements.

---

# Color Meaning

| Color | Meaning |
|---------|-----------|
| Amber | Navigation, Primary Action |
| Teal | OCR, Processing, Intelligence |
| Green | Success |
| Orange | Warning |
| Red | Error |
| Blue | External Links |
| Grey | Disabled |

---

# Typography

Primary Typeface

Poppins

Reason

Modern

Friendly

Readable

Open Source

Well supported

---

## Android Considerations

Poppins will be bundled as local assets to preserve the application's offline-first philosophy.

If font loading fails, gracefully fall back to the Android system sans-serif font.

---

## Font Scale

Display

36

Bold

---

Headline

28

SemiBold

---

Title

22

SemiBold

---

Section

18

Medium

---

Body

16

Regular

---

Supporting

14

Regular

---

Caption

12

Regular

---

Button

15

SemiBold

---

# Spacing System

The entire application uses an 8dp spacing grid.

## Standard Values

```
4
8
16
24
32
40
48
64
```

No arbitrary spacing values should be introduced.

---

# Border Radius

Small

8dp

Medium

12dp

Large

16dp

Extra Large

24dp

Floating Cards

28dp

Buttons

16dp

FAB

Circular

---

# Elevation

Flat UI is preferred.

Only three elevations exist.

Surface

0

Card

2

Modal

6

Do not introduce custom shadow depths.

---

# Iconography

Icons

Material Symbols Rounded

Style

Rounded

Filled where appropriate

Consistent stroke width

No mixed icon styles.

---

# Illustration Style

Illustrations should be:

Minimal

Flat

Modern

Friendly

High contrast

Avoid:

3D

Photo realism

Clipart

Overly playful cartoons

---

# Component Rules

## Buttons

Primary

Filled Amber

Secondary

Outlined

Ghost

Text only

Danger

Red

Disabled

Grey

---

## Cards

Rounded

16dp

Light elevation

Clear spacing

Never overcrowded.

---

## FAB Rules

Floating Action Button should only perform a creation or initiation task.

Examples:

Start OCR

Scan

Create PDF

New Folder

If the FAB launches OCR or scanning, it should use the Teal token rather than Amber to reinforce functional consistency.

---

## Navigation

Bottom Navigation

Maximum

4 items

Home

Files

Search

More

Search in Home provides immediate search access.

The dedicated Search tab exists for advanced search capabilities such as:

- filters
- OCR text search
- saved searches
- recent searches

---

# Motion

Animations should be subtle.

Maximum

250ms

Use easing.

Avoid:

bounce

elastic

overshoot

long fades

Animations should communicate state changes.

Never decorate.

---

# Haptics

Use only for:

Successful OCR completion

Successful scan

Long press

Delete confirmation

Selection

Never vibrate for normal navigation.

---

# Sound

No interface sounds by default.

Optional future accessibility setting.

---

# Empty States

Every empty state should include:

Illustration

Title

Description

Primary Action

No empty screen should appear unfinished.

---

# Loading States

Always use:

Skeletons

Progress indicators

Background tasks

Never freeze the interface.

---

# Error States

Errors should always explain:

What happened

Why

How to recover

Avoid generic messages such as:

"Unknown Error"

---

# Offline States

Offline is expected.

Never present offline as an error.

Instead communicate:

"Everything continues to work locally."

---

# Premium Indicators

Premium should be visible but never intrusive.

Use:

small amber badge

subtle label

optional crown icon

Avoid aggressive upgrade prompts.

---

# Design Freeze Rules

After Design Freeze:

No component may be redesigned without updating this document.

All new screens must reuse existing components before introducing new ones.

Design consistency takes precedence over individual preferences.

---

# Version 1 Scope

The design system supports:

✓ Smart Home

✓ File Browser

✓ OCR

✓ Screenshot Recognition

✓ PDF Creation

✓ Search

✓ Settings

Everything else should extend this system rather than replace it.

---

# Future Evolution

This design system is intentionally conservative.

Future improvements should be driven by:

User research

Usage analytics

Accessibility improvements

Platform evolution

—not by changing trends.

Consistency builds trust.

---

# Final Statement

The FilePilot Design System exists to ensure every interaction feels intentional, professional, and familiar.

It is not merely a collection of colors and components.

It is the visual language of the product.

Simple first.

Powerful underneath.