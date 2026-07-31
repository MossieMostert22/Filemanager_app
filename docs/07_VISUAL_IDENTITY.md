# 07_VISUAL_IDENTITY.md

> **Project:** FilePilot
> **Document Version:** 1.0
> **Status:** Design Frozen (Version 1)
> **Last Updated:** 31 July 2026

---

# Visual Identity

## Purpose

This document defines the official FilePilot visual identity.

It establishes the brand language, colour system, iconography, typography, imagery, and usage rules that ensure a consistent experience across every platform.

This document is the definitive visual reference for:

* Application UI
* Website
* Google Play Store
* Marketing material
* Documentation
* Presentations
* Social media
* Future product extensions

No visual changes should be introduced without updating this document.

---

# Brand Philosophy

FilePilot is built around one simple idea:

> **Help people understand and manage their files with confidence.**

The visual identity reflects that philosophy.

Every visual element should communicate:

* Simplicity
* Intelligence
* Trust
* Professionalism
* Clarity
* Modern Android design
* Offline-first privacy

The interface should feel approachable for new users while remaining powerful enough for advanced workflows.

---

# Brand Personality

FilePilot is:

* Intelligent
* Calm
* Helpful
* Efficient
* Trustworthy
* Privacy focused
* Modern
* Confident
* Human

FilePilot is **not**:

* Loud
* Playful
* Corporate
* Futuristic for the sake of appearance
* Overly technical
* Cluttered

The experience should inspire confidence rather than complexity.

---

# Brand Values

Every design decision should reinforce:

1. Privacy First
2. Offline First
3. User Control
4. Simplicity
5. Reliability
6. Speed
7. Accessibility
8. Continuous Improvement

---

# Logo

## Primary Logo

The official FilePilot logo consists of:

* Pilot Beacon icon
* FilePilot wordmark

The icon may appear independently where space is limited.

---

## Logo Variants

Approved versions:

* Full colour
* White
* Black
* Outline (secondary usage only)

The full-colour version is the primary brand mark.

---

## Minimum Size

Digital:

* Icon only: 24 px minimum
* Full logo: 120 px minimum width

Print:

* Icon: 8 mm minimum
* Full logo: 30 mm minimum width

---

## Clear Space

Maintain clear space equal to the height of the beacon center around all sides of the logo.

No text or graphics may enter this area.

---

# Pilot Beacon Icon

## Concept

The Pilot Beacon combines:

* Navigation
* Direction
* Guidance
* Documents
* Intelligence

It represents helping users find and understand their information.

It is **not** intended to represent:

* Wi-Fi
* Radar
* Networking
* Aviation hardware

---

## Design Principles

The icon must remain:

* Simple
* Recognisable
* Symmetrical
* Balanced
* Readable at small sizes

No decorative details may be added.

---

## Small Size Optimization

Icons below **48 px** require verification.

The beacon arcs may use a dedicated small-size rendering with:

* Slightly thicker strokes
* Pixel-grid alignment
* Simplified anti-aliasing

This optimisation must preserve the overall shape while improving readability.

---

# Colour Philosophy

Each colour has a functional purpose.

Colour should communicate meaning—not decoration.

---

# Primary Colours

## FilePilot Amber

**Primary Brand Colour**

Hex:

`#FFB703`

Purpose:

* Brand identity
* Primary highlights
* Active navigation
* Brand accents
* Marketing emphasis

### Accessibility Rule

Pure Amber **must not** be used for small text on light backgrounds.

For light theme interactive text or icons, use the approved accessible token:

**Amber Dark**

`#B45309`

or

`#D97706`

These variants satisfy accessibility requirements while preserving brand identity.

---

## Midnight Charcoal

Hex:

`#121417`

Purpose:

* Primary dark background
* Premium appearance
* Navigation surfaces
* Brand foundation

---

## Charcoal Surface

Hex:

`#1F2328`

Purpose:

* Cards
* Elevated surfaces
* Containers
* Secondary backgrounds

---

## Secondary Slate

Hex:

`#343B46`

Purpose:

* Dividers
* Borders
* Secondary containers
* Neutral controls

---

## FilePilot Teal

Hex:

`#219EBC`

Purpose:

* OCR
* Search intelligence
* Processing
* Analysis
* Smart features

### Functional Rule

Teal represents **processing and intelligence**, not navigation.

Examples:

* OCR progress
* Search indicators
* Processing status
* AI-inspired functionality
* Scan actions

Primary navigation and general call-to-action elements remain Amber.

### Accessibility Rule

Teal used as text must pass WCAG AA (4.5:1 minimum).

If contrast is insufficient on dark backgrounds, Teal should be limited to:

* Icons
* Progress indicators
* Badges
* Filled components with light text

---

# Supporting Colours

## Success

`#22C55E`

## Warning

`#F59E0B`

## Error

`#EF4444`

## Information

`#3B82F6`

These colours communicate system status only.

They must never replace brand colours.

---

# Light Theme

Background

`#F8F9FA`

Cards

`#FFFFFF`

Text Primary

`#111827`

Text Secondary

`#5B6470`

