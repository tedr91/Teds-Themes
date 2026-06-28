# Apple Design System — theme.md
> Aligned to Apple Human Interface Guidelines · iOS 18 / iPadOS 18 / macOS Sequoia / visionOS 2

---

## Table of Contents

1. [Theming Architecture](#1-theming-architecture)
2. [Light Mode](#2-light-mode)
3. [Dark Mode](#3-dark-mode)
4. [Tint & Accent Color System](#4-tint--accent-color-system)
5. [iOS 18 Adaptive Icon Theming](#5-ios-18-adaptive-icon-theming)
6. [High Contrast Mode](#6-high-contrast-mode)
7. [Accessibility Theming](#7-accessibility-theming)
8. [Platform Themes](#8-platform-themes)
9. [App-Level Theming](#9-app-level-theming)
10. [Theme Checklist](#10-theme-checklist)

---

## 1. Theming Architecture

Apple's theming system is **adaptive by default**. The system maintains a hierarchy of theme contexts, each of which can override the one above it:

```
System Appearance (Light / Dark)
    └─ Increase Contrast (Normal / High)
        └─ App Tint Color
            └─ Per-View Overrides (.colorScheme, .preferredColorScheme)
                └─ Per-Element Overrides (.foregroundStyle, .background)
```

### 1.1 Core Theming Philosophy

**Respond, don't dictate.** Apple's design philosophy requires apps to respect system settings. An app may express a brand identity through its tint color, typography choices, and custom assets — but it must defer to system appearance preferences.

| Layer | Who Controls | What It Affects |
|---|---|---|
| Appearance (Light/Dark) | User via System Settings | All semantic colors, materials |
| Contrast | User via Accessibility Settings | Border weights, fill opacity |
| Tint | App developer (user may override on iOS 18+) | Interactive element accent |
| Typography | User via Text Size Settings | Dynamic Type scale |
| Motion | User via Accessibility Settings | All animations and transitions |

### 1.2 SwiftUI Environment Propagation

In SwiftUI, the theme environment flows down the view tree automatically. Override at the closest ancestor needed:

```swift
// App-level: set tint
WindowGroup {
    ContentView()
        .tint(Color("AppAccent"))
}

// View-level: force light appearance
MyView()
    .preferredColorScheme(.light)

// Component-level: override color scheme
SidebarView()
    .colorScheme(.dark)  // Force dark — use only for truly dark surfaces (video, maps)
```

---

## 2. Light Mode

Light mode is the **default appearance** on all Apple platforms. It uses light backgrounds, dark labels, and moderately saturated tint colors.

### 2.1 Light Mode Color Values

| Role | Color | Hex |
|---|---|---|
| Primary Background | White | `#FFFFFF` |
| Secondary Background | System Gray 6 | `#F2F2F7` |
| Tertiary Background | White | `#FFFFFF` |
| Primary Label | Black | `rgba(0,0,0,1.00)` |
| Secondary Label | Dark Gray | `rgba(60,60,67,0.60)` |
| Separator | Light Gray | `#C6C6C8` |
| Default Tint | System Blue | `#007AFF` |
| Destructive | System Red | `#FF3B30` |

### 2.2 Light Mode Surface Hierarchy

```
Level 0 — Page Background:    #F2F2F7  (systemGroupedBackground)
Level 1 — Card / Section:     #FFFFFF  (secondarySystemGroupedBackground)
Level 2 — Inner Element:      #F2F2F7  (tertiarySystemGroupedBackground)
Level 3 — Input / Fill:       rgba(118,118,128,0.12)  (tertiaryFill)
```

### 2.3 Light Mode Shadow Usage

Shadows are active in light mode to separate elevated surfaces from flat backgrounds.

| Surface | Shadow Token | Visibility |
|---|---|---|
| Cards on white | `shadow.md` | Subtle but important |
| Popovers | `shadow.xl` | Clearly elevated |
| Navigation bar | None (separator line) | Separator on scroll |
| Tab bar | None (material blur) | Material handles separation |
| Modals / Sheets | `shadow.xl` | Always |

---

## 3. Dark Mode

Dark mode inverts the surface hierarchy: **dark backgrounds, light labels, slightly brighter tint colors**. It is not simply a color inversion — it is a fully designed alternate theme.

### 3.1 Dark Mode Color Values

| Role | Color | Hex |
|---|---|---|
| Primary Background | Near-Black | `#000000` |
| Secondary Background | System Gray 6 (dark) | `#1C1C1E` |
| Tertiary Background | System Gray 5 (dark) | `#2C2C2E` |
| Primary Label | White | `rgba(255,255,255,1.00)` |
| Secondary Label | Light Gray | `rgba(235,235,245,0.60)` |
| Separator | Dark Gray | `#38383A` |
| Default Tint | Brighter Blue | `#0A84FF` |
| Destructive | Brighter Red | `#FF453A` |

### 3.2 Dark Mode Surface Hierarchy

```
Level 0 — Page Background:    #000000  (systemBackground)
Level 1 — Card / Section:     #1C1C1E  (secondarySystemBackground)
Level 2 — Inner Element:      #2C2C2E  (tertiarySystemBackground)
Level 3 — Input / Fill:       rgba(118,118,128,0.24)  (tertiaryFill dark)
Level 4 — Elevated Panel:     #3A3A3C  (quaternary, subtle top layer)
```

Note: Elevation in dark mode is expressed through **lightness** (brighter = closer to user), not shadow. Shadows are invisible on black backgrounds.

### 3.3 Dark Mode Shadow Usage

| Surface | Shadow Token | Dark Mode Override |
|---|---|---|
| Cards | `shadow.md` | None — rely on background level contrast |
| Popovers | `shadow.xl` | Subtle — `rgba(0,0,0,0.5)` border may help |
| Navigation bar | None | Material handles appearance |
| Modals / Sheets | `shadow.xl` | Very subtle; background contrast more important |

### 3.4 Dark Mode Typography Adjustments

System fonts automatically adjust **optical weight** slightly in dark mode — letters appear slightly thinner against dark backgrounds due to halation (light blooming). The system handles this automatically; do not override font weights for dark mode.

- **Do not** use `.bold` text in dark mode where `.semibold` is specified — it can feel heavy.
- **Do not** force black text (`primary.black`) — always use `color.label.primary` which adapts.

### 3.5 Dark Mode Asset Requirements

Every visual asset used in the app must have a dark mode variant:

| Asset Type | Requirement |
|---|---|
| Images (photos) | If color-processed (tinted), provide both variants |
| Illustrations / decorative | Provide dark variant in `.xcassets` |
| Icons (custom, non-SF) | Provide dark variant or use template rendering |
| Backgrounds / patterns | Provide dark variant |
| Maps | Use `.standard(colorScheme: .dark)` map style |
| Web content (WKWebView) | Support `prefers-color-scheme: dark` in CSS |

### 3.6 Detecting Current Appearance

```swift
// SwiftUI
@Environment(\.colorScheme) var colorScheme
var isDark: Bool { colorScheme == .dark }

// UIKit
traitCollection.userInterfaceStyle == .dark

// AppKit
NSApp.effectiveAppearance.bestMatch(from: [.darkAqua, .aqua]) == .darkAqua
```

---

## 4. Tint & Accent Color System

### 4.1 What Tint Controls

The **app tint color** (called "accent color" in some contexts) automatically applies to:
- Button labels (`.borderless`, `.bordered` styles)
- Navigation bar back button and action buttons
- Toggle switch (on state)
- Segmented control selection
- List selection highlight
- Link text
- Toolbar icons (selected state)
- Progress indicators

### 4.2 Setting the App Tint

**SwiftUI (recommended)**
```swift
@main
struct MyApp: App {
    var body: some Scene {
        WindowGroup {
            ContentView()
        }
        .tint(Color("BrandBlue"))   // or Color(hex: "#007AFF")
    }
}
```

**Asset Catalog (applies to all targets)**
- In `Assets.xcassets`, create a Color Set named **`AccentColor`**.
- Set Light and Dark appearance variants.
- This is automatically used by the system as the app-wide tint.

### 4.3 Tint Color Design Guidelines

**Contrast requirements**
- Tint on white background: minimum 3:1 contrast for large interactive elements; 4.5:1 for small text.
- Tint on black background (dark mode): same constraints with a typically lighter/brighter value.

**Saturation & Perception**
- Tint colors should be **moderately saturated** — avoid pure, fully saturated colors that appear harsh.
- In dark mode, shift the tint **~10–20% lighter** and ~5% less saturated to maintain vibrancy.

| Light Tint | Hex | Dark Tint | Hex |
|---|---|---|---|
| Brand Blue | `#007AFF` | Brand Blue (dark) | `#0A84FF` |
| Brand Green | `#28A745` | Brand Green (dark) | `#30D158` |
| Brand Purple | `#7B2FBE` | Brand Purple (dark) | `#BF5AF2` |
| Brand Orange | `#E6820A` | Brand Orange (dark) | `#FF9F0A` |

### 4.4 iOS 18+ User Tint Override

In iOS 18, users can set a **global tint color** from Wallpaper settings that overrides the Home Screen and app icons. Apps must:
- Never rely on a specific tint color for **meaning** (e.g., "green = allowed, red = blocked"). Always pair with icons or text.
- Test with multiple system tints — use the "Light/Dark Tint" preview in Simulator.
- Provide tinted icon variants (see Section 5).

---

## 5. iOS 18 Adaptive Icon Theming

### 5.1 Icon Variant Requirements

iOS 18 introduces three required icon variants for optimal experience:

| Variant | Description | When Applied |
|---|---|---|
| **Light** | Standard full-color design on white/light background | Light mode Home Screen |
| **Dark** | Dark background version; inverted or redesigned palette | Dark mode Home Screen |
| **Tinted** | Grayscale base; system applies the user's chosen tint | User-enabled tinted mode |

### 5.2 Light Variant

The light variant is your existing icon. Designed on a light or mid-tone background.

**Best practices:**
- Use a clear, visually distinct silhouette.
- Avoid using red/green alone for meaning (colorblind consideration).
- Avoid fine details below 8 pt — lost at small sizes.

### 5.3 Dark Variant

The dark variant should feel like a natural evolution of the light version — not just a color inversion.

**Design approach:**
- Replace light backgrounds with dark backgrounds (typically `#1C1C1E` or equivalent in your brand palette).
- Adjust foreground elements to remain vivid on the dark background.
- Lighten any elements that appear dark on light but need to be bright on dark.
- Test on pure black (`#000000`) OLED screens — ensure the icon doesn't disappear into the background.

**Contrast requirement:** The icon should have sufficient contrast against `#000000` — minimum 3:1 contrast ratio at the icon silhouette edges.

### 5.4 Tinted Variant

The tinted variant is a **monochromatic grayscale** rendering. The system applies the user's chosen tint as a color overlay.

**Design approach:**
- Convert your full-color icon to grayscale, maintaining relative lightness relationships.
- Ensure the key shapes and silhouette of the icon remain clear in grayscale.
- Lighter areas will reflect the tint color most strongly; darker areas absorb it.
- Test your tinted icon at several popular tint colors (blue, pink, purple, yellow) to ensure it reads well across the range.

**Asset delivery:** Provide all three variants in a single layered `.xcassets` App Icon set using the "Appearance" option with Light, Dark, and Tinted rows.

### 5.5 Widget Theming (iOS 18)

Widgets also participate in the adaptive icon system:

- **Accented widgets** receive a color overlay from the user's tint. Provide a widget rendering with clear foreground/background separation so the system can apply the tint correctly.
- **Full-color widgets** display as designed.
- Use `containerBackground(for:)` API to set the widget background color that participates in the system material.

---

## 6. High Contrast Mode

When the user enables **Increase Contrast** in Accessibility Settings, all adaptive semantic colors automatically shift to higher-contrast variants. Apps must also manually respond where custom values are used.

### 6.1 Automatic Contrast Increases

| Element | Normal | High Contrast |
|---|---|---|
| Separator lines | `rgba(60,60,67,0.29)` | `rgba(60,60,67,0.65)` |
| Secondary label | `rgba(60,60,67,0.60)` | `rgba(60,60,67,0.90)` |
| Fill colors | `rgba(120,120,128,0.20)` | `rgba(120,120,128,0.40)` |
| System backgrounds | As-is | Slight contrast boost |

### 6.2 Manual High Contrast Responses

```swift
@Environment(\.accessibilityReduceTransparency) var reduceTransparency
@Environment(\.colorSchemeContrast) var contrast

var body: some View {
    MyView()
        .background(
            contrast == .increased
                ? Color.systemBackground          // opaque fallback
                : Color.systemBackground.opacity(0.85)  // translucent default
        )
}
```

### 6.3 Border Emphasis

In high contrast mode, add visible borders to components that rely solely on fill differentiation:

| Component | Normal | High Contrast |
|---|---|---|
| Cards | No border (shadow only) | 1 pt `color.separator.opaque` border |
| Buttons | No border | 1 pt tint-colored border |
| Text fields | Subtle fill | 1 pt `color.separator.opaque` border |
| Toggle (off) | Gray track | 1 pt gray border on track |

### 6.4 Reduce Transparency

When **Reduce Transparency** is enabled, materials (blur effects) are replaced with flat, opaque colors:

| Material | Opaque Fallback (Light) | Opaque Fallback (Dark) |
|---|---|---|
| `ultraThinMaterial` | `#EBEBF0` | `#2C2C2E` |
| `thinMaterial` | `#E8E8ED` | `#323234` |
| `regularMaterial` | `#D8D8DC` | `#3D3D3F` |
| `thickMaterial` | `#C8C8CC` | `#474749` |
| Navigation bar | `#F8F8F8` | `#1C1C1E` |
| Tab bar | `#F8F8F8` | `#1C1C1E` |

Check: `@Environment(\.accessibilityReduceTransparency) var reduceTransparency`

---

## 7. Accessibility Theming

### 7.1 Dynamic Type Theming

Dynamic Type is part of the theme. Every text element participates in the user's chosen text size preference.

**Scaling custom values with `@ScaledMetric`**
```swift
@ScaledMetric(relativeTo: .body) var iconSize: CGFloat = 24

Image(systemName: "star")
    .font(.system(size: iconSize))
```

**Responsive layouts with `ViewThatFits`**
```swift
ViewThatFits(in: .horizontal) {
    HStack {
        label
        Spacer()
        value
    }
    VStack(alignment: .leading) {
        label
        value
    }
}
```

### 7.2 Bold Text

When Bold Text is enabled in Accessibility Settings, the system increases font weight by one step (Regular → Medium, Medium → Semibold, etc.) automatically for system text styles. Custom fonts must respond manually:

```swift
@Environment(\.legibilityWeight) var legibilityWeight

var fontWeight: Font.Weight {
    legibilityWeight == .bold ? .bold : .regular
}
```

### 7.3 Button Shapes

When **Button Shapes** is enabled, system buttons gain a visible border or underline to make interactive regions clearer. Custom button implementations must add a visible shape:

```swift
@Environment(\.accessibilityShowButtonShapes) var showButtonShapes

Button("Save") { save() }
    .overlay(
        showButtonShapes
            ? RoundedRectangle(cornerRadius: 8).strokeBorder(Color.primary.opacity(0.3), lineWidth: 1)
            : nil
    )
```

### 7.4 On/Off Labels

When **On/Off Labels** is enabled, Toggle controls show "I" (on) and "O" (off) symbols on the switch thumb. This is handled automatically by system Toggle — custom toggle implementations must add this manually.

### 7.5 Smart Invert vs. Classic Invert

| Invert Mode | How It Works | What Apps Must Do |
|---|---|---|
| **Smart Invert** | Inverts UI colors; skips photos, videos, app icons | Opt images out: `.accessibilityIgnoresInvertColors(true)` |
| **Classic Invert** | Inverts everything, including photos | Gracefully degraded; cannot prevent |

Always mark media (photos, video thumbnails, maps) with `.accessibilityIgnoresInvertColors(true)`.

---

## 8. Platform Themes

### 8.1 iOS Theme

**Character:** Clean, bright, gesture-forward. Minimal chrome. Content fills the screen.

| Attribute | Value |
|---|---|
| Default background | `systemBackground` (`#FFFFFF` light / `#000000` dark) |
| Navigation style | Large title → inline title on scroll |
| Primary typeface | SF Pro, Dynamic Type |
| Interaction | Touch; gestures primary |
| Corner radius style | Rounded (12–20 pt) |
| Shadow usage | Minimal; materials handle depth |
| Animation style | Spring physics; snappy |

**iOS-Specific Theming Rules**
- Status bar style must match the background beneath it. Light content on dark navbars, dark content on light.
- The home indicator area (bottom of screen) must not be obscured by interactive controls.
- The Dynamic Island is system-reserved. Never overlap it with custom content.

---

### 8.2 iPadOS Theme

**Character:** Desktop-like density with touch warmth. Sidebars, multi-column, pointer support.

| Attribute | Value |
|---|---|
| Default background | Same as iOS |
| Navigation style | Split view; sidebar-dominant |
| Primary typeface | SF Pro (same scale as iOS) |
| Interaction | Touch + optional pointer + keyboard |
| Corner radius style | Same as iOS |
| Shadow usage | Slightly more visible (larger canvas) |
| Animation style | Same spring physics |

**iPadOS-Specific Theming Rules**
- Pointer hover states must be implemented — `.hoverEffect()` for all interactive elements.
- Keyboard shortcut indicators should appear in menus and tooltips.
- Support light and dark mode independently from the host iPhone; iPadOS has its own appearance setting.

---

### 8.3 macOS Theme

**Character:** Dense, precise, information-rich. Chrome is present and serves power users.

| Attribute | Value |
|---|---|
| Default background | `windowBackgroundColor` (light gray / dark gray) |
| Navigation style | Sidebar + content + inspector |
| Primary typeface | SF Pro at macOS scale (13 pt body) |
| Interaction | Mouse pointer; keyboard-primary |
| Corner radius style | Smaller (6–10 pt); consistent with macOS chrome |
| Shadow usage | Present on popovers, panels, menus |
| Animation style | Shorter, snappier; macOS users are power users |

**macOS Appearance Types**
| Appearance | Description |
|---|---|
| `NSAppearance.Name.aqua` | Standard light mode |
| `NSAppearance.Name.darkAqua` | Standard dark mode |
| `NSAppearance.Name.vibrantLight` | Light vibrancy (panels on light) |
| `NSAppearance.Name.vibrantDark` | Dark vibrancy (panels on dark) |
| `NSAppearance.Name.accessibilityHighContrastAqua` | High contrast light |
| `NSAppearance.Name.accessibilityHighContrastDarkAqua` | High contrast dark |

**macOS-Specific Theming Rules**
- Window backgrounds use `NSColor.windowBackgroundColor` (auto-adaptive), not hardcoded hex.
- Sidebar backgrounds use `NSColor.controlBackgroundColor` for proper vibrancy context.
- Avoid forcing `.vibrantDark` context on the main content area — reserve for truly dark surfaces.
- Menu bar extras must work in both light and dark menu bars (provide template images).

---

### 8.4 visionOS Theme

**Character:** Spatial, immersive, glass-forward. The environment is the theme.

| Attribute | Value |
|---|---|
| Default window | Glass (translucent, blurred) |
| Background color | Never opaque; always glass |
| Primary typeface | SF Pro, same semantic styles |
| Interaction | Gaze + pinch; direct touch; indirect pointer |
| Corner radius | Large (20–24 pt) for windows; system-managed |
| Shadow usage | None (glass has inherent depth) |
| Animation style | Physics-forward; spatial transitions |

**visionOS Glass Material**
```swift
// Window glass is automatic — set in scene configuration
WindowGroup {
    ContentView()
}
.windowStyle(.plain)  // removes glass — only for immersive spaces
// Default: glass is always on for shared space windows
```

**visionOS-Specific Theming Rules**
- **Never** fill a glass window with an opaque background. The environment is visible through glass — this is a core design principle.
- Color used in visionOS should be functional, not decorative (glass mutes saturation).
- Tint should be used for interactive element highlighting only; backgrounds should remain glass.
- Depth (z-axis) is a theming dimension: use hover depth (`+10–40 pt`) and ornament depth to create spatial relationships.
- All text must meet legibility standards at ~1.5–2 m viewing distance. Body text minimum: 17 pt.

---

### 8.5 watchOS Theme

**Character:** Ultra-compact, glanceable, wrist-aware.

| Attribute | Value |
|---|---|
| Default background | Pure black (`#000000`) — OLED efficiency |
| Foreground | White and tint color |
| Primary typeface | SF Compact (condensed) |
| Interaction | Digital Crown; tap; double-tap (watchOS 10+) |
| Corner radius | Matches watch face shape (system-managed) |
| Animation style | Minimal; power-aware |

**watchOS Theming Rules**
- Use black backgrounds exclusively — energy conservation on OLED is critical.
- Content must be legible at a glance (~1–2 s reading time per view).
- Complication design must work in full color, tinted, and grayscale rendering modes.

---

## 9. App-Level Theming

### 9.1 Brand Color Integration

Integrating a brand color into the Apple design system:

**Step 1: Verify contrast**
- Brand color on white: check contrast ratio with a tool (e.g., WebAIM Contrast Checker).
- Brand color on black: check contrast ratio for dark mode usage.
- If contrast fails, derive a **light mode** and **dark mode** variant that meet 3:1 (large) and 4.5:1 (small text).

**Step 2: Create Asset Catalog color**
- `Assets.xcassets` → New Color Set → name: `AccentColor`
- Set Any Appearance: brand color (light)
- Set Dark Appearance: brand color (dark variant)

**Step 3: Create semantic tokens**
```swift
extension Color {
    static let brandPrimary = Color("BrandPrimary")    // from asset catalog
    static let brandSecondary = Color("BrandSecondary")
    static let brandOnPrimary = Color.white              // text on brandPrimary
}
```

**Step 4: Apply at app root**
```swift
WindowGroup {
    RootView()
}
.tint(.brandPrimary)
```

### 9.2 Typography Brand Customization

Apple allows a custom typeface to coexist with SF Pro, provided Dynamic Type support is maintained.

```swift
extension Font {
    static func brandFont(_ style: Font.TextStyle, weight: Font.Weight = .regular) -> Font {
        // Use scaled custom font
        return Font.custom("YourBrandFont-Regular", size: UIFontMetrics(forTextStyle: style.uiStyle).scaledValue(for: style.defaultSize), relativeTo: style)
    }
}
```

**Rules for custom typefaces:**
- Must support Dynamic Type scaling via `UIFontMetrics`.
- Must include Regular, Medium, Semibold, and Bold weights minimum.
- Must pass the same legibility standards as SF Pro at all text sizes.
- Reserve the custom typeface for **display/hero** elements; use SF Pro for UI chrome.
- Never use a custom typeface in system-provided chrome (navigation bar title is overridable, but menu bar items are not on macOS).

### 9.3 Custom Icon Sets

If your app uses custom icons beyond SF Symbols:

- Match the **visual weight** of SF Symbols at the equivalent scale.
- Provide both **outlined** (default/unselected) and **filled** (selected/active) variants.
- Include **template image** rendering mode in the asset catalog so the system can apply tint color.
- Provide 1× and 2× (and 3× for iOS) renderings.

### 9.4 Per-View Theming

Occasionally a view needs to override the global theme (e.g., a video player, full-screen map, onboarding).

```swift
// Force dark for a media view
VideoPlayerView()
    .colorScheme(.dark)
    .preferredColorScheme(.dark)

// Override tint for a section
PromotionalBannerView()
    .tint(.brandSecondary)

// Override navigation bar appearance
.toolbarBackground(.brandPrimary, for: .navigationBar)
.toolbarColorScheme(.dark, for: .navigationBar)  // Ensure white icons/title
```

**Caution:** Per-view overrides must be purposeful. Overriding appearance without reason breaks user expectations and accessibility.

### 9.5 Theming for Multiple Platforms (SwiftUI)

SwiftUI's adaptive system handles most platform theming automatically. Where platform-specific overrides are needed:

```swift
var body: some View {
    content
#if os(iOS)
        .navigationBarTitleDisplayMode(.large)
#elseif os(macOS)
        .frame(minWidth: 640, minHeight: 400)
#endif
}

// Environment-based conditional
@Environment(\.horizontalSizeClass) var hSizeClass

var isCompact: Bool { hSizeClass == .compact }
```

---

## 10. Theme Checklist

Use this checklist before shipping any screen or component.

### 10.1 Appearance

- [ ] All text uses semantic color tokens (`label`, `secondaryLabel`, etc.)
- [ ] All backgrounds use semantic background tokens
- [ ] All fills use semantic fill tokens
- [ ] Separator lines use `separator.nonOpaque` or `separator.opaque`
- [ ] No hardcoded hex colors used directly in view code (all through tokens/asset catalog)
- [ ] App tint set in root via `.tint()`
- [ ] `AccentColor` defined in asset catalog with light and dark variants

### 10.2 Dark Mode

- [ ] All views reviewed in Dark Mode simulator
- [ ] No hardcoded white or black used for text/backgrounds
- [ ] Shadow opacity set to 0 in dark mode (or handled by semantic tokens)
- [ ] All custom images have dark mode variants in asset catalog
- [ ] Web content (WKWebView) supports `prefers-color-scheme: dark`
- [ ] Map views use `.colorScheme`-responsive map style
- [ ] Charts use semantic colors that adapt to dark mode

### 10.3 High Contrast

- [ ] All text meets 4.5:1 contrast in both light and dark modes
- [ ] All interactive elements meet 3:1 contrast for large targets
- [ ] `colorSchemeContrast` environment value read where needed
- [ ] Cards/fields gain visible borders in high contrast
- [ ] `accessibilityReduceTransparency` environment value read; opaque fallbacks provided for materials

### 10.4 iOS 18 Icon Variants

- [ ] Light icon variant defined
- [ ] Dark icon variant defined (not just color-inverted)
- [ ] Tinted (grayscale) icon variant defined
- [ ] Widget accented variant defined (if app has widgets)
- [ ] Tinted icon tested against at least 5 popular tint colors

### 10.5 Platform

**iOS / iPadOS**
- [ ] Status bar style matches background
- [ ] Dynamic Island and home indicator areas clear of interactive controls
- [ ] Tint color works in both light and dark, and against potential user tint override

**macOS**
- [ ] Window uses `windowBackgroundColor`, not hardcoded hex
- [ ] Sidebar uses `controlBackgroundColor`
- [ ] Menu bar extra icon is a template image (works on any menu bar color)
- [ ] Full-screen mode tested

**visionOS**
- [ ] No opaque backgrounds in glass windows
- [ ] All interactive elements have hover depth effect
- [ ] Typography meets 17 pt minimum at 1.5 m viewing distance
- [ ] All views tested in Simulator with Shared Space and Full Space modes

**watchOS**
- [ ] Background is pure black
- [ ] Content is legible at a glance
- [ ] Complication works in full color, tinted, and grayscale

### 10.6 Accessibility

- [ ] All text uses Dynamic Type (system text styles or scaled custom fonts)
- [ ] Layouts reflow at AX5 (largest accessibility text size)
- [ ] `legibilityWeight` environment checked for bold text settings
- [ ] `accessibilityShowButtonShapes` handled for custom buttons
- [ ] Media images marked `.accessibilityIgnoresInvertColors(true)`
- [ ] Reduce Motion: animations replaced with crossfade or removed
- [ ] Reduce Transparency: materials replaced with opaque fallbacks

---

*Last updated: June 2025 · Source: Apple Human Interface Guidelines, Apple Accessibility Documentation, WWDC 2024–2025*
