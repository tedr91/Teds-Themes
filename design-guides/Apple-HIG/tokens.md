# Apple Design System — tokens.md
> Aligned to Apple Human Interface Guidelines · iOS 18 / iPadOS 18 / macOS Sequoia / visionOS 2

---

## Table of Contents

1. [Token Architecture](#1-token-architecture)
2. [Color Tokens](#2-color-tokens)
3. [Typography Tokens](#3-typography-tokens)
4. [Spacing Tokens](#4-spacing-tokens)
5. [Border & Shape Tokens](#5-border--shape-tokens)
6. [Shadow & Elevation Tokens](#6-shadow--elevation-tokens)
7. [Motion Tokens](#7-motion-tokens)
8. [Icon & Symbol Tokens](#8-icon--symbol-tokens)
9. [Platform Overrides](#9-platform-overrides)
10. [Token Usage Reference](#10-token-usage-reference)

---

## 1. Token Architecture

Apple's design token system operates in **three tiers**:

```
Primitive Tokens  →  Semantic Tokens  →  Component Tokens
     (#007AFF)         (color.tint)       (button.background)
```

| Tier | Description | When to Use |
|---|---|---|
| **Primitive** | Raw values (hex, pt, ms). Named after their value, not purpose. | Define semantic tokens; never use directly in components. |
| **Semantic** | Named by purpose/role (e.g., `color.label.primary`). Resolve to a primitive. | In all component and layout definitions. |
| **Component** | Specific to a single component (e.g., `button.primary.background`). Alias of a semantic token. | Inside component code only. |

### Naming Convention

```
{category}.{variant}.{property}.{state}

Examples:
  color.label.primary
  color.label.primary.disabled
  color.fill.tertiary
  spacing.inset.standard
  radius.container.card
  motion.duration.navigation
  type.style.headline.size
```

---

## 2. Color Tokens

### 2.1 Primitive Color Palette

These are raw named values. **Do not reference in product code** — use semantic tokens below.

#### Blues
| Token | Light | Dark |
|---|---|---|
| `primitive.blue.50` | `#EBF5FF` | `#001930` |
| `primitive.blue.100` | `#C4E0FF` | `#002D5C` |
| `primitive.blue.300` | `#5BA9FF` | `#0A5DC9` |
| `primitive.blue.500` | `#007AFF` | `#0A84FF` |
| `primitive.blue.700` | `#0051D4` | `#409CFF` |
| `primitive.blue.900` | `#002D82` | `#6FBEFF` |

#### Greens
| Token | Light | Dark |
|---|---|---|
| `primitive.green.500` | `#34C759` | `#30D158` |
| `primitive.green.700` | `#248A3D` | `#3EE258` |

#### Reds
| Token | Light | Dark |
|---|---|---|
| `primitive.red.500` | `#FF3B30` | `#FF453A` |
| `primitive.red.700` | `#C0221A` | `#FF6961` |

#### Oranges
| Token | Light | Dark |
|---|---|---|
| `primitive.orange.500` | `#FF9500` | `#FF9F0A` |
| `primitive.orange.700` | `#C47000` | `#FFB340` |

#### Purples
| Token | Light | Dark |
|---|---|---|
| `primitive.purple.500` | `#AF52DE` | `#BF5AF2` |
| `primitive.purple.700` | `#882DC7` | `#DA8FFF` |

#### Pinks
| Token | Light | Dark |
|---|---|---|
| `primitive.pink.500` | `#FF2D55` | `#FF375F` |
| `primitive.pink.700` | `#C30035` | `#FF6B81` |

#### Teals
| Token | Light | Dark |
|---|---|---|
| `primitive.teal.500` | `#5AC8FA` | `#64D2FF` |

#### Yellows
| Token | Light | Dark |
|---|---|---|
| `primitive.yellow.500` | `#FFCC00` | `#FFD60A` |

#### Neutrals (Gray Scale)
| Token | Light | Dark |
|---|---|---|
| `primitive.gray.50` | `#F2F2F7` | `#1C1C1E` |
| `primitive.gray.100` | `#E5E5EA` | `#2C2C2E` |
| `primitive.gray.200` | `#D1D1D6` | `#3A3A3C` |
| `primitive.gray.300` | `#C7C7CC` | `#48484A` |
| `primitive.gray.400` | `#AEAEB2` | `#636366` |
| `primitive.gray.500` | `#8E8E93` | `#8E8E93` |
| `primitive.gray.600` | `#6C6C70` | `#AEAEB2` |
| `primitive.gray.700` | `#48484A` | `#C7C7CC` |
| `primitive.gray.800` | `#3A3A3C` | `#D1D1D6` |
| `primitive.gray.900` | `#1C1C1E` | `#F2F2F7` |
| `primitive.white` | `#FFFFFF` | `#FFFFFF` |
| `primitive.black` | `#000000` | `#000000` |

---

### 2.2 Semantic Color Tokens

#### Label (Text) Colors
| Token | Light | Dark | Usage |
|---|---|---|---|
| `color.label.primary` | `rgba(0,0,0,1.00)` | `rgba(255,255,255,1.00)` | Body text, titles |
| `color.label.secondary` | `rgba(60,60,67,0.60)` | `rgba(235,235,245,0.60)` | Subtitles, captions |
| `color.label.tertiary` | `rgba(60,60,67,0.30)` | `rgba(235,235,245,0.30)` | Placeholder, disabled hints |
| `color.label.quaternary` | `rgba(60,60,67,0.18)` | `rgba(235,235,245,0.18)` | Decorative marks |
| `color.label.link` | `primitive.blue.500` | `primitive.blue.500` | Hyperlinks |
| `color.label.placeholder` | `rgba(60,60,67,0.30)` | `rgba(235,235,245,0.30)` | Text field placeholder |

#### Fill Colors
| Token | Light | Dark | Usage |
|---|---|---|---|
| `color.fill.primary` | `rgba(120,120,128,0.20)` | `rgba(120,120,128,0.36)` | Thin overlay |
| `color.fill.secondary` | `rgba(120,120,128,0.16)` | `rgba(120,120,128,0.32)` | Medium overlay |
| `color.fill.tertiary` | `rgba(118,118,128,0.12)` | `rgba(118,118,128,0.24)` | Tab bars, search |
| `color.fill.quaternary` | `rgba(116,116,128,0.08)` | `rgba(118,118,128,0.18)` | Thick overlay |

#### Background Colors
| Token | Light | Dark | Usage |
|---|---|---|---|
| `color.background.primary` | `#FFFFFF` | `#000000` | Main view background |
| `color.background.secondary` | `#F2F2F7` | `#1C1C1E` | Grouped table bg |
| `color.background.tertiary` | `#FFFFFF` | `#2C2C2E` | Cards in grouped list |
| `color.background.grouped.primary` | `#F2F2F7` | `#000000` | Inset grouped bg |
| `color.background.grouped.secondary` | `#FFFFFF` | `#1C1C1E` | Inner cards |
| `color.background.grouped.tertiary` | `#F2F2F7` | `#2C2C2E` | Nested content |

#### Separator Colors
| Token | Light | Dark | Usage |
|---|---|---|---|
| `color.separator.opaque` | `#C6C6C8` | `#38383A` | Full-width separators |
| `color.separator.nonOpaque` | `rgba(60,60,67,0.29)` | `rgba(84,84,88,0.65)` | Inset separators |

#### Tint / Accent Colors
| Token | Light | Dark | Usage |
|---|---|---|---|
| `color.tint.default` | `primitive.blue.500` | `primitive.blue.500` | Default system tint |
| `color.tint.app` | *(app-defined)* | *(app-defined)* | App-level accent |
| `color.tint.destructive` | `primitive.red.500` | `primitive.red.500` | Destructive actions |
| `color.tint.warning` | `primitive.orange.500` | `primitive.orange.500` | Warnings |
| `color.tint.success` | `primitive.green.500` | `primitive.green.500` | Confirmations |

#### System Colors (Semantic Aliases)
| Token | Resolves To |
|---|---|
| `color.system.blue` | `primitive.blue.500` (adaptive) |
| `color.system.green` | `primitive.green.500` (adaptive) |
| `color.system.red` | `primitive.red.500` (adaptive) |
| `color.system.orange` | `primitive.orange.500` (adaptive) |
| `color.system.yellow` | `primitive.yellow.500` (adaptive) |
| `color.system.pink` | `primitive.pink.500` (adaptive) |
| `color.system.purple` | `primitive.purple.500` (adaptive) |
| `color.system.teal` | `primitive.teal.500` (adaptive) |
| `color.system.gray` | `primitive.gray.500` |

---

### 2.3 Component Color Tokens

#### Button
| Token | Value |
|---|---|
| `button.primary.background` | `color.tint.default` |
| `button.primary.foreground` | `primitive.white` |
| `button.primary.background.disabled` | `color.fill.tertiary` |
| `button.primary.foreground.disabled` | `color.label.tertiary` |
| `button.secondary.background` | `color.fill.secondary` |
| `button.secondary.foreground` | `color.tint.default` |
| `button.destructive.background` | `color.tint.destructive` |
| `button.destructive.foreground` | `primitive.white` |

#### Text Field
| Token | Value |
|---|---|
| `textfield.background` | `color.fill.quaternary` |
| `textfield.border.default` | `color.separator.nonOpaque` |
| `textfield.border.focused` | `color.tint.default` |
| `textfield.border.error` | `color.tint.destructive` |
| `textfield.text` | `color.label.primary` |
| `textfield.placeholder` | `color.label.placeholder` |

#### Navigation Bar
| Token | Value |
|---|---|
| `navbar.background` | `color.background.primary` (with `ultraThinMaterial`) |
| `navbar.title` | `color.label.primary` |
| `navbar.tint` | `color.tint.app` |

#### Tab Bar
| Token | Value |
|---|---|
| `tabbar.background` | `color.fill.tertiary` (with `thickMaterial`) |
| `tabbar.item.selected` | `color.tint.app` |
| `tabbar.item.unselected` | `color.label.tertiary` |
| `tabbar.badge.background` | `color.system.red` |
| `tabbar.badge.text` | `primitive.white` |

---

## 3. Typography Tokens

### 3.1 Font Family Tokens
| Token | Value | Usage |
|---|---|---|
| `font.family.sans` | `SF Pro` | iOS, iPadOS, macOS, tvOS |
| `font.family.rounded` | `SF Pro Rounded` | Friendly, casual contexts |
| `font.family.mono` | `SF Mono` | Code, terminal |
| `font.family.serif` | `New York` | Editorial, reading |
| `font.family.compact` | `SF Compact` | watchOS |

### 3.2 Type Style Tokens (iOS / iPadOS)

| Token | Size | Weight | Line Height | Tracking |
|---|---|---|---|---|
| `type.style.largeTitle` | 34 pt | Regular (400) | 41 pt | +0.37 pt |
| `type.style.title1` | 28 pt | Regular (400) | 34 pt | +0.36 pt |
| `type.style.title2` | 22 pt | Regular (400) | 28 pt | +0.35 pt |
| `type.style.title3` | 20 pt | Regular (400) | 25 pt | +0.38 pt |
| `type.style.headline` | 17 pt | Semibold (600) | 22 pt | -0.41 pt |
| `type.style.body` | 17 pt | Regular (400) | 22 pt | -0.41 pt |
| `type.style.callout` | 16 pt | Regular (400) | 21 pt | -0.32 pt |
| `type.style.subheadline` | 15 pt | Regular (400) | 20 pt | -0.23 pt |
| `type.style.footnote` | 13 pt | Regular (400) | 18 pt | -0.08 pt |
| `type.style.caption1` | 12 pt | Regular (400) | 16 pt | 0 pt |
| `type.style.caption2` | 11 pt | Regular (400) | 13 pt | +0.07 pt |

### 3.3 Type Style Tokens (macOS)

| Token | Size | Weight | Line Height |
|---|---|---|---|
| `type.style.mac.largeTitle` | 26 pt | Regular (400) | 32 pt |
| `type.style.mac.title1` | 22 pt | Regular (400) | 26 pt |
| `type.style.mac.title2` | 17 pt | Regular (400) | 22 pt |
| `type.style.mac.title3` | 15 pt | Regular (400) | 20 pt |
| `type.style.mac.headline` | 13 pt | Semibold (600) | 16 pt |
| `type.style.mac.body` | 13 pt | Regular (400) | 16 pt |
| `type.style.mac.callout` | 12 pt | Regular (400) | 15 pt |
| `type.style.mac.subheadline` | 11 pt | Regular (400) | 14 pt |
| `type.style.mac.footnote` | 10 pt | Regular (400) | 13 pt |
| `type.style.mac.caption1` | 10 pt | Regular (400) | 13 pt |
| `type.style.mac.caption2` | 10 pt | Medium (500) | 13 pt |

### 3.4 Font Weight Tokens
| Token | CSS / SwiftUI Weight | Numeric |
|---|---|---|
| `font.weight.ultraLight` | `.ultraLight` | 100 |
| `font.weight.thin` | `.thin` | 200 |
| `font.weight.light` | `.light` | 300 |
| `font.weight.regular` | `.regular` | 400 |
| `font.weight.medium` | `.medium` | 500 |
| `font.weight.semibold` | `.semibold` | 600 |
| `font.weight.bold` | `.bold` | 700 |
| `font.weight.heavy` | `.heavy` | 800 |
| `font.weight.black` | `.black` | 900 |

---

## 4. Spacing Tokens

All spacing is based on a **4 pt base unit**. Standard values are multiples of 4.

### 4.1 Base Scale
| Token | Value | Usage |
|---|---|---|
| `spacing.0` | 0 pt | Zero (intentionally explicit) |
| `spacing.1` | 2 pt | Hairline gaps, icon badge offset |
| `spacing.2` | 4 pt | Tight icon-to-label, tag padding |
| `spacing.3` | 6 pt | Compact row padding |
| `spacing.4` | 8 pt | Default icon padding, small gaps |
| `spacing.5` | 10 pt | Small component padding |
| `spacing.6` | 12 pt | Card internal padding, row insets |
| `spacing.7` | 14 pt | Standard list row vertical padding |
| `spacing.8` | 16 pt | Standard content margin (compact) |
| `spacing.9` | 18 pt | |
| `spacing.10` | 20 pt | Content margin (regular), section gap |
| `spacing.12` | 24 pt | Modal horizontal padding |
| `spacing.14` | 28 pt | |
| `spacing.16` | 32 pt | Section header gap |
| `spacing.20` | 40 pt | Large section spacing |
| `spacing.24` | 48 pt | Hero / display spacing |
| `spacing.32` | 64 pt | Extra large section gap |

### 4.2 Semantic Spacing Tokens
| Token | Value | Usage |
|---|---|---|
| `spacing.inset.xs` | `spacing.4` (8 pt) | Tag, badge padding |
| `spacing.inset.sm` | `spacing.6` (12 pt) | Small button padding |
| `spacing.inset.md` | `spacing.8` (16 pt) | Standard padding (compact width) |
| `spacing.inset.lg` | `spacing.10` (20 pt) | Standard padding (regular width) |
| `spacing.inset.xl` | `spacing.12` (24 pt) | Modal, sheet padding |
| `spacing.gap.xs` | `spacing.2` (4 pt) | Icon-text micro gap |
| `spacing.gap.sm` | `spacing.4` (8 pt) | List item gap |
| `spacing.gap.md` | `spacing.6` (12 pt) | Card item gap |
| `spacing.gap.lg` | `spacing.8` (16 pt) | Section item gap |
| `spacing.gap.xl` | `spacing.10` (20 pt) | Grid item gap |
| `spacing.stack.sm` | `spacing.4` (8 pt) | Tight label stack |
| `spacing.stack.md` | `spacing.6` (12 pt) | Standard label stack |
| `spacing.stack.lg` | `spacing.12` (24 pt) | Card content stack |
| `spacing.stack.xl` | `spacing.16` (32 pt) | Section divider |

### 4.3 Component-Specific Spacing
| Token | Value | Component |
|---|---|---|
| `spacing.list.row.vertical` | `spacing.7` (14 pt) | List row top/bottom |
| `spacing.list.inset.horizontal` | `spacing.8` (16 pt) | List row leading margin |
| `spacing.list.section.header` | `spacing.5` (10 pt) | Section header top padding |
| `spacing.card.padding` | `spacing.8` (16 pt) | Card internal padding |
| `spacing.card.gap` | `spacing.6` (12 pt) | Elements within a card |
| `spacing.button.horizontal.sm` | `spacing.6` (12 pt) | Small button h-padding |
| `spacing.button.horizontal.md` | `spacing.8` (16 pt) | Medium button h-padding |
| `spacing.button.horizontal.lg` | `spacing.10` (20 pt) | Large button h-padding |
| `spacing.button.vertical.sm` | `spacing.3` (6 pt) | Small button v-padding |
| `spacing.button.vertical.md` | `spacing.5` (10 pt) | Medium button v-padding |
| `spacing.button.vertical.lg` | `spacing.7` (14 pt) | Large button v-padding |
| `spacing.navbar.height` | 44 pt | Navigation bar height |
| `spacing.tabbar.height` | 49 pt | Tab bar height (iPhone) |
| `spacing.toolbar.height` | 44 pt | Toolbar height (iPad) |

---

## 5. Border & Shape Tokens

### 5.1 Corner Radius Tokens
| Token | Value | Usage |
|---|---|---|
| `radius.xs` | 4 pt | Tags, badges, small chips |
| `radius.sm` | 8 pt | Buttons (mini, small), inner cards |
| `radius.md` | 10 pt | Buttons (default), text fields |
| `radius.lg` | 12 pt | Cards, grouped list sections |
| `radius.xl` | 16 pt | Modal cards, large cards |
| `radius.2xl` | 20 pt | Sheet top corners, large panels |
| `radius.3xl` | 24 pt | Full-width hero cards |
| `radius.full` | 9999 pt | Pills, toggle switch track |
| `radius.container.button.sm` | `radius.sm` | |
| `radius.container.button.md` | `radius.md` | |
| `radius.container.button.lg` | `radius.lg` | |
| `radius.container.card` | `radius.xl` | |
| `radius.container.sheet` | `radius.2xl` | |
| `radius.container.input` | `radius.md` | |

### 5.2 Border Width Tokens
| Token | Value | Usage |
|---|---|---|
| `border.width.hairline` | 0.33 pt | Separator lines (1 px on 3x display) |
| `border.width.thin` | 0.5 pt | Subtle dividers |
| `border.width.default` | 1 pt | Standard borders |
| `border.width.medium` | 1.5 pt | Selected state borders |
| `border.width.thick` | 2 pt | Focus rings, emphasis borders |

### 5.3 Border Color Tokens
| Token | Value | Usage |
|---|---|---|
| `border.color.separator` | `color.separator.opaque` | List separators |
| `border.color.separator.light` | `color.separator.nonOpaque` | Inset separators |
| `border.color.input.default` | `color.fill.tertiary` | Text field border |
| `border.color.input.focused` | `color.tint.default` | Focused text field |
| `border.color.input.error` | `color.tint.destructive` | Validation error |
| `border.color.card` | `color.separator.nonOpaque` | Subtle card border (dark mode) |

---

## 6. Shadow & Elevation Tokens

Apple uses shadows sparingly. In dark mode, elevation is communicated via **background lightness**, not shadow.

| Token | Offset Y | Blur | Spread | Opacity (Light) | Opacity (Dark) | Usage |
|---|---|---|---|---|---|---|
| `shadow.none` | 0 | 0 | 0 | 0% | 0% | Flat surfaces |
| `shadow.xs` | 1 pt | 2 pt | 0 | 4% | 0% | Subtle depth (badges) |
| `shadow.sm` | 2 pt | 4 pt | 0 | 6% | 0% | Small cards, buttons |
| `shadow.md` | 4 pt | 8 pt | 0 | 8% | 0% | Standard cards |
| `shadow.lg` | 8 pt | 16 pt | 0 | 10% | 0% | Modals, popovers |
| `shadow.xl` | 12 pt | 24 pt | 0 | 12% | 0% | Floating panels, sheets |
| `shadow.color` | — | — | — | — | — | `primitive.black` |

**Dark Mode Rule:** Set all shadow opacities to 0 in dark mode. Use `color.background.tertiary` vs `color.background.secondary` contrast to express elevation instead.

---

## 7. Motion Tokens

### 7.1 Duration Tokens
| Token | Value | Usage |
|---|---|---|
| `motion.duration.instant` | 0 ms | State changes with no animation |
| `motion.duration.micro` | 100 ms | Micro-interactions (tap scale) |
| `motion.duration.fast` | 200 ms | Component state changes |
| `motion.duration.standard` | 300 ms | General transitions |
| `motion.duration.navigation` | 400 ms | Navigation push/pop |
| `motion.duration.modal` | 500 ms | Modal presentation |
| `motion.duration.slow` | 600 ms | Complex transitions |
| `motion.duration.ambient` | 1000–3000 ms | Looping idle animations |

### 7.2 Spring Animation Tokens

```swift
// SwiftUI implementation references
motion.spring.snappy    → .spring(response: 0.25, dampingFraction: 0.7)
motion.spring.standard  → .spring(response: 0.35, dampingFraction: 0.82)
motion.spring.smooth    → .spring(response: 0.50, dampingFraction: 0.9)
motion.spring.bouncy    → .spring(response: 0.40, dampingFraction: 0.6)
motion.spring.tight     → .spring(response: 0.20, dampingFraction: 0.85)
```

| Token | Response | Damping | Use Case |
|---|---|---|---|
| `motion.spring.snappy` | 0.25 s | 0.70 | Buttons, toggles |
| `motion.spring.standard` | 0.35 s | 0.82 | Lists, card appearance |
| `motion.spring.smooth` | 0.50 s | 0.90 | Modals, sheets |
| `motion.spring.bouncy` | 0.40 s | 0.60 | Playful UI, onboarding |
| `motion.spring.tight` | 0.20 s | 0.85 | Toolbar items, quick feedback |

### 7.3 Easing Tokens (Non-spring, CSS/CA contexts)

| Token | Cubic Bezier | Usage |
|---|---|---|
| `motion.easing.linear` | `cubic-bezier(0, 0, 1, 1)` | Continuous loops |
| `motion.easing.easeIn` | `cubic-bezier(0.42, 0, 1, 1)` | Exit / leave |
| `motion.easing.easeOut` | `cubic-bezier(0, 0, 0.58, 1)` | Enter / appear |
| `motion.easing.easeInOut` | `cubic-bezier(0.42, 0, 0.58, 1)` | Within-view state change |
| `motion.easing.standard` | `cubic-bezier(0.25, 0.1, 0.25, 1)` | General motion |

### 7.4 Reduce Motion Tokens

| Token | Default | Reduce Motion Override |
|---|---|---|
| `motion.reducedMotion.duration` | Use standard tokens | `motion.duration.fast` (150 ms crossfade) |
| `motion.reducedMotion.spring` | Use spring tokens | `.easeOut` with `motion.duration.fast` |
| `motion.reducedMotion.parallax` | Enabled | Disabled |
| `motion.reducedMotion.ambient` | Enabled | Disabled |

---

## 8. Icon & Symbol Tokens

### 8.1 SF Symbol Scale Tokens
| Token | Scale | Size Relative to Text | Usage |
|---|---|---|---|
| `icon.scale.small` | `.small` | ~80% of cap height | Dense toolbars, status bar |
| `icon.scale.medium` | `.medium` | ~100% of cap height | Inline with body text (default) |
| `icon.scale.large` | `.large` | ~140% of cap height | Primary actions, empty states |

### 8.2 SF Symbol Weight Tokens
| Token | Value | When to Use |
|---|---|---|
| `icon.weight.ultraLight` | `.ultraLight` | Decorative, background |
| `icon.weight.thin` | `.thin` | Subtle UI elements |
| `icon.weight.light` | `.light` | Supporting icons |
| `icon.weight.regular` | `.regular` | Default (matches body text) |
| `icon.weight.medium` | `.medium` | Slightly emphasized |
| `icon.weight.semibold` | `.semibold` | Matches headline/semibold text |
| `icon.weight.bold` | `.bold` | Strong emphasis |
| `icon.weight.heavy` | `.heavy` | High-contrast UI |
| `icon.weight.black` | `.black` | Maximum weight |

**Rule:** Symbol weight should match the typographic weight of the adjacent text label.

### 8.3 App Icon Size Tokens
| Token | Size (px) | Platform | Context |
|---|---|---|---|
| `icon.app.ios.1024` | 1024 × 1024 | iOS / App Store | App Store submission |
| `icon.app.ios.180` | 180 × 180 | iOS | Home Screen (3x) |
| `icon.app.ios.120` | 120 × 120 | iOS | Home Screen (2x) |
| `icon.app.ipad.167` | 167 × 167 | iPadOS | Home Screen (2x Pro) |
| `icon.app.ipad.152` | 152 × 152 | iPadOS | Home Screen (2x) |
| `icon.app.mac.512` | 512 × 512 | macOS | Finder / Dock (2x) |
| `icon.app.mac.256` | 256 × 256 | macOS | Finder (1x) |
| `icon.app.vision.1024` | 1024 × 1024 | visionOS | App Store / Home View |
| `icon.app.watch.44` | 88 × 88 | watchOS | 44mm face (2x) |

---

## 9. Platform Overrides

Some tokens resolve differently per platform. Below are the key divergences.

### 9.1 Spacing Overrides
| Token | iOS | iPadOS | macOS |
|---|---|---|---|
| `spacing.inset.md` | 16 pt | 20 pt | 20 pt |
| `spacing.list.inset.horizontal` | 16 pt | 20 pt | 16 pt |
| `spacing.navbar.height` | 44 pt | 44 pt | 22 pt (title bar) |
| `spacing.tabbar.height` | 49 pt | 49 pt (sidebar) | N/A |
| `spacing.button.vertical.md` | 10 pt | 10 pt | 5 pt (compact) |

### 9.2 Typography Overrides
| Token | iOS / iPadOS | macOS |
|---|---|---|
| `type.style.body` | 17 pt Regular | 13 pt Regular |
| `type.style.headline` | 17 pt Semibold | 13 pt Semibold |
| `type.style.largeTitle` | 34 pt Regular | 26 pt Regular |
| `type.style.caption1` | 12 pt Regular | 10 pt Regular |

### 9.3 Radius Overrides
| Token | iOS | macOS |
|---|---|---|
| `radius.container.button.md` | 10 pt | 6 pt |
| `radius.container.card` | 16 pt | 10 pt |
| `radius.container.sheet` | 20 pt | 10 pt (panel) |

### 9.4 Control Size Overrides (macOS)
| Control | Regular | Small | Mini |
|---|---|---|---|
| Button height | 22 pt | 18 pt | 15 pt |
| Text field height | 22 pt | 19 pt | 16 pt |
| Checkbox size | 14 pt | 12 pt | 10 pt |
| Segmented control height | 22 pt | 18 pt | 15 pt |

---

## 10. Token Usage Reference

### 10.1 Quick-Lookup: Most Common Tokens

| Design Decision | Token |
|---|---|
| Primary text | `color.label.primary` |
| Secondary text | `color.label.secondary` |
| App tint / accent | `color.tint.app` |
| Destructive | `color.tint.destructive` |
| Page background | `color.background.primary` |
| Card background | `color.background.grouped.secondary` |
| Separator | `color.separator.nonOpaque` |
| Body text style | `type.style.body` |
| Heading text style | `type.style.headline` |
| Standard padding | `spacing.inset.md` |
| Card padding | `spacing.card.padding` |
| Card corner radius | `radius.container.card` |
| Card shadow | `shadow.md` |
| Standard transition | `motion.spring.standard` |
| Navigation transition | `motion.duration.navigation` |

### 10.2 Token Chaining Example

```
Component: Primary Button (enabled, large, iOS)

background       → button.primary.background
                 → color.tint.default
                 → primitive.blue.500 [light: #007AFF, dark: #0A84FF]

foreground       → button.primary.foreground
                 → primitive.white [#FFFFFF]

font             → type.style.headline
                 → { size: 17pt, weight: 600, lineHeight: 22pt }

padding.h        → spacing.button.horizontal.lg
                 → spacing.10 → 20pt

padding.v        → spacing.button.vertical.lg
                 → spacing.7 → 14pt

corner-radius    → radius.container.button.lg
                 → radius.lg → 12pt

shadow           → shadow.none (buttons are flat by default)

transition       → motion.spring.snappy
                 → { response: 0.25s, damping: 0.70 }
```

---

*Last updated: June 2025 · Source: Apple Human Interface Guidelines, Apple Design Resources, WWDC 2024–2025*
