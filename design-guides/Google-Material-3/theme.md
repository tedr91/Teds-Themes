# Material Design 3 — Theming System

> **Version:** Material Design 3 (M3) / Material You · Updated June 2026  
> **Source:** [m3.material.io/styles](https://m3.material.io/styles) · Google LLC  
> **Scope:** Dynamic color, static theming, dark mode, brand customization, Material Theme Builder

---

## Table of Contents

1. [Theming Overview](#theming-overview)
2. [Dynamic Color](#dynamic-color)
   - [How Dynamic Color Works](#how-dynamic-color-works)
   - [Source Color Strategies](#source-color-strategies)
   - [Wallpaper-Based Color (Android)](#wallpaper-based-color-android)
   - [In-App Content-Based Color](#in-app-content-based-color)
   - [Dynamic Color on Web](#dynamic-color-on-web)
3. [Static Theming](#static-theming)
   - [Baseline Scheme](#baseline-scheme)
   - [Custom Static Themes](#custom-static-themes)
   - [Material Theme Builder](#material-theme-builder)
4. [Color Customization](#color-customization)
   - [Brand Color Integration](#brand-color-integration)
   - [Color Harmonization](#color-harmonization)
   - [Fixed Accent Colors](#fixed-accent-colors)
   - [Custom Color Groups](#custom-color-groups)
5. [Dark Mode Theming](#dark-mode-theming)
   - [Automatic Dark Theme](#automatic-dark-theme)
   - [Forced Dark Mode vs. True Dark Theme](#forced-dark-mode-vs-true-dark-theme)
   - [Dark Theme Best Practices](#dark-theme-best-practices)
6. [Typography Theming](#typography-theming)
7. [Shape Theming](#shape-theming)
8. [Motion Theming (M3 Expressive)](#motion-theming)
9. [Component-Level Theming](#component-level-theming)
10. [Multi-Brand & White-Label Theming](#multi-brand--white-label-theming)
11. [Platform-Specific Theming](#platform-specific-theming)
    - [Android (Compose)](#android-compose)
    - [Web (material-web)](#web-material-web)
    - [Flutter](#flutter)
12. [Adaptive & Foldable Theming](#adaptive--foldable-theming)
13. [Theming Checklist](#theming-checklist)

---

## Theming Overview

Material Design 3 introduces **Material You** — a theming system that puts the user at the center of personalization. The system has three layers:

```
┌──────────────────────────────────────────────────────────┐
│                   PRODUCT THEME                          │
│  Brand colors, typography, shape personality             │
├──────────────────────────────────────────────────────────│
│                   SYSTEM THEME                           │
│  User's chosen wallpaper / OS appearance settings        │
├──────────────────────────────────────────────────────────│
│                   BASELINE THEME                         │
│  Default Material Design 3 static color scheme           │
└──────────────────────────────────────────────────────────┘
```

The **baseline** is always the fallback. The **product theme** sets brand personality. **Dynamic color** from the system layer adapts the palette to each individual user — creating a unique, personalized experience that still feels unmistakably like your product.

### Theming Subsystems

| Subsystem | What It Controls | Customizable? |
|---|---|---|
| **Color scheme** | All semantic color roles | Yes — brand seed or dynamic |
| **Typography** | Type scale, typefaces, weights | Yes — full replacement |
| **Shape** | Corner radius scale | Yes — adjust per component |
| **Elevation** | Surface tint + shadow levels | Partial — tint tied to primary |
| **Motion** | Durations, easing, spring physics | Yes — token-level control |
| **Icon style** | Symbol weight, fill, grade | Yes — per context |

---

## Dynamic Color

### How Dynamic Color Works

Dynamic color automatically generates a complete, accessible M3 color scheme from a **single source color**. The pipeline:

```
Source Color (HEX, image pixel)
        │
        ▼
    HCT Analysis
   (Hue, Chroma, Tone)
        │
        ▼
  Key Color Extraction
 (Primary, Secondary,
  Tertiary, Neutral,
  Neutral-Variant)
        │
        ▼
  Tonal Palette Generation
  (13 tones × 5 palettes)
        │
        ▼
  Color Role Assignment
  (Light + Dark schemes)
        │
        ▼
  Component Token Mapping
  (Full UI coverage)
```

#### Key Color Derivation

From the primary source color, the system algorithmically derives:

| Key | Derivation from Source |
|---|---|
| **Primary** | Directly from source; hue preserved |
| **Secondary** | Same hue family; reduced chroma (~16) |
| **Tertiary** | Hue rotated +60°; chroma ~24 |
| **Neutral** | Source hue; very low chroma (~4) |
| **Neutral-Variant** | Source hue; low chroma (~8) |

This ensures all five palettes feel harmonically related to the source, not randomly generated.

#### Accessibility Guarantee

Because M3's color roles are always drawn from specific tonal steps (e.g., P-40 for primary in light mode, P-80 in dark mode), the contrast relationship between paired roles is **mathematically guaranteed** regardless of what hue the source provides. The HCT model's perceptual uniformity ensures a tone-40 color has consistent luminance across all hues.

---

### Source Color Strategies

M3 Expressive (2026) extends dynamic color to support **two seed colors**: primary and tertiary can each have independent seeds, allowing richer and more expressive palettes while the secondary and neutral palettes are still derived automatically.

| Strategy | How It Works | Best For |
|---|---|---|
| **Single seed** | One color generates all 5 palettes | Wallpaper-based color, simple branding |
| **Dual seed** | Primary + Tertiary seeds; secondary derived | Brand colors with two distinct accents |
| **Static brand** | Hand-picked colors; no dynamic generation | Strict brand consistency |
| **Harmonized** | Static colors blended toward dynamic scheme | Third-party content within a dynamic app |

---

### Wallpaper-Based Color (Android)

On Android 12+, the system reads the user's wallpaper and generates a `WallpaperColors` object. Material You's `DynamicColorsOptions` API consumes this to populate the app's theme.

```kotlin
// Apply dynamic colors in Application.onCreate()
DynamicColors.applyToActivitiesIfAvailable(this)

// Or apply to a specific activity
DynamicColors.applyToActivityIfAvailable(
    activity,
    DynamicColorsOptions.Builder()
        .setPrecondition { activity, _ ->
            // optionally gate by user preference
            userHasEnabledDynamicColor
        }
        .build()
)
```

When the system applies dynamic colors, it overrides the app's `colorPrimary`, `colorSecondary`, and related attributes in `MaterialTheme`.

#### Android API Requirements

| Feature | Min API |
|---|---|
| Wallpaper-based dynamic color | Android 12 (API 31) |
| Dual-seed dynamic color | Android 16+ / Material 3 Expressive |
| Themed icons (launcher) | Android 13 (API 33) |
| Dynamic color in widgets | Android 12 (API 31) |

---

### In-App Content-Based Color

For apps that cannot rely on wallpaper (web, older Android, iOS, desktop), dynamic color can be seeded from in-app content — typically the **dominant color of an image**.

#### Extracting a Seed from an Image (Android)

```kotlin
// Using Palette API or Material Color Utilities
val bitmap: Bitmap = /* your image */
val wallpaperColors = WallpaperColors.fromBitmap(bitmap)
val colorScheme = ColorScheme(
    source = wallpaperColors.primaryColor.toArgb(),
    isDark = false
)
```

#### Content-Seeded Color Best Practices

- Extract color from **the most prominent image** on the screen (e.g., album art, product photo, hero image).
- Avoid low-chroma sources (near-white, near-black, or near-gray) — they produce desaturated themes.
- Animate the color transition when the source image changes; use `motion.duration.medium4` (400ms) with `easing.standard`.
- Re-extract on image load, not on every frame. Cache the resulting scheme.

---

### Dynamic Color on Web

The web does not have OS-level wallpaper color access. Web dynamic color is sourced from:

1. **Brand seed** (product-defined, stable)
2. **Content extraction** (JavaScript canvas pixel sampling)
3. **User-defined** (color picker preference)

```javascript
// Using @material/material-color-utilities
import { argbFromHex, themeFromSourceColor, applyTheme } from '@material/material-color-utilities';

const theme = themeFromSourceColor(argbFromHex('#6750A4'), [
  { name: 'custom-1', value: argbFromHex('#E8DEF8'), blend: true }
]);

// Apply to CSS custom properties
applyTheme(theme, { target: document.body, dark: false });
```

`applyTheme()` sets all `--md-sys-color-*` CSS custom properties on the target element, enabling instant theme switching.

---

## Static Theming

### Baseline Scheme

The **Material Design baseline** is the default theme for products that don't implement dynamic color or custom branding. It uses Google's purple seed (`#6750A4`) and is always available as a fallback.

The baseline scheme ships in the M3 design kit and all platform implementations as the default token values. See `tokens.md` for the full baseline color table.

### Custom Static Themes

To create a custom static theme:

1. **Choose a primary seed** representing your brand's main color.
2. **Feed it into Material Theme Builder** — the tool generates all system token values.
3. **Export** as Compose code, XML, CSS custom properties, or JSON.
4. **Apply** by overriding the relevant system tokens in your platform theme.

Static themes do not adapt per user, but they can still implement **light/dark modes** using the two scheme variants output by the Theme Builder.

---

### Material Theme Builder

The **Material Theme Builder** ([m3.material.io/theme-builder](https://m3.material.io/theme-builder)) is the official tool for generating M3 themes.

#### Inputs

| Input | Description |
|---|---|
| Primary color | Main brand seed (required) |
| Secondary color | Optional; if omitted, derived from primary |
| Tertiary color | Optional; if omitted, derived from primary (+60° hue) |
| Neutral color | Optional; if omitted, derived from primary (low chroma) |
| Extended colors | Custom additional colors with optional harmonization |

#### Outputs

| Export | Platform | Contents |
|---|---|---|
| **Compose** | Android (Kotlin) | `Color.kt`, `Theme.kt`, `Type.kt` |
| **XML** | Android (XML Views) | `colors.xml`, `themes.xml` |
| **CSS** | Web | CSS custom properties stylesheet |
| **Dart** | Flutter | `color_scheme.dart`, `theme_data.dart` |
| **Figma** | Design | Figma variables (via plugin) |
| **JSON** | Any | Raw token values |

---

## Color Customization

### Brand Color Integration

Integrating a brand identity into M3 means mapping brand colors to HCT and using them as seeds — not hard-coding them directly into roles.

#### Process

```
1. Convert brand hex → HCT
2. Adjust chroma if needed (brand colors can be too vibrant or too muted)
3. Use as seed → generate tonal palette
4. Map tones to color roles
5. Verify contrast of all role pairs (target ≥ 4.5:1)
6. Adjust seed tone if needed to hit contrast targets
```

#### Brand Color Mapping Example

A brand with primary `#FF6D00` (orange):

| Role | Light Mode | Dark Mode |
|---|---|---|
| `primary` | `#924900` (P-40) | `#FFB690` (P-80) |
| `on-primary` | `#FFFFFF` (P-100) | `#4E2500` (P-20) |
| `primary-container` | `#FFDBCC` (P-90) | `#6E3500` (P-30) |
| `on-primary-container` | `#321200` (P-10) | `#FFDBCC` (P-90) |

The hex values at each tone are generated by the HCT algorithm — not hand-picked. This ensures the entire palette is harmonically consistent and accessible.

---

### Color Harmonization

**Color harmonization** shifts a standalone color toward the primary hue to make it feel cohesive within the dynamic color scheme. This is especially useful for:

- Semantic colors (success green, warning amber) that should feel native to the theme
- Third-party content colors (e.g., app icons, category badges)
- Illustration and data visualization colors

#### Harmonization Algorithm

```
harmonized = HCT.from(
  target.hue + (source.hue - target.hue) × 0.5,
  target.chroma,
  target.tone
)
```

The result shifts the target hue halfway toward the source hue, preserving the target's chroma and tone (and therefore its accessibility).

```kotlin
// Android / Material Color Utilities
val harmonized = Blend.harmonize(
    designColor = Color.Green.toArgb(),
    sourceColor = colorScheme.primary.toArgb()
)
```

```javascript
// Web
import { Blend } from '@material/material-color-utilities';
const harmonized = Blend.harmonize(0xFF00FF00, sourceArgb);
```

---

### Fixed Accent Colors

**Fixed colors** are brand or semantic colors that must not change with dynamic color. They maintain their identity across all user wallpapers and themes.

Fixed roles available:

| Role | Description |
|---|---|
| `primary-fixed` | Primary at P-90 tone, unchanging across schemes |
| `primary-fixed-dim` | Dimmer variant at P-80; for secondary uses |
| `on-primary-fixed` | High-contrast text on primary-fixed (P-10) |
| `on-primary-fixed-variant` | Lower-emphasis text on primary-fixed (P-30) |

Secondary and tertiary have equivalent fixed roles. Use fixed colors when:
- Displaying a colored logo or icon that represents the brand
- Showing data visualization with meaningful hue associations
- Rendering partner brand colors within your app

---

### Custom Color Groups

For colors outside the standard M3 palette (e.g., success, warning, info, or brand-specific accent), M3 supports **custom color groups** — mini palettes with the same role structure.

Each custom group contains 4 roles:

| Role | Description |
|---|---|
| `{name}` | The custom color itself (tone 40 light / 80 dark) |
| `on-{name}` | Text/icon color on top of `{name}` |
| `{name}-container` | Fill color using `{name}` at tone 90/30 |
| `on-{name}-container` | Text/icon on top of `{name}-container` |

```javascript
// Material Theme Builder extended color
{
  name: "success",
  value: 0xFF00A550,  // brand green
  blend: true         // harmonize toward primary
}
```

The builder generates all 4 roles for both light and dark themes.

---

## Dark Mode Theming

### Automatic Dark Theme

M3 generates both light and dark color schemes from the same seed. Dark mode is not an inversion of light mode — it's an independent mapping of tonal palette tones to semantic roles.

In dark mode, the system uses **higher-tone** values for accent colors (P-80 instead of P-40) and **lower-tone** values for surfaces (N-10 instead of N-99), ensuring legibility and reducing eye strain in low-light environments.

#### Light vs. Dark Tone Assignments

| Role | Light Tone | Dark Tone | Reason |
|---|---|---|---|
| `primary` | 40 | 80 | Enough chroma; accessible on dark surfaces |
| `on-primary` | 100 | 20 | High contrast reverse |
| `primary-container` | 90 | 30 | Muted fill for dark backgrounds |
| `surface` | 99 | 10 | Near-white / near-black |
| `on-surface` | 10 | 90 | High contrast on surface |
| `outline` | NV-50 | NV-60 | Visible on both backgrounds |

---

### Forced Dark Mode vs. True Dark Theme

| Approach | How | Result | Recommendation |
|---|---|---|---|
| **True dark theme** | Separate dark color scheme with designer-chosen roles | Best accessibility; intentional | ✅ Always preferred |
| **Forced dark** | OS/browser inverts light theme automatically | Inconsistent; may fail contrast | ❌ Avoid; only as fallback |

On Android, enabling `android:forceDarkAllowed="false"` prevents the system from force-darkening your views and ensures your true dark theme is used.

---

### Dark Theme Best Practices

#### Do
- Use `surface-container-*` tokens for all card and panel backgrounds — they encode proper dark-mode elevation tinting.
- Desaturate colors in dark mode — high-chroma colors cause eye strain on dark backgrounds.
- Test all color role pairings in dark mode specifically; don't assume light-mode contrasts carry over.
- Use `elevation-level*` tinting in dark mode to differentiate surfaces without shadows.

#### Don't
- Don't use pure black (`#000000`) as the surface color — use `neutral10` (`#1C1B1F`). Pure black causes harsh contrast.
- Don't use the same colors for dark and light modes without tonal adjustment.
- Don't rely solely on color to differentiate cards from the background in dark mode — use the tinted elevation system.
- Don't use very high-chroma colors for large surface areas in dark mode.

#### Dark Mode Elevation Tinting

In dark mode, surfaces appear to get **lighter** as elevation increases — the opposite of shadows. This is done by applying the `primary` color at increasing opacity:

| Elevation | Primary Overlay |
|---|---|
| Level 0 | 0% |
| Level 1 | 5% → `surface-container-low` |
| Level 2 | 8% → `surface-container` |
| Level 3 | 11% → `surface-container-high` |
| Level 4 | 12% → slightly above high |
| Level 5 | 14% → `surface-container-highest` |

---

## Typography Theming

### Replacing Typefaces

The two typeface slots — `brand` (expressive) and `plain` (utility) — can be replaced with any typeface while maintaining the full type scale structure.

```css
/* Web */
:root {
  --md-ref-typeface-brand: 'Playfair Display', serif;
  --md-ref-typeface-plain: 'Source Sans 3', sans-serif;
}
```

```kotlin
// Android Compose
val myTypography = Typography(
    displayLarge = TextStyle(
        fontFamily = PlayfairDisplayFontFamily,
        fontWeight = FontWeight.Normal,
        fontSize = 57.sp,
        lineHeight = 64.sp,
        letterSpacing = (-0.25).sp,
    ),
    bodyLarge = TextStyle(
        fontFamily = SourceSansFontFamily,
        fontWeight = FontWeight.Normal,
        fontSize = 16.sp,
        lineHeight = 24.sp,
        letterSpacing = 0.5.sp,
    ),
    // ... all 15 styles
)
```

### Variable Font Theming

For products using variable fonts (including Google Sans Flex), weight and width axes can be animated in response to interaction states — creating expressive, physics-driven typographic responses.

```css
/* Responsive weight on hover */
.hero-headline {
  font-family: 'Google Sans Flex', sans-serif;
  font-variation-settings: 'wght' 400, 'wdth' 100;
  transition: font-variation-settings 300ms cubic-bezier(0.05, 0.7, 0.1, 1.0);
}
.hero-headline:hover {
  font-variation-settings: 'wght' 600, 'wdth' 90;
}
```

### Type Scale Customization

To adjust the type scale without replacing the typeface, override system typescale tokens:

```css
:root {
  /* Scale up display text for a more expressive brand */
  --md-sys-typescale-display-large-size: 4.5rem;
  --md-sys-typescale-display-large-line-height: 5rem;

  /* Increase body tracking for a more editorial feel */
  --md-sys-typescale-body-large-tracking: 0.05rem;
}
```

---

## Shape Theming

### Applying a Shape Personality

Shape personality affects the emotional character of the product. Customize by overriding system shape tokens:

```css
/* Rounded, friendly brand */
:root {
  --md-sys-shape-corner-extra-small: 8px;
  --md-sys-shape-corner-small: 12px;
  --md-sys-shape-corner-medium: 16px;
  --md-sys-shape-corner-large: 24px;
  --md-sys-shape-corner-extra-large: 32px;
}

/* Sharp, technical brand */
:root {
  --md-sys-shape-corner-extra-small: 0px;
  --md-sys-shape-corner-small: 2px;
  --md-sys-shape-corner-medium: 4px;
  --md-sys-shape-corner-large: 8px;
  --md-sys-shape-corner-extra-large: 12px;
}
```

### Per-Component Shape Override

Target specific components without affecting the global scale:

```css
/* Square buttons for a specific section */
.technical-toolbar {
  --md-filled-button-container-shape: 0px;
  --md-outlined-button-container-shape: 0px;
}

/* Rounder cards for a consumer feel */
.lifestyle-section {
  --md-elevated-card-container-shape: 20px;
}
```

### Shape Morph Animation (M3 Expressive)

Shape morph enables smooth transitions between different corner radii. Use the M3 Expressive `ShapeDefaults.morphTo()` API on Android, or CSS `border-radius` transition on web.

```css
/* Web shape morph */
.fab {
  border-radius: var(--md-sys-shape-corner-large); /* 16px */
  transition: border-radius 300ms cubic-bezier(0.05, 0.7, 0.1, 1.0),
              width 300ms cubic-bezier(0.05, 0.7, 0.1, 1.0);
}
.fab.extended {
  border-radius: var(--md-sys-shape-corner-full); /* pill */
  width: auto;
}
```

```kotlin
// Android Compose — shape morph via Expressive APIs
val morphedShape = remember { MutableInteractionSource() }
FilledTonalIconButton(
    modifier = Modifier.morphShape(
        from = MaterialTheme.shapes.large,
        to = MaterialTheme.shapes.full,
        fraction = expansionFraction
    )
)
```

---

## Motion Theming

### Customizing Duration Tokens

Override motion duration system tokens to make your app feel faster or more deliberate:

```css
/* Faster, snappier app */
:root {
  --md-sys-motion-duration-short4: 150ms;
  --md-sys-motion-duration-medium2: 220ms;
  --md-sys-motion-duration-medium4: 300ms;
}

/* Slower, more cinematic */
:root {
  --md-sys-motion-duration-medium2: 400ms;
  --md-sys-motion-duration-long2: 650ms;
}
```

### Customizing Spring Physics (M3 Expressive)

```kotlin
// Android Compose — custom spring spec
val brandSpring = spring<Float>(
    dampingRatio = 0.6f,      // slight bounce
    stiffness = 300f,
    visibilityThreshold = 0.001f
)

val offsetX by animateFloatAsState(
    targetValue = expanded.value,
    animationSpec = brandSpring
)
```

### Reduced Motion Handling

Always implement reduced motion alternatives:

```css
@media (prefers-reduced-motion: reduce) {
  :root {
    /* Replace spatial transitions with instant cross-fades */
    --md-sys-motion-duration-short1: 0ms;
    --md-sys-motion-duration-medium2: 100ms;
    --md-sys-motion-duration-long2: 100ms;
  }
}
```

```kotlin
// Android
val isReducedMotion = LocalAccessibilityManager.current
    ?.isUserHapticsEnabled == false

val duration = if (isReducedMotion) 0 else 300
```

---

## Component-Level Theming

### Strategy: Global → Component → Instance

Theme changes should be applied at the **most general level** that meets your needs:

```
Global theme (system tokens)
  ↓ override for a category
Section/Page scope (CSS class or Compose CompositionLocalProvider)
  ↓ override for one component type
Component token (--md-comp-* or MaterialTheme.colorScheme.copy())
  ↓ override for one specific instance
Instance override (inline style or Modifier)
```

### Web: CSS Scoping

```css
/* Section-level theme: Invert to primary-container */
.promo-section {
  --md-sys-color-primary: var(--md-ref-palette-primary90);
  --md-sys-color-on-primary: var(--md-ref-palette-primary10);
  --md-sys-color-surface: var(--md-ref-palette-primary40);
  --md-sys-color-on-surface: var(--md-ref-palette-primary100);
}

/* Component token override */
.danger-button {
  --md-filled-button-container-color: var(--md-sys-color-error);
  --md-filled-button-label-text-color: var(--md-sys-color-on-error);
  --md-filled-button-hover-state-layer-color: var(--md-sys-color-on-error);
}
```

### Android Compose: CompositionLocalProvider

```kotlin
// Override theme for a subtree
CompositionLocalProvider(
    LocalContentColor provides MaterialTheme.colorScheme.tertiary
) {
    Icon(Icons.Filled.Star, contentDescription = null)
    Text("Featured", style = MaterialTheme.typography.labelLarge)
}

// Override color scheme for a surface
Surface(
    color = MaterialTheme.colorScheme.primaryContainer
) {
    // All children see primaryContainer as their surface color
    Text(
        text = "Highlighted content",
        color = MaterialTheme.colorScheme.onPrimaryContainer
    )
}
```

### Theme Overlays (Android XML)

```xml
<!-- res/values/themes.xml -->
<style name="Theme.App.PromoSection" parent="Theme.Material3.DayNight">
    <item name="colorPrimary">@color/brand_alt_primary</item>
    <item name="colorOnPrimary">@color/brand_alt_on_primary</item>
    <item name="colorSurface">@color/brand_alt_surface</item>
</style>

<!-- In layout -->
<LinearLayout
    android:theme="@style/Theme.App.PromoSection"
    ...>
    <!-- Children pick up the overlay theme -->
</LinearLayout>
```

---

## Multi-Brand & White-Label Theming

### Architecture for Multiple Themes

For apps that serve multiple brands or support white-label theming, structure themes as independently loadable token sets:

```
themes/
  base/                  # Shared baseline tokens
    color-roles.css
    typography.css
    motion.css
  brand-a/               # Brand A overrides
    color-roles.css      # Override only --md-sys-color-* tokens
    typography.css
  brand-b/               # Brand B overrides
    color-roles.css
    typography.css
    shape.css
```

```css
/* Load order: base first, then brand override */
@import 'themes/base/color-roles.css';
@import 'themes/brand-a/color-roles.css'; /* overrides take precedence */
```

### Runtime Theme Switching

```javascript
// Web: swap theme via data attribute
function applyTheme(brandId) {
  document.documentElement.setAttribute('data-brand', brandId);
}

// CSS
[data-brand="brand-a"] {
  --md-sys-color-primary: #0057D9;
  --md-sys-color-on-primary: #FFFFFF;
  /* ... */
}

[data-brand="brand-b"] {
  --md-sys-color-primary: #D32F2F;
  --md-sys-color-on-primary: #FFFFFF;
  /* ... */
}
```

```kotlin
// Android: dynamic theme selection
@Composable
fun AppTheme(brandId: Brand, content: @Composable () -> Unit) {
    val colorScheme = when (brandId) {
        Brand.BrandA -> brandALightColorScheme()
        Brand.BrandB -> brandBLightColorScheme()
    }
    MaterialTheme(
        colorScheme = colorScheme,
        typography = sharedTypography,
        shapes = sharedShapes,
        content = content
    )
}
```

---

## Platform-Specific Theming

### Android (Compose)

Complete theme setup in Jetpack Compose:

```kotlin
// ui/theme/Theme.kt
private val LightColorScheme = lightColorScheme(
    primary = Primary40,
    onPrimary = Primary100,
    primaryContainer = Primary90,
    onPrimaryContainer = Primary10,
    secondary = Secondary40,
    onSecondary = Secondary100,
    secondaryContainer = Secondary90,
    onSecondaryContainer = Secondary10,
    tertiary = Tertiary40,
    onTertiary = Tertiary100,
    tertiaryContainer = Tertiary90,
    onTertiaryContainer = Tertiary10,
    error = Error40,
    errorContainer = Error90,
    onError = Error100,
    onErrorContainer = Error10,
    background = Neutral99,
    onBackground = Neutral10,
    surface = Neutral99,
    onSurface = Neutral10,
    surfaceVariant = NeutralVariant90,
    onSurfaceVariant = NeutralVariant30,
    outline = NeutralVariant50,
    inverseOnSurface = Neutral95,
    inverseSurface = Neutral20,
    inversePrimary = Primary80,
    surfaceTint = Primary40,
    outlineVariant = NeutralVariant80,
    scrim = Neutral0,
)

private val DarkColorScheme = darkColorScheme(
    primary = Primary80,
    onPrimary = Primary20,
    primaryContainer = Primary30,
    onPrimaryContainer = Primary90,
    // ... continue for all roles
)

@Composable
fun AppTheme(
    darkTheme: Boolean = isSystemInDarkTheme(),
    dynamicColor: Boolean = true,
    content: @Composable () -> Unit
) {
    val colorScheme = when {
        dynamicColor && Build.VERSION.SDK_INT >= Build.VERSION_CODES.S -> {
            val context = LocalContext.current
            if (darkTheme) dynamicDarkColorScheme(context)
            else dynamicLightColorScheme(context)
        }
        darkTheme -> DarkColorScheme
        else -> LightColorScheme
    }

    MaterialTheme(
        colorScheme = colorScheme,
        typography = AppTypography,
        shapes = AppShapes,
        content = content
    )
}
```

```kotlin
// ui/theme/Type.kt
val AppTypography = Typography(
    displayLarge = TextStyle(
        fontFamily = GoogleSansFontFamily,
        fontWeight = FontWeight.Normal,
        fontSize = 57.sp,
        lineHeight = 64.sp,
        letterSpacing = (-0.25).sp
    ),
    // ... all 15 styles
)

// ui/theme/Shape.kt
val AppShapes = Shapes(
    extraSmall = RoundedCornerShape(4.dp),
    small = RoundedCornerShape(8.dp),
    medium = RoundedCornerShape(12.dp),
    large = RoundedCornerShape(16.dp),
    extraLarge = RoundedCornerShape(28.dp)
)
```

---

### Web (material-web)

```html
<!-- index.html: load MWC + Google Fonts -->
<link rel="stylesheet" href="https://fonts.googleapis.com/css2?family=Google+Sans&family=Roboto&display=swap">
<script type="module">
  import '@material/web/button/filled-button.js';
  import '@material/web/textfield/filled-text-field.js';
  // ... import needed components
</script>
```

```css
/* theme.css: set all system tokens */
:root {
  /* Typeface */
  --md-ref-typeface-brand: 'Google Sans', sans-serif;
  --md-ref-typeface-plain: 'Roboto', sans-serif;

  /* Shape */
  --md-sys-shape-corner-full: 9999px;
  --md-sys-shape-corner-extra-large: 28px;
  --md-sys-shape-corner-large: 16px;
  --md-sys-shape-corner-medium: 12px;
  --md-sys-shape-corner-small: 8px;
  --md-sys-shape-corner-extra-small: 4px;
}

/* Light theme color tokens */
:root, [data-theme="light"] {
  --md-sys-color-primary: #6750A4;
  --md-sys-color-on-primary: #FFFFFF;
  --md-sys-color-primary-container: #EADDFF;
  --md-sys-color-on-primary-container: #21005D;
  --md-sys-color-secondary: #625B71;
  --md-sys-color-on-secondary: #FFFFFF;
  --md-sys-color-secondary-container: #E8DEF8;
  --md-sys-color-on-secondary-container: #1D192B;
  --md-sys-color-surface: #FFFBFE;
  --md-sys-color-on-surface: #1C1B1F;
  --md-sys-color-surface-container: #F3EDF7;
  --md-sys-color-outline: #79747E;
  --md-sys-color-error: #B3261E;
  /* ... all roles */
}

/* Dark theme */
@media (prefers-color-scheme: dark) {
  :root {
    --md-sys-color-primary: #D0BCFF;
    --md-sys-color-on-primary: #381E72;
    /* ... */
  }
}

[data-theme="dark"] {
  --md-sys-color-primary: #D0BCFF;
  --md-sys-color-on-primary: #381E72;
  /* ... */
}
```

---

### Flutter

```dart
// lib/theme/app_theme.dart
import 'package:flutter/material.dart';

class AppTheme {
  static const _seedColor = Color(0xFF6750A4);

  static ThemeData get light => ThemeData(
    useMaterial3: true,
    colorScheme: ColorScheme.fromSeed(
      seedColor: _seedColor,
      brightness: Brightness.light,
    ),
    textTheme: _textTheme,
    fontFamily: 'GoogleSans',
  );

  static ThemeData get dark => ThemeData(
    useMaterial3: true,
    colorScheme: ColorScheme.fromSeed(
      seedColor: _seedColor,
      brightness: Brightness.dark,
    ),
    textTheme: _textTheme,
    fontFamily: 'GoogleSans',
  );

  static const TextTheme _textTheme = TextTheme(
    displayLarge: TextStyle(fontSize: 57, height: 1.12, letterSpacing: -0.25),
    displayMedium: TextStyle(fontSize: 45, height: 1.16),
    displaySmall: TextStyle(fontSize: 36, height: 1.22),
    headlineLarge: TextStyle(fontSize: 32, height: 1.25),
    headlineMedium: TextStyle(fontSize: 28, height: 1.29),
    headlineSmall: TextStyle(fontSize: 24, height: 1.33),
    titleLarge: TextStyle(fontSize: 22, height: 1.27),
    titleMedium: TextStyle(fontSize: 16, height: 1.50, fontWeight: FontWeight.w500, letterSpacing: 0.15),
    titleSmall: TextStyle(fontSize: 14, height: 1.43, fontWeight: FontWeight.w500, letterSpacing: 0.1),
    bodyLarge: TextStyle(fontSize: 16, height: 1.50, letterSpacing: 0.5),
    bodyMedium: TextStyle(fontSize: 14, height: 1.43, letterSpacing: 0.25),
    bodySmall: TextStyle(fontSize: 12, height: 1.33, letterSpacing: 0.4),
    labelLarge: TextStyle(fontSize: 14, height: 1.43, fontWeight: FontWeight.w500, letterSpacing: 0.1),
    labelMedium: TextStyle(fontSize: 12, height: 1.33, fontWeight: FontWeight.w500, letterSpacing: 0.5),
    labelSmall: TextStyle(fontSize: 11, height: 1.45, fontWeight: FontWeight.w500, letterSpacing: 0.5),
  );
}

// main.dart
MaterialApp(
  theme: AppTheme.light,
  darkTheme: AppTheme.dark,
  themeMode: ThemeMode.system, // respects OS setting
)
```

---

## Adaptive & Foldable Theming

M3 theming adapts across the device size landscape. Foldable devices (Android) introduce new considerations for how themes apply across physical display segments.

### Folded vs. Unfolded States

```kotlin
// Detect fold state with WindowManager
val windowInfoTracker = WindowInfoTracker.getOrCreate(this)
lifecycleScope.launch {
    windowInfoTracker.windowLayoutInfo(this@Activity).collect { info ->
        val foldFeature = info.displayFeatures.filterIsInstance<FoldingFeature>().firstOrNull()
        val isFolded = foldFeature?.state == FoldingFeature.State.HALF_OPENED
                    || foldFeature?.state == FoldingFeature.State.FLAT
        // Adjust navigation component and layout accordingly
        updateNavigationPattern(isFolded)
    }
}
```

### Navigation Pattern by Posture

| Posture | Navigation | Content Layout |
|---|---|---|
| **Compact / Folded** | Bottom Navigation Bar | Single column |
| **Half-open (tabletop)** | Bottom Navigation Bar | Two-panel above fold |
| **Flat (unfolded)** | Navigation Rail or Drawer | Two-panel; list-detail |
| **Large screen (tablet)** | Navigation Drawer | Three-panel possible |

### Breakpoint-Responsive Token Adjustments

Consider increasing spacing and typography for large screens:

```css
@media (min-width: 840px) {
  :root {
    /* Slightly larger body for readability at distance */
    --md-sys-typescale-body-large-size: 1.0625rem;
    --md-sys-typescale-body-large-line-height: 1.625rem;

    /* More breathing room in containers */
    --section-padding: 32px;
  }
}

@media (min-width: 1240px) {
  :root {
    --section-padding: 48px;
    --content-max-width: 1024px;
  }
}
```

---

## Theming Checklist

Use this checklist when implementing or auditing an M3 theme.

### Color

- [ ] Primary seed color chosen and fed into Material Theme Builder
- [ ] Light and dark color schemes generated (all roles defined)
- [ ] All color role pairings verified at ≥4.5:1 contrast
- [ ] Dynamic color implemented (Android 12+ / web fallback)
- [ ] Static fallback scheme defined for pre-Android 12
- [ ] Extended/custom colors harmonized toward primary
- [ ] Fixed colors defined for immutable brand elements
- [ ] Dark mode tested across all screens and components

### Typography

- [ ] Brand and plain typefaces specified via `--md-ref-typeface-brand` / `plain`
- [ ] All 15 type styles defined and mapped to components correctly
- [ ] Font files licensed and loaded (WOFF2 for web; TTF/OTF for Android)
- [ ] Text scaling tested at 100%, 130%, 150%, 200%
- [ ] Reading line length constrained to 50–75 characters for body text

### Shape

- [ ] Shape personality chosen and applied consistently
- [ ] Custom shape scale defined if deviating from defaults
- [ ] Partial-corner shapes applied where required (bottom sheets, text fields)
- [ ] Shape morph animations implemented for expressive transitions

### Motion

- [ ] Duration tokens selected based on app feel (fast vs. deliberate)
- [ ] Easing curves assigned: Emphasized for spatial; Standard for utility
- [ ] `prefers-reduced-motion` media query implemented and tested
- [ ] Spring physics applied for expressive components (M3 Expressive)

### Accessibility

- [ ] All text meets 4.5:1 contrast (or 3:1 for large text / UI components)
- [ ] Focus rings visible on all interactive components
- [ ] Touch targets ≥ 48×48dp throughout
- [ ] Screen reader labels present on all icon-only controls
- [ ] Color is not the sole differentiator for any state or category

### Platform

- [ ] Android: Dynamic color with `DynamicColors.applyToActivitiesIfAvailable()`
- [ ] Android: True dark theme (not forced dark)
- [ ] Web: CSS custom properties for all system tokens
- [ ] Web: `prefers-color-scheme` media query for automatic dark mode
- [ ] Flutter: `ThemeData(useMaterial3: true)` with `ColorScheme.fromSeed()`

### Testing

- [ ] Snapshot tests for both light and dark themes
- [ ] Dynamic color tested with multiple wallpaper hues
- [ ] Typography tested with system font size overrides
- [ ] Theme switch animation verified (where applicable)
- [ ] Accessibility audit with automated tools (Accessibility Scanner, axe)

---

*Last updated: June 2026 · Material Design 3 Expressive*
