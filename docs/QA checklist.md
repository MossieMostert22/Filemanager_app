# FilePilot — Design Freeze QA Checklist

**Version:** 2.0 Final Proposal  
**Date:** 31 July 2026  
**Purpose:** Ensure objective issues are resolved before design freeze.

---

## 🎨 Accessibility Verification
- [ ] Run WCAG contrast checks for **Amber (#FFB703)** on light backgrounds.  
- [ ] Verify **Teal (#219EBC)** readability on Charcoal (#1A1D23) in dark mode.  
- [ ] Confirm minimum contrast ratio of **4.5:1** for body text and interactive elements.  
- [ ] Test text scaling at 200% to ensure readability.  

---

## 📱 Icon Rendering QA
- [ ] Test **Pilot Beacon icon** at 48px, 96px, and 192px on Android launchers.  
- [ ] Verify anti‑aliasing and pixel grid alignment.  
- [ ] Ensure arcs do not resemble Wi‑Fi/radar signals.  
- [ ] Confirm recognizability without text labels.  
- [ ] Check monochrome variations for accessibility in high‑contrast mode.  

---

## 📐 Android UX Consistency
- [ ] Validate **Smart Home default** against Android UX expectations.  
- [ ] Confirm onboarding offers choice: Smart Home / Files First / Remember Last Location.  
- [ ] Ensure OCR and PDF are discoverable from Quick Actions.  
- [ ] Verify navigation model (`Home, Files, Search, More`) aligns with Material 3 guidelines.  

---

## 🛒 Play Store Conversion
- [ ] Refine tagline to **“The file manager that reads your screenshots.”**  
- [ ] Ensure Screenshot 2 (Smart File Management) highlights differentiators, not generic file lists.  
- [ ] Confirm screenshots follow storytelling sequence:  
  1. OCR Transformation  
  2. Smart File Management  
  3. PDF Workflow  
  4. Privacy & Offline Processing  
  5. Premium Productivity  
- [ ] Test feature graphic for clarity and differentiation.  

---

## ✍️ Typography Consistency
- [ ] Clarify whether **Poppins** is purely functional or part of brand identity.  
- [ ] Ensure hierarchy (Display, Heading, Body, Supporting, Caption) is consistent across light/dark modes.  
- [ ] Verify readability at small sizes (caption text).  

---

## 💎 Premium Positioning
- [ ] Confirm free tier screenshots look complete and not crippled.  
- [ ] Use Amber sparingly for premium indicators.  
- [ ] Test premium cues (badges, highlights) for subtlety and clarity.  

---

## 🛠 Implementation Readiness
- [ ] Document design tokens (colour, typography, spacing, shape, elevation).  
- [ ] Verify centralized implementation of tokens — no arbitrary widget colours.  
- [ ] Confirm animation principles (fast, smooth, purposeful).  
- [ ] Validate error states are actionable and human‑readable.  
- [ ] Ensure empty states guide users toward next actions.  

---

## 📌 Final QA Gate
- [ ] Accessibility verified.  
- [ ] Icon rendering confirmed at small sizes.  
- [ ] Play Store messaging refined.  
- [ ] Typography clarified.  
- [ ] Premium positioning balanced.  
- [ ] Design tokens documented.  

**Status after completion:**  
✅ APPROVED — READY FOR DESIGN FREEZE