Dividers

`#E5E7EB`

---

# Dark Theme

Background

`#121417`

Surface

`#1F2328`

Text Primary

`#F8FAFC`

Text Secondary

`#CBD5E1`

Borders

`#343B46`

---

# Colour Usage Principles

Amber communicates:

* Navigation
* Confirmation
* Brand

Teal communicates:

* Processing
* OCR
* Search
* Intelligence

Green communicates:

* Success

Red communicates:

* Errors

Orange communicates:

* Warnings

Blue communicates:

* Neutral information

Colours should never compete for attention.

---

# Typography

## Philosophy

Typography exists to support readability and clarity.

It is intentionally neutral and functional rather than a primary brand differentiator.

The FilePilot identity is primarily expressed through its iconography, colour system, and interaction design.

---

## Primary Typeface

**Poppins**

Weights:

* Bold
* SemiBold
* Medium
* Regular

---

## Android Implementation

To preserve offline functionality and minimise application size:

* Bundle only required font weights.
* Where appropriate, allow graceful fallback to the Android system sans-serif font if a font asset is unavailable.
* Do not rely on runtime font downloads.

---

## Text Hierarchy

Display

32–40 sp

Heading

24–28 sp

Section

18–22 sp

Body

16 sp

Supporting

14 sp

Caption

12 sp

---

# Iconography

Icons follow Material Design principles.

Requirements:

* Rounded geometry
* Simple forms
* Consistent stroke width
* Minimal detail

Avoid decorative icons.

---

# Illustration Style

Illustrations should be:

* Flat
* Clean
* Friendly
* Modern

Avoid:

* Heavy gradients
* Skeuomorphism
* Complex textures
* Cartoon styling

---

# Photography

Photography should emphasise:

* Productivity
* Workspaces
* Documents
* Mobile usage
* Real people
* Authentic environments

Avoid generic stock imagery where possible.

---

# Imagery Tone

Preferred:

* Bright
* Natural
* Clean
* Calm

Avoid:

* Dark dramatic imagery
* Over-saturated colours
* Artificial visual effects

---

# Motion

Animations should communicate state changes rather than decoration.

Maximum duration:

* Small interactions: 150–200 ms
* Screen transitions: 250–300 ms

Animations should remain subtle and performant.

Respect reduced motion preferences where supported.

---

# Accessibility

Every visual element must comply with accessibility requirements.

Minimum standards:

* WCAG 2.1 AA colour contrast
* Minimum 48 dp touch targets
* Dynamic font scaling support
* Screen reader compatibility
* Clear focus indicators
* Colour never used as the only indicator of meaning

Accessibility validation is mandatory before release.

---

# Google Play Store Identity

The Play Store presentation must consistently communicate the complete FilePilot value proposition.

## App Title

**FilePilot**

---

## Short Description

**Smart File Manager, OCR & PDF — Organize files, extract text from screenshots, and create PDFs offline.**

---

## Core Messages

* Intelligent file management
* Screenshot OCR
* Offline PDF creation
* Privacy by design
* Fast local search

---

## Screenshot Order

1. Screenshot → OCR → Text → PDF
2. Smart Home
3. Intelligent File Search
4. Offline Privacy
5. Premium Features

The first screenshot must always highlight the application's primary differentiator rather than a generic file list.

---

# Premium Visual Identity

Premium should feel aspirational without diminishing the free experience.

Visual indicators:

* Amber Premium badge
* Subtle highlight treatment
* Premium label

Avoid excessive crowns, gradients, or visual clutter.

The free version must always appear complete and capable.

---

# Brand Voice

FilePilot communicates with users using clear, respectful language.

Preferred tone:

* Helpful
* Professional
* Friendly
* Concise

Avoid:

* Technical jargon
* Marketing exaggeration
* Fear-based messaging
* Humour that obscures clarity

---

# Visual Consistency Rules

Always:

* Use approved colours
* Respect spacing
* Maintain typography hierarchy
* Keep layouts uncluttered
* Preserve logo clear space
* Use official icon assets

Never:

* Stretch the logo
* Recolour the icon arbitrarily
* Replace Amber with another primary accent
* Introduce additional brand colours without approval
* Overload interfaces with decorative graphics

---

# Version 1 Brand Scope

Included:

* Pilot Beacon identity
* Amber + Charcoal colour system
* Material-inspired iconography
* Poppins typography
* Smart Home visual language
* Google Play branding
* Premium visual treatment

Future evolution may include:

* Desktop adaptations
* Tablet-specific layouts
* Wear OS assets
* Marketing illustration library
* Motion identity expansion

All future additions must remain consistent with the principles defined in this document.

---

# Conclusion

The FilePilot visual identity establishes a distinctive, professional, and accessible brand that supports the product's long-term vision.

Its strength comes from disciplined consistency rather than visual excess. The Pilot Beacon icon, Amber-led colour system, offline-first philosophy, and restrained interface create a recognisable identity that differentiates FilePilot in a crowded marketplace while remaining practical for everyday use.

This document is the definitive reference for all visual implementation across the FilePilot ecosystem.
