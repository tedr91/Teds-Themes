# DESIGN.md — Fluent 2 Design System Overview

## Introduction

This document describes the design philosophy, principles, and structural guidelines for this project, aligned with **Microsoft Fluent 2** — a cross-platform design system built for clarity, accessibility, and expressive interfaces.

---

## Design Philosophy

Fluent 2 is grounded in four core principles:

| Principle | Description |
|-----------|-------------|
| **Engaging** | Interfaces feel alive, purposeful, and emotionally resonant |
| **Flexible** | Scales across screen sizes, densities, and platforms |
| **Familiar** | Patterns users already know, applied consistently |
| **Inclusive** | Accessible by default; designed for everyone |

---

## Visual Language

### Geometry & Shape
- Use **rounded corners** (`border-radius: 4px` for small elements, `8px` for cards, `12px` for panels).
- Avoid sharp rectangular containers except for data-dense tables.

### Depth & Elevation
Fluent 2 uses **layered surfaces** to communicate hierarchy:

| Layer | Usage | Shadow |
|-------|-------|--------|
| Base | Page background | None |
| Card | Content grouping | Elevation 2 |
| Overlay | Dialogs, tooltips | Elevation 8 |
| Temporary | Menus, drawers | Elevation 16 |

### Motion & Animation
- **Duration**: Use `100ms` for micro-interactions, `200ms` for transitions, `300ms` for page-level changes.
- **Easing**: `cubic-bezier(0.33, 0, 0.67, 1)` for entrances; `cubic-bezier(0.33, 0, 0, 1)` for exits.
- Respect `prefers-reduced-motion` — fall back to instant transitions.

---

## Layout System

### Grid
- **Base unit**: `4px` grid. All spacing values are multiples of 4.
- **12-column grid** for large screens; 4-column for mobile.
- Gutters: `16px` (mobile), `24px` (tablet), `32px` (desktop).

### Breakpoints

| Breakpoint | Min Width | Layout |
|------------|-----------|--------|
| xs | 0px | Single column, stacked |
| sm | 480px | 4-column |
| md | 768px | 8-column |
| lg | 1024px | 12-column |
| xl | 1366px+ | 12-column, max-width container |

---

## Accessibility

- **WCAG 2.1 AA** compliance is the minimum standard; target AAA where feasible.
- Minimum touch target: `44×44px`.
- Focus indicators must be clearly visible (3:1 contrast ratio minimum against the adjacent background).
- All interactive elements must be keyboard-navigable.
- Use semantic HTML elements; add ARIA roles only when semantics are insufficient.

---

## Iconography

- Use **Fluent System Icons** (the official library).
- Sizes: `16px`, `20px`, `24px`, `28px`, `32px`, `48px`.
- Filled variants for selected/active states; Regular for default.

---

## References

- [Fluent 2 Design](https://fluent2.microsoft.design/)
- [Fluent System Icons](https://github.com/microsoft/fluentui-system-icons)
- [WCAG 2.1 Guidelines](https://www.w3.org/TR/WCAG21/)
