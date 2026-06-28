# Apple Design System — design.md
> Aligned to Apple Human Interface Guidelines · iOS 18 / iPadOS 18 / macOS Sequoia / visionOS 2

---

## Table of Contents

1. [Design Principles](#1-design-principles)
2. [Layout](#2-layout)
3. [Typography](#3-typography)
4. [Color](#4-color)
5. [Materials & Vibrancy](#5-materials--vibrancy)
6. [Iconography & SF Symbols](#6-iconography--sf-symbols)
7. [Motion & Animation](#7-motion--animation)
8. [Platform-Specific Patterns](#8-platform-specific-patterns)
9. [Accessibility Foundations](#9-accessibility-foundations)

---

## 1. Design Principles

Apple's design philosophy centers on three enduring values that guide every decision at every scale.

### 1.1 Clarity
- Content is the interface. Typography, color, and negative space communicate hierarchy without decoration.
- Text is legible at all sizes. Icons are precise and unambiguous.
- Functional elements are intuitively understood without instruction.

### 1.2 Deference
- The UI serves content, never competing with it.
- Translucency, blur, and subtle gradients hint at depth without obscuring information.
- Chrome recedes; content breathes.

### 1.3 Depth
- Layered, translucent surfaces create a realistic sense of spatial hierarchy.
- Gestures and transitions are physics-based, reinforcing the metaphor of navigating real space.
- Feedback is immediate and proportional to the gesture that triggered it.

### 1.4 Additional Pillars (2024–2025 additions)
| Pillar | Description |
|---|---|
| **Personalization** | Tinted icons, customizable widgets, Control Center express individual identity. |
| **Intelligence** | Siri, Writing Tools, and on-device AI surface at the right moment, never intrusively. |
| **Spatial Continuity** | visionOS establishes a new dimension — apps extend beyond glass into physical space. |

---

## 2. Layout

### 2.1 Safe Areas & Margins

Every platform defines a **safe area** — the region free of system chrome (notches, home indicators, menu bars).

```
iOS / iPadOS
  Portrait:  top safe area = 59 pt (Dynamic Island), bottom = 34 pt
  Landscape: leading/trailing safe area = 59 pt on notch side

macOS
  Menu bar clearance: 24 pt from top of screen
  Window chrome: title bar 22 pt (standard), 28 pt (large)

visionOS
  Windows float in space; no physical edges. Use ornaments for controls outside the window.
```

#### Standard Content Margins (iOS)
| Context | Margin |
|---|---|
| Full-width list rows | 16 pt (compact) / 20 pt (regular) |
| Grouped table insets | 20 pt leading, 20 pt trailing |
| Modal card insets | 24 pt horizontal |
| Edge-to-edge hero content | 0 pt (bleeds to safe area) |

### 2.2 Grid System

Apple does not prescribe a single grid but recommends alignment to the **8-pt grid** (multiples of 8 for spacing, 4-pt for micro-adjustments).

```
Base unit:   4 pt
Standard:    8 pt
Comfortable: 16 pt
Generous:    24 pt, 32 pt, 48 pt
```

#### Layout Guides by Platform
| Platform | Primary Grid | Sidebar Width | Content Max Width |
|---|---|---|---|
| **iOS (compact)** | Single column, 16 pt margins | — | Device width |
| **iPadOS (regular)** | 2–3 column split, 20 pt margins | 320 pt (collapsed: 0) | ~1024 pt |
| **macOS** | Flexible multi-column | 200–260 pt | Unrestricted |
| **visionOS** | Window-relative; 1120 × 720 pt default | Ornaments | ~1120 pt |

### 2.3 Adaptive Layouts

Use **size classes** to switch layout configurations:

| Size Class Combination | Device State |
|---|---|
| Compact width × Compact height | iPhone landscape (small) |
| Compact width × Regular height | iPhone portrait (all sizes) |
| Regular width × Compact height | iPad landscape (split view 1/3) |
| Regular width × Regular height | iPad full screen, Mac Catalyst |

**Rules:**
- Never assume a specific device; respond to size class changes.
- Prefer `UISplitViewController` / `NavigationSplitView` for multi-column flows.
- Use `LazyVGrid` / `LazyHGrid` with adaptive columns, not hardcoded column counts.

### 2.4 Scroll Views & Pagination

- **Scroll indicators** appear on the trailing edge; do not hide them unless the surface is purely decorative.
- **Paging** is appropriate for onboarding, photo galleries, and card carousels. Use `tabViewStyle(.page)`.
- **Sticky headers** should be used sparingly; they consume vertical space on compact displays.
- **Pull-to-refresh** is a universal convention on iOS — support it in any list or scroll view that fetches data.

### 2.5 Keyboard Avoidance

When the software keyboard appears, the layout must adjust so the focused element remains visible. Use `scrollDismissesKeyboard(.interactively)` and avoid pinning footers that would be obscured.

---

## 3. Typography

### 3.1 System Fonts

Apple platforms use the **San Francisco (SF)** type family as the system typeface.

| Font | Platform Usage |
|---|---|
| **SF Pro** | iOS, iPadOS, macOS, tvOS |
| **SF Compact** | watchOS; condensed companion to SF Pro |
| **SF Mono** | Code, terminal, fixed-width contexts |
| **New York** | Serif companion; editorial, reading experiences |
| **SF Arabic / Hebrew / Armenian / Georgian** | Localized script companions |

> Always use system fonts via semantic APIs (`Font.body`, `.headline`) rather than by name. The system automatically selects the right optical size, weight, and variant.

### 3.2 Dynamic Type Scale

Dynamic Type is mandatory. Every text element must respond to the user's preferred text size.

#### iOS Text Styles (SF Pro)
| Text Style | Default Size | Weight | Leading |
|---|---|---|---|
| `largeTitle` | 34 pt | Regular | 41 pt |
| `title` | 28 pt | Regular | 34 pt |
| `title2` | 22 pt | Regular | 28 pt |
| `title3` | 20 pt | Regular | 25 pt |
| `headline` | 17 pt | Semibold | 22 pt |
| `body` | 17 pt | Regular | 22 pt |
| `callout` | 16 pt | Regular | 21 pt |
| `subheadline` | 15 pt | Regular | 20 pt |
| `footnote` | 13 pt | Regular | 18 pt |
| `caption` | 12 pt | Regular | 16 pt |
| `caption2` | 11 pt | Regular | 13 pt |

#### Larger Accessibility Sizes (AX1–AX5)
Dynamic Type extends to 5 accessibility sizes beyond the standard 7. Text must reflow gracefully, truncation is forbidden where content is essential, and multiline is always preferred over clipping.

### 3.3 Typography Rules

**Hierarchy**
- Use no more than 3 distinct type styles in a single view.
- Differentiate levels via size and weight before reaching for color.
- Never use color as the *sole* differentiator between heading and body.

**Line Length**
- Optimal: 45–75 characters per line.
- Max: 90 characters. Beyond this, column layout is preferred.

**Tracking & Kerning**
- SF Pro handles optical tracking automatically at each size. Do not override tracking for display sizes (≥ 40 pt) — SF Pro Display applies tighter tracking automatically.
- Increase tracking only in all-caps UI labels (suggested: +0.5 to +1.0 pt at 12–14 pt).

**Alignment**
- Body text: leading (left in LTR) aligned.
- Centered text: acceptable for titles, empty state headers, and short callouts only.
- Never center paragraphs longer than 2 lines.

**Numbers**
- Use **proportional lining** figures for tabular data with `monospacedDigit` modifier.
- Use **oldstyle** figures for inline editorial text (New York).

---

## 4. Color

### 4.1 System Color Roles

Apple's semantic colors adapt automatically to light/dark mode and increase contrast when Increase Contrast is enabled.

#### Semantic Label Colors
| Token | Usage |
|---|---|
| `label` | Primary text |
| `secondaryLabel` | Supporting text |
| `tertiaryLabel` | Placeholder, disabled hints |
| `quaternaryLabel` | Subtlest decorative text |
| `placeholderText` | Text field placeholder |
| `link` | Tappable links |

#### Semantic Fill Colors
| Token | Usage |
|---|---|
| `systemFill` | Thin overlay on content |
| `secondarySystemFill` | Medium overlay |
| `tertiarySystemFill` | Tab bars, search bars |
| `quaternarySystemFill` | Thick overlay, sheet background |

#### Semantic Background Colors
| Token | Usage |
|---|---|
| `systemBackground` | Primary view background |
| `secondarySystemBackground` | Grouped table background |
| `tertiarySystemBackground` | Cards within grouped lists |
| `systemGroupedBackground` | Inset grouped background |
| `secondarySystemGroupedBackground` | Inner cards in grouped layout |
| `tertiarySystemGroupedBackground` | Deeply nested content |

### 4.2 System Accent & Tint

- **System Blue** (`#007AFF` light / `#0A84FF` dark) is the default accent.
- Apps select a **global tint color** applied to interactive elements (buttons, links, selection indicators) system-wide.
- In iOS 18+, users can override the tint from Settings; apps must not assume a fixed tint color for branding.

### 4.3 iOS 18 Adaptive Icon Colors

With user-controlled Home Screen tinting, app icons and widgets must supply:
- **Light** variant
- **Dark** variant
- **Tinted** variant (grayscale base; tinted by the system)

### 4.4 Vibrant Colors

On blurred/translucent backgrounds (materials), use **vibrant** color variants that punch through the blur. In SwiftUI, apply `.foregroundStyle(.primary)` in a `.ultraThinMaterial` context and the system handles vibrancy automatically.

### 4.5 Color Usage Rules

1. **Never use color alone** to convey information. Pair with icon, label, or pattern.
2. Maintain **minimum 4.5 : 1** contrast ratio for body text (WCAG AA). Target **7 : 1** for small text (WCAG AAA).
3. Avoid pure black (`#000000`) on dark backgrounds; use `label` semantic token instead.
4. Status colors (red = destructive, green = success, orange = warning, blue = informational) are universal — do not repurpose them.
5. Test with **Smart Invert** and **Differentiate Without Color** simulator settings.

### 4.6 System Palette Quick Reference

| Name | Light | Dark | Usage |
|---|---|---|---|
| System Blue | `#007AFF` | `#0A84FF` | Default tint, links |
| System Green | `#34C759` | `#30D158` | Success, confirmation |
| System Indigo | `#5856D6` | `#5E5CE6` | Secondary accent |
| System Orange | `#FF9500` | `#FF9F0A` | Warnings, highlights |
| System Pink | `#FF2D55` | `#FF375F` | Media, love, favorites |
| System Purple | `#AF52DE` | `#BF5AF2` | Creative, premium |
| System Red | `#FF3B30` | `#FF453A` | Errors, destructive |
| System Teal | `#5AC8FA` | `#64D2FF` | Informational |
| System Yellow | `#FFCC00` | `#FFD60A` | Caution, stars |
| System Gray | `#8E8E93` | `#8E8E93` | Neutral |
| System Gray 2 | `#AEAEB2` | `#636366` | Secondary neutral |
| System Gray 3 | `#C7C7CC` | `#48484A` | Tertiary |
| System Gray 4 | `#D1D1D6` | `#3A3A3C` | Separator lines |
| System Gray 5 | `#E5E5EA` | `#2C2C2E` | Grouped fill |
| System Gray 6 | `#F2F2F7` | `#1C1C1E` | Background fill |

---

## 5. Materials & Vibrancy

### 5.1 Material Hierarchy

Materials are translucent layering surfaces. They don't use flat colors — they blur and blend with the content behind them, creating depth.

| Material | Blur Radius | Opacity | Use Case |
|---|---|---|---|
| `ultraThinMaterial` | Low | Very low | Overlays on rich content, HUD |
| `thinMaterial` | Medium | Low | Sidebars, transient panels |
| `regularMaterial` | Medium | Medium | Popovers, sheets |
| `thickMaterial` | High | High | Tab bars, toolbars |
| `ultraThickMaterial` | Very high | Very high | Navigation bars, opaque-like |

### 5.2 Vibrancy Effects

Within a material container, text and icons appear **vibrant** — adapted to the content beneath. Three vibrancy modes:
- **Primary** — full opacity foreground
- **Secondary** — slightly reduced prominence
- **Tertiary** — muted, used for timestamps, metadata

### 5.3 Usage Rules

- Do not place a material on top of another material (stacked blur artifacts).
- Materials must be used on surfaces that move; static decorative blurs are discouraged.
- On visionOS, glass is the primary window material. Do not fill glass windows with opaque backgrounds.
- Test materials on a variety of wallpapers to ensure legibility across all conditions.

---

## 6. Iconography & SF Symbols

### 6.1 SF Symbols Overview

SF Symbols 7 (2025) provides **7,000+ symbols** in 9 weights and 3 scales, designed to integrate seamlessly with SF Pro.

| Scale | Use Case |
|---|---|
| Small | Dense UI, toolbars, status bars |
| Medium | Default (inline with body text) |
| Large | Primary actions, empty states |

### 6.2 Symbol Rendering Modes

| Mode | Description |
|---|---|
| **Monochrome** | Single color applied to entire symbol |
| **Hierarchical** | Multi-layer with depth via opacity |
| **Palette** | Each layer gets a distinct color |
| **Multicolor** | Symbol's intended full-color representation |
| **Automatic** | System chooses the best mode per context |

Use **Automatic** unless you have a specific design reason to override.

### 6.3 Symbol Animations (SF Symbols 6+)

Symbols support discrete animations that respect system preferences (Reduce Motion).

| Animation | Description |
|---|---|
| `bounce` | Symbol bounces on appearance or interaction |
| `pulse` | Opacity pulses continuously |
| `variableColor` | Layers animate sequentially (progress, signal) |
| `scale` | Symbol scales up/down |
| `appear` / `disappear` | Symbol fades/scales in or out |
| `replace` | Smooth symbol-to-symbol morphing |

### 6.4 App Icons

All platforms require app icons at specific sizes. Icons must:
- Have a distinct silhouette recognizable at 16×16 pt.
- Use no transparency on iOS/iPadOS (system applies corner radius mask).
- Supply **Light**, **Dark**, and **Tinted** variants (iOS 18+).
- Have no text that becomes illegible below 44×44 pt.
- macOS icons may include a subtle drop shadow and perspective tilt.
- visionOS icons use a layered, parallax-aware 3D stack format.

### 6.5 Custom Icons

When creating custom icons outside SF Symbols:
- Match the visual weight of SF Symbols at the same scale.
- Use a 100×100 pt canvas with padding so the optical fill is ~60–70%.
- Stroke width: ~2–2.5 pt at medium scale.
- Prefer outlined over filled variants for toolbar/navigation contexts.
- Filled variants for selected/active states.

---

## 7. Motion & Animation

### 7.1 Core Principles

Motion on Apple platforms serves a purpose. Every animation must:
1. **Orient the user** — where did content come from? Where did it go?
2. **Provide feedback** — confirm interaction and system state.
3. **Add delight** — feel natural and alive, not mechanical.

Never animate purely for decoration. Decoration adds latency without information.

### 7.2 Spring Animations

Apple platforms use **spring physics** for almost all transitions. Springs communicate that elements have mass and momentum.

```swift
// SwiftUI standard spring presets
.spring()                                // balanced, general use
.spring(response: 0.3, dampingFraction: 0.7)   // snappy
.spring(response: 0.5, dampingFraction: 0.85)  // smooth, natural
.snappy                                  // quick with slight overshoot
.bouncy                                  // playful, more overshoot
.smooth                                  // no overshoot, like ease-out
```

### 7.3 Duration Guidelines

| Interaction Type | Target Duration |
|---|---|
| Micro-interaction (tap feedback) | 80–120 ms |
| Component state change | 150–250 ms |
| Navigation transition | 350–450 ms |
| Modal presentation | 400–500 ms |
| Full-screen transition | 500–600 ms |
| Ambient / idle animation | 1,000–3,000 ms (looping) |

### 7.4 Standard Transitions

#### iOS / iPadOS Navigation
| Transition | Description |
|---|---|
| **Push** | New view slides in from trailing edge; old view slides to leading |
| **Modal sheet** | View rises from bottom with spring (detents: `.medium`, `.large`, `.fraction`) |
| **Cover (full screen)** | View slides up from bottom, covers entirely |
| **Crossfade** | Used within a tab or for content replacement |
| **Zoom** | iOS 18 default navigation transition — view expands from source element |

#### macOS
| Transition | Description |
|---|---|
| **Sheet** | Drops from window chrome edge |
| **Panel** | Floats independently |
| **Popover** | Anchored to source control with arrow |
| **Sidebar reveal** | Slides in from leading edge |

### 7.5 Reduce Motion

Always respect `isReduceMotionEnabled`. Two strategies:
1. **Crossfade** — replace spring transitions with simple opacity fades.
2. **Instant** — for very brief interactions, skip animation entirely.

```swift
@Environment(\.accessibilityReduceMotion) var reduceMotion

var body: some View {
    content
        .animation(reduceMotion ? .none : .spring(), value: isVisible)
}
```

### 7.6 Haptics

| Feedback Type | When to Use |
|---|---|
| `impact(.light)` | Subtle selection, drag begin |
| `impact(.medium)` | Primary action completion |
| `impact(.heavy)` | Destructive action, high-energy moment |
| `notification(.success)` | Task completed successfully |
| `notification(.warning)` | Non-blocking warning |
| `notification(.error)` | Failed action, error state |
| `selection` | Scroll picker tick, segmented control change |

Do not trigger haptics on every tap. Use them for moments of consequence.

---

## 8. Platform-Specific Patterns

### 8.1 iOS

**Navigation Model**
- Primary navigation: `TabView` with 2–5 tabs (5 max in bottom tab bar).
- Hierarchical navigation: `NavigationStack` with `navigationDestination`.
- Avoid nested navigation stacks; prefer flat hierarchies.

**Gestures**
- **Swipe from leading edge** — go back (system-owned; do not intercept).
- **Swipe to delete** — destructive action on list rows.
- **Long press** — reveals context menu (`.contextMenu`).
- **Drag to reorder** — `.onMove` in `List`.
- **Pinch/spread** — zoom on maps, images, PDFs.

**Interaction Areas**
- Minimum tappable target: **44 × 44 pt** (physical size ~9 × 9 mm).
- Use `contentShape(Rectangle())` to expand tap targets on small icons.

**iOS 18 Specific**
- **Tab bar customization** — users can add, remove, and reorder tabs.
- **Control Center widgets** — expose app functionality as `ControlWidget`.
- **RealityKit anchoring** — bring spatial overlays into photos/videos.

---

### 8.2 iPadOS

**Navigation Model**
- Prefer `NavigationSplitView` with sidebar + content + detail columns.
- Support Stage Manager: multiple windows, resizable, overlapping.
- Implement `UIWindowScene` multiple scene support.

**Pointer Support**
- All interactive elements must respond to pointer hover.
- Use `.hoverEffect(.highlight)` for cards and rows.
- Use `.hoverEffect(.lift)` for buttons and controls.
- Pointer cursor changes automatically based on element type.

**Keyboard Support**
- All primary actions must have keyboard shortcuts.
- Register shortcuts via `KeyboardShortcut` in SwiftUI or `UIKeyCommand`.
- Support arrow-key navigation in lists and grids.
- Document all shortcuts in a discoverable `UIKeyCommandDiscoverability` group.

**Drag & Drop**
- Support `UTType`-conformant drag sources and drop targets.
- Multi-item drag is supported; show a drag preview badge count.
- Drop animations should indicate acceptance with a spring scale-up.

---

### 8.3 macOS

**Window Model**
- Use standard window chrome: title bar, traffic lights, toolbar.
- Support full-screen mode (`NSWindowStyleMask.fullScreen`).
- Minimum window size should match the smallest useful layout (typically 640 × 480 pt).

**Menu Bar**
- Every action should be discoverable from the menu bar.
- Follow the standard menu structure: App > File > Edit > View > Window > Help.
- Menu items must have keyboard shortcuts for common actions.
- Use ellipsis (`…`) to indicate an action opens a dialog before executing.

**Toolbar**
- Primary actions in the toolbar; secondary in menus.
- Support user customization of toolbar items.
- Toolbar icons use SF Symbols at small scale.
- On macOS 13+, use `NSToolbarItem` with the unified title bar / toolbar.

**Sidebar & Inspector**
- Sidebar width: 200–300 pt (collapsible).
- Inspector (trailing panel): 260–320 pt.
- Use `.navigationSplitViewStyle(.balanced)` for equal-weight panels.

**Contextual Menus & Popovers**
- Right-click (or Control+click) must reveal a contextual menu with relevant actions.
- Popovers are preferred over floating dialogs for non-modal interactions.

**macOS Specific Sizing**
- Click targets: minimum **14 × 14 pt** (smaller than iOS due to pointer precision).
- Standard control height: 22 pt (regular), 18 pt (small), 15 pt (mini).
- Button corner radius follows the system; do not override per-button radii.

---

### 8.4 visionOS

**Spatial Computing Model**
visionOS apps live in **Shared Space** (alongside other apps, like a windowed OS) or **Full Space** (exclusive, immersive). 

| Mode | Description |
|---|---|
| Shared Space | Default — windows float in the user's environment |
| Full Space (Mixed) | App content + real world visible simultaneously |
| Full Space (Progressive) | Passthrough dims, focus drawn to app content |
| Full Space (Immersive) | Fully virtual environment, no passthrough |

**Windows**
- Default window size: **1120 × 720 pt**.
- Windows are positioned in 3D space; do not hardcode position.
- Glass background is the only window material; avoid opaque fills.
- Window chrome (close/resize) is auto-provided; do not replicate it.

**Volumes**
- 3D content containers anchored in shared space.
- Use `RealityView` for placing entities in a `ModelEntity` hierarchy.
- Volumes have a fixed size, not resizable by the user.

**Ornaments**
- Controls that "hang off" the edge of a window (below, to the side).
- Used for toolbars and tab bars so they don't consume window content area.
- Tabs implemented as vertical tab strip ornament on the leading edge.

**Depth & Hover Effects**
- Interactive elements must respond to user gaze (hover state).
- Use `.hoverEffect()` to add depth lift and highlight on hover.
- Elements "push forward" (z-depth +10–40 pt) on hover.

**Input Model**
- **Gaze + Pinch** — primary interaction (index + thumb = tap).
- **Direct touch** — for elements within arm's reach.
- **Trackpad / game controller** — optional pointer/spatial input.
- No keyboard is required for core flows; keyboard is optional.

**visionOS Typography**
- Standard window reading distance: ~1.5–2 m.
- Minimum legible point size: **17 pt** at 1 m viewing distance.
- Prefer `.body` and `.headline` for window UI.
- `largeTitle` and `title` are appropriate for immersive entry points.

---

## 9. Accessibility Foundations

### 9.1 VoiceOver

- All interactive elements must have a meaningful accessibility label.
- Labels: what the element **is** (noun), not what it **does**.
- Hints: what will happen if activated (verb phrase), only when non-obvious.
- Traits: `.isButton`, `.isHeader`, `.isSelected`, `.updatesFrequently`.
- Group related elements with `accessibilityElement(children: .combine)`.
- Custom containers: use `accessibilityElement(children: .contain)` with `.accessibilityLabel`.

### 9.2 Dynamic Type Compliance

- All labels, buttons, and body text must use system text styles.
- Layouts must reflow — no truncation of essential content at AX5.
- Images and icons can remain fixed size; only text must scale.
- Horizontal stack → vertical stack at large text sizes via `@ScaledMetric` or `ViewThatFits`.

### 9.3 Color & Contrast

- Text contrast ≥ 4.5:1 (WCAG AA). Small decorative text: ≥ 3:1.
- Use `isHighContrastEnabled` to offer stronger borders or fills when Increase Contrast is on.
- Never use color alone to convey status.

### 9.4 Motion Sensitivity

- Parallax, spring overshoot, and ambient animations must be removed when Reduce Motion is enabled.
- Cross-fade is the universal fallback transition.

### 9.5 Input Flexibility

- All touch targets reachable with Switch Control.
- All primary actions have keyboard equivalents on iPadOS/macOS.
- Voice Control labels must match visible text or have a pronunciation hint.

### 9.6 Focus Management

- On macOS and iPadOS (with keyboard), focus must be visible (system focus ring).
- Do not suppress focus rings. Customize only via `focusedBorder` in controlled contexts.
- On view appearance, set initial focus to the primary interactive element.

---

*Last updated: June 2025 · Source: Apple Human Interface Guidelines, WWDC 2024–2025*
