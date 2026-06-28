# Material Design 3 — Design Foundations

> **Version:** Material Design 3 (M3) / Material You · Updated June 2026  
> **Source:** [m3.material.io](https://m3.material.io) · Google LLC  
> **Scope:** Foundations — Color · Typography · Motion · Elevation · Shape · Iconography · Layout · Accessibility

---

## Table of Contents

1. [Overview](#overview)
2. [Color System](#color-system)
3. [Typography](#typography)
4. [Motion](#motion)
5. [Elevation](#elevation)
6. [Shape](#shape)
7. [Iconography](#iconography)
8. [Layout & Adaptive Design](#layout--adaptive-design)
9. [Accessibility](#accessibility)

---

## Overview

Material Design 3 (M3) is Google's open-source design system for building beautiful, usable products across Android, web, and beyond. Built on the principles of **Material You**, M3 emphasizes personalization, expressiveness, and accessibility through adaptive theming and user-driven customization.

M3 Expressive (2026) expands the foundation with emotion-driven design patterns, motion physics, an expanded shape library, and new expressive components — all while maintaining accessibility and cross-platform compatibility.

### Design Principles

| Principle | Description |
|---|---|
| **Personal** | Adapts to each user through dynamic color sourced from wallpaper or in-app content |
| **Accessible** | Contrast, legibility, and interaction targets meet or exceed WCAG 2.1 AA |
| **Expressive** | Uses color, shape, and motion to convey personality and brand identity |
| **Adaptive** | Scales gracefully from compact phones to foldables, tablets, and large-screen displays |
| **Systematic** | Every visual decision is tokenized, enabling consistent application across platforms |

---

## Color System

### Overview

Material Design 3 uses the **HCT (Hue, Chroma, Tone)** color model — a perceptually uniform color space derived from CAM16 and CIELAB. HCT ensures that color transformations (lightening, darkening, harmonizing) produce predictable, accessible, and visually pleasing results.

> **Dynamic Color** generates an entire scheme automatically from a single source (user wallpaper, app imagery, or a brand seed). Static baseline colors are available as defaults.

### HCT Color Model

| Property | Range | Description |
|---|---|---|
| **Hue** | 0–360 | The color family (red, orange, yellow, green, blue, violet) |
| **Chroma** | 0–∞ (practical max ~120) | Colorfulness or saturation; 0 = neutral gray |
| **Tone** | 0–100 | Lightness; 0 = pure black, 100 = pure white |

Tone is the primary driver of **accessible contrast**. A difference of ≥40 tonal steps ensures 3:1 contrast; ≥50 steps ensures 4.5:1. All default M3 pairings meet these thresholds.

### Tonal Palettes

From each of the five key seed colors, the system generates a **tonal palette** of 13 tones: 0, 10, 20, 30, 40, 50, 60, 70, 80, 90, 95, 99, 100.

The five palettes are:

| Palette | Role |
|---|---|
| **Primary** | Main brand color; used for key actions and components |
| **Secondary** | Supporting accent; complementary hue to primary |
| **Tertiary** | Contrasting accent; used sparingly for highlights |
| **Neutral** | Surfaces, backgrounds, outlines |
| **Neutral Variant** | Subtle surfaces and outline variants |
| **Error** | Error states and destructive actions |

### Color Roles

Color roles are semantic slots assigned to UI elements. They map tones from the palettes into specific use cases.

#### Accent Roles

| Token | Light | Dark | Usage |
|---|---|---|---|
| `primary` | P-40 | P-80 | Prominent elements, key actions |
| `on-primary` | P-100 | P-20 | Icons/text on Primary fill |
| `primary-container` | P-90 | P-30 | Less prominent fills |
| `on-primary-container` | P-10 | P-90 | Text/icons on Primary Container |
| `secondary` | S-40 | S-80 | Secondary actions |
| `on-secondary` | S-100 | S-20 | Text on Secondary |
| `secondary-container` | S-90 | S-30 | Secondary fills |
| `on-secondary-container` | S-10 | S-90 | Text on Secondary Container |
| `tertiary` | T-40 | T-80 | Contrasting accents |
| `on-tertiary` | T-100 | T-20 | Text on Tertiary |
| `tertiary-container` | T-90 | T-30 | Tertiary fills |
| `on-tertiary-container` | T-10 | T-90 | Text on Tertiary Container |
| `error` | E-40 | E-80 | Error indicators |
| `on-error` | E-100 | E-20 | Text on Error |
| `error-container` | E-90 | E-30 | Error fills |
| `on-error-container` | E-10 | E-90 | Text on Error Container |

#### Surface Roles

| Token | Light | Dark | Usage |
|---|---|---|---|
| `surface` | N-99 | N-10 | Default page/screen background |
| `on-surface` | N-10 | N-90 | Text/icons on Surface |
| `surface-variant` | NV-90 | NV-30 | Alternate surface fills |
| `on-surface-variant` | NV-30 | NV-80 | Text on Surface Variant |
| `surface-container-lowest` | N-100 | N-4 | Lowest container tone |
| `surface-container-low` | N-96 | N-10 | Low-emphasis containers |
| `surface-container` | N-94 | N-12 | Default container |
| `surface-container-high` | N-92 | N-17 | Elevated containers |
| `surface-container-highest` | N-90 | N-22 | Highest containers |
| `inverse-surface` | N-20 | N-90 | Inverted surface (toasts, chips) |
| `inverse-on-surface` | N-95 | N-20 | Text on Inverse Surface |
| `inverse-primary` | P-80 | P-40 | Primary on Inverse Surface |

#### Utility Roles

| Token | Usage |
|---|---|
| `outline` | Component borders, dividers |
| `outline-variant` | Lower-emphasis borders |
| `shadow` | Drop shadows (elevation) |
| `scrim` | Modal overlays, dimming layers |

### Fixed Accent Colors

For cases where color must not adapt (data visualization, brand marks), M3 provides fixed tonal roles:

| Token | Value |
|---|---|
| `primary-fixed` | P-90 |
| `primary-fixed-dim` | P-80 |
| `on-primary-fixed` | P-10 |
| `on-primary-fixed-variant` | P-30 |

_Secondary and tertiary equivalents follow the same pattern._

### Dynamic Color Pipeline

```
Source Color (wallpaper / brand)
      │
      ▼
 HCT Analysis
      │
      ▼
5 Key Colors (primary seed, secondary, tertiary, neutral, neutral-variant)
      │
      ▼
Tonal Palettes (13 tones each × 5 palettes = 65 reference colors)
      │
      ▼
Color Roles (mapped from palette tones to semantic slots)
      │
      ▼
Component Tokens (component-specific roles mapped to system roles)
```

### Baseline Color Scheme

The baseline static scheme (used when dynamic color is unavailable) uses Google Blue as its primary seed:

| Role | Light HEX | Dark HEX |
|---|---|---|
| `primary` | `#6750A4` | `#D0BCFF` |
| `on-primary` | `#FFFFFF` | `#381E72` |
| `primary-container` | `#EADDFF` | `#4F378B` |
| `on-primary-container` | `#21005D` | `#EADDFF` |
| `secondary` | `#625B71` | `#CCC2DC` |
| `secondary-container` | `#E8DEF8` | `#4A4458` |
| `tertiary` | `#7D5260` | `#EFB8C8` |
| `tertiary-container` | `#FFD8E4` | `#633B48` |
| `surface` | `#FFFBFE` | `#1C1B1F` |
| `on-surface` | `#1C1B1F` | `#E6E1E5` |
| `error` | `#B3261E` | `#F2B8B5` |
| `outline` | `#79747E` | `#938F99` |

### Color Accessibility Rules

- **Never** place `primary` text directly on `surface` without checking contrast.
- **Always** pair accent colors with their designated `on-*` counterpart.
- **Container** colors are for fills only — never use them for text.
- Minimum contrast ratios: **4.5:1** for normal text; **3:1** for large text and UI components.
- Do not rely on color alone to convey meaning; supplement with icons, labels, or patterns.

---

## Typography

### Typeface

Material Design 3 uses **Google Sans** (brand typeface) and **Google Sans Flex** (variable font, 2026) as its primary typefaces for Google-owned products. For third-party products, **Roboto** is the default system typeface. The `plain` slot accepts any sans-serif supporting system fonts.

| Slot | Default Typeface | Usage |
|---|---|---|
| `brand` | Google Sans / Google Sans Flex | Display, headlines, key UI text |
| `plain` | Roboto | Body text, captions, supporting text |

**Google Sans Flex** (released 2026) is a variable font with continuous axes for weight, width, and optical size — enabling richer expressive typesetting with a single font file.

### Type Scale

M3 defines 15 type styles across 5 categories, each with precise `size`, `line-height`, `weight`, and `tracking` values.

#### Display

| Style | Size | Line Height | Weight | Tracking |
|---|---|---|---|---|
| `display-large` | 57sp | 64sp | Regular (400) | −0.25sp |
| `display-medium` | 45sp | 52sp | Regular (400) | 0 |
| `display-small` | 36sp | 44sp | Regular (400) | 0 |

Display styles are for short, high-impact text at large sizes. Used for hero text and splash screens.

#### Headline

| Style | Size | Line Height | Weight | Tracking |
|---|---|---|---|---|
| `headline-large` | 32sp | 40sp | Regular (400) | 0 |
| `headline-medium` | 28sp | 36sp | Regular (400) | 0 |
| `headline-small` | 24sp | 32sp | Regular (400) | 0 |

Headlines provide high-emphasis text for section titles. Shorter than Display, used inside content.

#### Title

| Style | Size | Line Height | Weight | Tracking |
|---|---|---|---|---|
| `title-large` | 22sp | 28sp | Regular (400) | 0 |
| `title-medium` | 16sp | 24sp | Medium (500) | +0.15sp |
| `title-small` | 14sp | 20sp | Medium (500) | +0.1sp |

Titles are medium-emphasis text for component labels, toolbar titles, and section subheadings.

#### Body

| Style | Size | Line Height | Weight | Tracking |
|---|---|---|---|---|
| `body-large` | 16sp | 24sp | Regular (400) | +0.5sp |
| `body-medium` | 14sp | 20sp | Regular (400) | +0.25sp |
| `body-small` | 12sp | 16sp | Regular (400) | +0.4sp |

Body styles are for long-form reading. `body-large` is the default content style.

#### Label

| Style | Size | Line Height | Weight | Tracking |
|---|---|---|---|---|
| `label-large` | 14sp | 20sp | Medium (500) | +0.1sp |
| `label-medium` | 12sp | 16sp | Medium (500) | +0.5sp |
| `label-small` | 11sp | 16sp | Medium (500) | +0.5sp |

Labels are used for UI element text: buttons, tabs, chips, form labels, and overlines.

### Typography Tokens

Each style maps to a set of system tokens:

```
--md-sys-typescale-{style}-font
--md-sys-typescale-{style}-size
--md-sys-typescale-{style}-line-height
--md-sys-typescale-{style}-weight
--md-sys-typescale-{style}-tracking
```

Example — Body Large:
```css
--md-sys-typescale-body-large-font:      Roboto, sans-serif;
--md-sys-typescale-body-large-size:      1rem;       /* 16sp */
--md-sys-typescale-body-large-line-height: 1.5rem;   /* 24sp */
--md-sys-typescale-body-large-weight:    400;
--md-sys-typescale-body-large-tracking:  0.031rem;   /* +0.5sp */
```

### Typography Best Practices

- Use `display-*` sparingly — one per screen maximum for key hero moments.
- Default reading content should use `body-large` or `body-medium`.
- Button labels use `label-large`; tab labels use `label-medium`.
- Never set `tracking` to a large negative value for body text — it reduces readability.
- Minimum touch target text size: **11sp** (label-small). Never go below this in interactive elements.
- Line length for body text: **50–75 characters** per line for optimal readability.

---

## Motion

### Motion Principles

Material Design 3 motion is purposeful, natural, and responsive. Animation guides attention, communicates state changes, and reinforces spatial relationships within the UI.

M3 Expressive (2026) introduces **motion physics** — a spring-based token system replacing duration-based easing curves with more natural, physics-driven animations.

| Principle | Description |
|---|---|
| **Informative** | Communicates relationships and hierarchy through movement |
| **Responsive** | Acknowledges input immediately, even before an action completes |
| **Respectful** | Doesn't obstruct the user or delay task completion |

### Easing Curves

M3 defines four standard easing curves for traditional (duration-based) transitions:

| Easing | Token | Cubic Bezier | Usage |
|---|---|---|---|
| **Emphasized** | `motion.easing.emphasized` | Custom path | Spatial navigation, persistent elements |
| **Emphasized Decelerate** | `motion.easing.emphasized-decelerate` | (0.05, 0.7, 0.1, 1.0) | Elements entering the screen |
| **Emphasized Accelerate** | `motion.easing.emphasized-accelerate` | (0.3, 0.0, 0.8, 0.15) | Elements exiting the screen |
| **Standard** | `motion.easing.standard` | (0.2, 0.0, 0, 1.0) | Utility transitions, non-spatial changes |
| **Standard Decelerate** | `motion.easing.standard-decelerate` | (0, 0, 0, 1) | Simple enter |
| **Standard Accelerate** | `motion.easing.standard-accelerate` | (0.3, 0, 1, 1) | Simple exit |
| **Linear** | `motion.easing.linear` | (0, 0, 1, 1) | Color, opacity fades |

### Duration Tokens

| Token | Value | Usage |
|---|---|---|
| `motion.duration.short1` | 50ms | Hover/press state micro-interactions |
| `motion.duration.short2` | 100ms | Ripple, small state changes |
| `motion.duration.short3` | 150ms | FAB collapse, chip dismiss |
| `motion.duration.short4` | 200ms | Icon swap, switch toggle |
| `motion.duration.medium1` | 250ms | Dropdown open, tooltip appear |
| `motion.duration.medium2` | 300ms | Dialog open (short), chip expand |
| `motion.duration.medium3` | 350ms | Bottom sheet appear |
| `motion.duration.medium4` | 400ms | Navigation drawer open |
| `motion.duration.long1` | 450ms | Complex layout transitions |
| `motion.duration.long2` | 500ms | Page transitions (shared axis) |
| `motion.duration.long3` | 550ms | Full-screen dialog open |
| `motion.duration.long4` | 600ms | Full-screen navigation transitions |
| `motion.duration.extra-long1` | 700ms | Hero animation start |
| `motion.duration.extra-long2` | 800ms | Large spatial rearrangements |
| `motion.duration.extra-long3` | 900ms | Staggered enter (long list) |
| `motion.duration.extra-long4` | 1000ms | Full page hero / splash |

### Motion Physics (M3 Expressive, 2026)

Spring-based motion tokens replace fixed durations with physics parameters for more natural feel. Springs automatically compute their own duration based on mass, stiffness, and damping.

| Token | Spring Type | Stiffness | Damping | Usage |
|---|---|---|---|---|
| `motion.spring.default` | Standard | 380 | 30 | General component transitions |
| `motion.spring.bouncy` | Bouncy | 270 | 20 | Expressive, celebratory moments |
| `motion.spring.no-wobble` | Critically damped | 200 | 28 | Utility, precise placement |
| `motion.spring.stiff` | Stiff | 700 | 35 | Immediate-feeling responses |
| `motion.spring.gentle` | Gentle | 120 | 14 | Large spatial moves |

### Transition Patterns

#### Shared Axis
Used for navigating content with a clear spatial relationship (e.g., onboarding steps, tabs).

- **X-axis:** Horizontal slide + cross-fade  
- **Y-axis:** Vertical slide + cross-fade  
- **Z-axis:** Scale + cross-fade (zoom)

#### Container Transform
Used for transitions between a small UI element and a full-screen view (e.g., a card expanding to a detail page). The container morphs in position and size.

#### Fade Through
Used when navigating between destinations with no spatial relationship. The outgoing element fades and scales down; the incoming element fades and scales up.

#### Fade
Used for subtle state changes within a component (e.g., a placeholder becoming content).

### Motion Accessibility

- Respect the `prefers-reduced-motion` media query.
- Provide non-animated alternatives for all motion-critical interactions.
- Never use motion as the sole indicator of a state change.
- Avoid flashing content (>3 flashes per second) that may trigger photosensitive responses.

---

## Elevation

### Overview

Elevation in M3 is expressed through **surface tinting** — not shadows alone. As a surface rises in elevation, it receives an overlay of the `primary` color tinted at increasing opacity. This approach works in both light and dark themes, and removes reliance on drop shadows for depth perception.

Drop shadows are used as a secondary cue on specific components (FAB, dialogs).

### Elevation Levels

M3 defines 6 elevation levels:

| Level | Token | Overlay Opacity | dp | Usage |
|---|---|---|---|---|
| 0 | `elevation.level0` | 0% | 0dp | Flat surfaces; no elevation |
| 1 | `elevation.level1` | 5% | 1dp | Cards, navigation drawers (closed) |
| 2 | `elevation.level2` | 8% | 3dp | Menus, dropdowns |
| 3 | `elevation.level3` | 11% | 6dp | Navigation bar, bottom sheet (peeked) |
| 4 | `elevation.level4` | 12% | 8dp | Floating elements (reserved) |
| 5 | `elevation.level5` | 14% | 12dp | FAB, dialogs |

> Overlay opacity is applied using the `primary` color blended on top of the surface using the **alpha channel**. This creates the "tinted elevation" effect unique to M3.

### Component Elevation Map

| Component | Resting | Hover | Pressed | Dragging |
|---|---|---|---|---|
| Card (filled) | 0 | 1 | 0 | — |
| Card (elevated) | 1 | 2 | 1 | 4 |
| FAB | 3 | 4 | 3 | — |
| Navigation Drawer | — | — | — | — |
| Bottom Sheet | 1 | — | — | — |
| Dialog | 3 | — | — | — |
| Menu / Dropdown | 2 | — | — | — |
| Navigation Bar | 2 | — | — | — |
| Snackbar / Toast | 3 | — | — | — |

### Elevation & Dark Mode

In dark mode, elevation tinting is especially important for differentiating surfaces that would otherwise appear identical. Higher elevation = lighter surface (more primary overlay).

Surface container tokens (`surface-container-lowest` through `surface-container-highest`) automatically encode the appropriate tone for dark-theme elevation.

### Shadow Specs

For components that use drop shadows in addition to tinting:

| Level | Shadow Y | Blur | Spread | Opacity |
|---|---|---|---|---|
| 1 | 1px | 3px | 0 | 0.30 |
| 2 | 1px | 2px | 2px | 0.15; + 2px blur 6px spread 0.15 |
| 3 | 4px | 8px | 3px | 0.15; + 1px 3px 0 0.30 |
| 4 | 6px | 10px | 4px | 0.15; + 2px 3px 0 0.30 |
| 5 | 8px | 12px | 6px | 0.15; + 4px 4px 0 0.30 |

---

## Shape

### Overview

Shape is a defining characteristic of Material Design 3's visual identity. M3 uses **corner rounding** as the primary shape expression, with a system of named shape tokens that scale from fully sharp to fully round.

M3 Expressive (2026) dramatically expands the shape vocabulary with **35 distinct shape styles**, including asymmetric corners, notched edges, and shape morph animations.

### Shape Scale

The core shape scale uses corner radius values:

| Token | Corner Radius | Typical Usage |
|---|---|---|
| `shape.corner.none` | 0dp | Flat, structural elements |
| `shape.corner.extra-small` | 4dp | Chips (small), text fields |
| `shape.corner.extra-small-top` | Top 4dp only | Partial rounding |
| `shape.corner.small` | 8dp | Cards, buttons |
| `shape.corner.medium` | 12dp | Menus, dialogs |
| `shape.corner.large` | 16dp | Bottom sheets, navigation drawer |
| `shape.corner.large-end` | End 16dp only | Partial rounding |
| `shape.corner.large-top` | Top 16dp only | Bottom app bar |
| `shape.corner.extra-large` | 28dp | FAB, expanded FAB |
| `shape.corner.extra-large-top` | Top 28dp only | Large surface peeking |
| `shape.corner.full` | 50% (pill) | Badges, sliders, toggles |

### Component Shape Defaults

| Component | Shape Token | Radius |
|---|---|---|
| Button (all) | `shape.corner.full` | Pill |
| FAB (small) | `shape.corner.medium` | 12dp |
| FAB (default) | `shape.corner.large` | 16dp |
| FAB (large) | `shape.corner.large` | 28dp |
| Extended FAB | `shape.corner.large` | 16dp |
| Card (filled, elevated, outlined) | `shape.corner.medium` | 12dp |
| Chip (all) | `shape.corner.small` | 8dp |
| Dialog | `shape.corner.extra-large` | 28dp |
| Menu | `shape.corner.extra-small` | 4dp |
| Navigation Drawer | `shape.corner.large-end` | 16dp right |
| Bottom Sheet (modal) | `shape.corner.extra-large-top` | 28dp top |
| Text Field (filled) | `shape.corner.extra-small-top` | 4dp top |
| Text Field (outlined) | `shape.corner.extra-small` | 4dp |
| Snackbar | `shape.corner.extra-small` | 4dp |
| Switch | `shape.corner.full` | Pill |
| Slider | `shape.corner.full` | Pill handle |
| Badge | `shape.corner.full` | Pill |
| Tooltip | `shape.corner.extra-small` | 4dp |

### Expressive Shape Library (M3 Expressive)

The M3 Expressive update introduces decorative shapes for layout elements (not interactive components). These include:

- **Asymmetric roundrects** — different radii on each corner for playful, organic layouts
- **Clover / Petal** — multi-lobe decorative shapes for imagery and illustration containers
- **Squircle** — superellipse corners for a balanced feel between circles and squares
- **Notched / Cut** — chamfered or cut corners for technical / industrial aesthetics
- **Arch** — top rounded, flat bottom for cards and containers
- **Scalloped** — rhythmic wave edges for hero imagery or decorative panels

**Shape Morph:** Shapes can animate between states using the shape morph motion primitive, enabling expressive state transitions (e.g., a FAB morphing from round to pill when extended).

### Shape & Brand Expression

Shape personality scale:

```
← More Geometric / Technical          More Organic / Friendly →
 none   extra-small   small   medium   large   extra-large   full
  0dp       4dp        8dp     12dp     16dp       28dp      50%
```

Choose a shape personality and apply it consistently across components. Mixing extremes (none + full) without intent creates inconsistency.

---

## Iconography

### Material Symbols

Material Design 3 uses **Material Symbols** — a variable icon font with three optical styles and four axes of customization.

#### Icon Styles

| Style | Description |
|---|---|
| **Outlined** | Thin strokes with outlined fills; default recommended style |
| **Rounded** | Rounded terminals; friendlier, warmer aesthetic |
| **Sharp** | Square terminals; structured, technical aesthetic |

#### Variable Font Axes

| Axis | Token | Range | Default | Description |
|---|---|---|---|---|
| **Weight** | `WGHT` | 100–700 | 400 | Stroke weight (similar to font weight) |
| **Fill** | `FILL` | 0–1 | 0 | 0 = outlined; 1 = filled |
| **Grade** | `GRAD` | −25 to 200 | 0 | Fine-grained weight adjustment for display conditions |
| **Optical Size** | `opsz` | 20–48 | 24 | Adjusts detail level for the rendered size |

#### Recommended Configurations

| Context | Weight | Fill | Grade | Optical Size |
|---|---|---|---|---|
| Navigation (active) | 600 | 1 | 0 | 24 |
| Navigation (inactive) | 400 | 0 | 0 | 24 |
| Action icons | 400 | 0 | 0 | 24 |
| Display / hero | 200 | 0 | 0 | 48 |
| Dense UI | 400 | 0 | −25 | 20 |
| High-contrast dark | 400 | 0 | 200 | 24 |

### Icon Sizing

| Context | dp Size | Min Touch Target |
|---|---|---|
| Dense UI (chips, labels) | 16dp | N/A (non-interactive) |
| Standard UI | 24dp | 48dp touch target |
| Navigation | 24dp | 48dp touch target |
| FAB icon | 24dp | — |
| Large / hero icons | 32–48dp | 48dp touch target |

### Icon Usage Rules

- Use **outlined** icons for inactive states; **filled** for active/selected states.
- Always pair icons with labels in navigation unless space is critically constrained.
- Never scale icons below 16dp — detail is lost at smaller optical sizes.
- Apply icon color from the relevant `on-*` color role (e.g., `on-surface`, `on-primary`).

---

## Layout & Adaptive Design

### Layout Principles

Material Design 3 uses a **grid + margin + gutter** system that adapts across breakpoints. The layout system is built on columns that contract and expand, with components responding to available space.

### Breakpoints & Window Size Classes

M3 defines window size classes for adaptive layouts:

| Class | Width | Columns | Gutter | Margin | Typical Form Factor |
|---|---|---|---|---|---|
| **Compact** | < 600dp | 4 | 16dp | 16dp | Phone portrait |
| **Medium** | 600–839dp | 8 | 24dp | 24dp | Tablet portrait, phone landscape |
| **Expanded** | ≥ 840dp | 12 | 24dp | 24dp | Tablet landscape, desktop, foldable open |

### Navigation Patterns by Breakpoint

| Breakpoint | Navigation Component | Notes |
|---|---|---|
| Compact | Navigation Bar (bottom) | 3–5 destinations, icons + labels |
| Compact | Navigation Drawer (modal) | For 5+ destinations; full-screen overlay |
| Medium | Navigation Rail | Vertical rail, icons + optional labels |
| Expanded | Navigation Drawer (standard) | Always-visible side panel |
| Expanded | Navigation Drawer (modal) | Overlay when space is constrained |

### Canonical Layouts

M3 defines canonical layout patterns for common use cases:

| Pattern | Description | Best For |
|---|---|---|
| **List-Detail** | Two-panel: list on left, detail on right | Email, settings, messaging |
| **Supporting Panel** | Main content + contextual side panel | Docs with outline/properties |
| **Feed** | Vertically scrolling cards | News, social, discovery |

### Spacing System

M3 uses a **4dp base unit** for all spacing values. Common spacing:

| Token | Value | Use |
|---|---|---|
| `spacing.1` | 4dp | Micro gaps between inline elements |
| `spacing.2` | 8dp | Tight padding inside components |
| `spacing.3` | 12dp | Standard internal component padding |
| `spacing.4` | 16dp | Standard section padding; default margin |
| `spacing.5` | 20dp | Navigation rail width offset |
| `spacing.6` | 24dp | Dialog padding; large internal padding |
| `spacing.8` | 32dp | Section spacing |
| `spacing.12` | 48dp | Minimum touch target height |
| `spacing.16` | 64dp | Large content blocks |

### Touch Targets

- **Minimum touch target:** 48×48dp for all interactive elements.
- Interactive elements smaller than 48dp must have invisible padding to meet the target size.
- Maintain **8dp minimum spacing** between adjacent touch targets.

---

## Accessibility

### Overview

Material Design 3 is designed to meet or exceed **WCAG 2.1 AA** across all components and color roles. The system is built with accessibility as a foundation, not an afterthought.

### Color Contrast

| Text Type | Minimum Ratio | Enhanced Ratio (AAA) |
|---|---|---|
| Normal text (<18sp regular, <14sp bold) | 4.5:1 | 7:1 |
| Large text (≥18sp regular, ≥14sp bold) | 3:1 | 4.5:1 |
| UI components and graphical objects | 3:1 | — |
| Disabled elements | No requirement | — |

All default M3 color role pairings meet 4.5:1 contrast. Dynamic color uses HCT tone to guarantee contrast even when user wallpaper changes.

### Interaction & Focus

- **Focus indicators:** Every interactive element must have a visible focus ring (default: `outline` color, 3dp, with 2dp offset).
- **Keyboard navigation:** All components support Tab, Enter, Space, Arrow key navigation.
- **Focus trap:** Modal dialogs and bottom sheets trap focus within until dismissed.

### Touch & Motor

- **Minimum touch target:** 48×48dp.
- **Swipe interactions** must have a button/tap alternative.
- **Gesture shortcuts** must be supplemented with standard interaction paths.

### Screen Readers

- All interactive elements must have accessible labels (`contentDescription` on Android, `aria-label` on web).
- Icon-only buttons must always include a descriptive label for screen readers.
- Use `LiveRegion` / `aria-live` to announce dynamic content changes.
- Ensure reading order follows visual order in all layouts.

### Reduced Motion

```css
@media (prefers-reduced-motion: reduce) {
  * {
    animation-duration: 0.01ms !important;
    transition-duration: 0.01ms !important;
  }
}
```

- Replace spatial transitions (slide, scale) with simple cross-fades.
- Preserve functional animations (loading spinners, progress bars) but minimize decoration.

### Text Scaling

- All text must scale correctly from system text size settings (up to 200% on Android).
- Use `sp` units for text sizes in Android; `rem`/`em` on web.
- Test layouts with font scale at 85%, 100%, 115%, 130%, 150%, 200%.
- Never truncate essential UI text — allow wrapping.

---

*Last updated: June 2026 · Material Design 3 Expressive*
