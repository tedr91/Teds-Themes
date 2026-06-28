# TOKENS.md — Fluent 2 Design Tokens

## Overview

Design tokens are the atomic values that power the visual language of this project. They are organized in a three-tier hierarchy aligned with Fluent 2's token architecture:

```
Global Tokens  →  Alias Tokens  →  Component Tokens
(raw values)      (semantic)        (component-specific)
```

> **Note**: Token names use the `camelCase` convention matching Fluent UI React v9. When used in CSS custom properties, prefix with `--` (e.g., `--colorBrandBackground`).

---

## Color Tokens

### Brand Colors (Blue — default theme)

| Token | Value | Usage |
|-------|-------|-------|
| `colorBrandBackground` | `#0F6CBD` | Primary button background, links |
| `colorBrandBackgroundHover` | `#115EA3` | Hover state |
| `colorBrandBackgroundPressed` | `#0E4775` | Pressed state |
| `colorBrandBackgroundSelected` | `#0E4775` | Selected state |
| `colorBrandForeground1` | `#0F6CBD` | Brand-colored text |
| `colorBrandForeground2` | `#115EA3` | Brand text hover |
| `colorBrandStroke1` | `#0F6CBD` | Brand border/outline |

### Neutral Colors

| Token | Light Value | Dark Value | Usage |
|-------|-------------|------------|-------|
| `colorNeutralForeground1` | `#242424` | `#FFFFFF` | Primary text |
| `colorNeutralForeground2` | `#424242` | `#D6D6D6` | Secondary text |
| `colorNeutralForeground3` | `#616161` | `#ADADAD` | Placeholder, disabled text |
| `colorNeutralForeground4` | `#707070` | `#8B8B8B` | Hint text |
| `colorNeutralBackground1` | `#FFFFFF` | `#292929` | Page / card surface |
| `colorNeutralBackground2` | `#F5F5F5` | `#1F1F1F` | Secondary surface |
| `colorNeutralBackground3` | `#F0F0F0` | `#141414` | Tertiary surface |
| `colorNeutralBackground4` | `#EBEBEB` | `#0A0A0A` | Quaternary surface |
| `colorNeutralBackground1Hover` | `#F5F5F5` | `#3D3D3D` | Hovered surface |
| `colorNeutralBackground1Pressed` | `#EBEBEB` | `#333333` | Pressed surface |
| `colorNeutralStroke1` | `#D1D1D1` | `#575757` | Default border |
| `colorNeutralStroke2` | `#E0E0E0` | `#484848` | Subtle border |
| `colorNeutralStrokeAccessible` | `#616161` | `#ADADAD` | Accessible border |

### Semantic / Status Colors

| Token | Value | Usage |
|-------|-------|-------|
| `colorStatusSuccessBackground1` | `#F1FAF1` | Success surface |
| `colorStatusSuccessForeground1` | `#107C10` | Success text / icon |
| `colorStatusSuccessBorderActive` | `#107C10` | Success border |
| `colorStatusWarningBackground1` | `#FFF9F0` | Warning surface |
| `colorStatusWarningForeground1` | `#8A5B00` | Warning text / icon |
| `colorStatusWarningBorderActive` | `#F7CA5C` | Warning border |
| `colorStatusDangerBackground1` | `#FEF0EF` | Error surface |
| `colorStatusDangerForeground1` | `#BC2F32` | Error text / icon |
| `colorStatusDangerBorderActive` | `#BC2F32` | Error border |
| `colorStatusInfoBackground1` | `#EBF3FC` | Info surface |
| `colorStatusInfoForeground1` | `#0F6CBD` | Info text / icon |

---

## Typography Tokens

### Font Family

| Token | Value |
|-------|-------|
| `fontFamilyBase` | `'Segoe UI', system-ui, -apple-system, sans-serif` |
| `fontFamilyMonospace` | `'Cascadia Code', 'Courier New', Courier, monospace` |
| `fontFamilyNumeric` | `'Bahnschrift', 'DIN Alternate', 'Franklin Gothic Medium', sans-serif` |

### Font Size

| Token | Value | Usage |
|-------|-------|-------|
| `fontSizeBase100` | `10px` | Caption 2 |
| `fontSizeBase200` | `12px` | Caption 1 |
| `fontSizeBase300` | `14px` | Body 1 (default) |
| `fontSizeBase400` | `16px` | Body 2 |
| `fontSizeBase500` | `20px` | Subtitle 2 |
| `fontSizeBase600` | `24px` | Subtitle 1 |
| `fontSizeHero700` | `28px` | Title 3 |
| `fontSizeHero800` | `32px` | Title 2 |
| `fontSizeHero900` | `40px` | Title 1 |
| `fontSizeHero1000` | `68px` | Large Title |

### Font Weight

| Token | Value |
|-------|-------|
| `fontWeightRegular` | `400` |
| `fontWeightMedium` | `500` |
| `fontWeightSemibold` | `600` |
| `fontWeightBold` | `700` |

### Line Height

| Token | Value |
|-------|-------|
| `lineHeightBase100` | `14px` |
| `lineHeightBase200` | `16px` |
| `lineHeightBase300` | `20px` |
| `lineHeightBase400` | `22px` |
| `lineHeightBase500` | `28px` |
| `lineHeightBase600` | `32px` |
| `lineHeightHero700` | `36px` |
| `lineHeightHero800` | `40px` |
| `lineHeightHero900` | `52px` |
| `lineHeightHero1000` | `92px` |

---

## Spacing Tokens

All spacing is based on a **4px base unit**.

| Token | Value | Usage |
|-------|-------|-------|
| `spacingHorizontalNone` | `0` | No spacing |
| `spacingHorizontalXXS` | `2px` | Hairline gaps |
| `spacingHorizontalXS` | `4px` | Tight inline spacing |
| `spacingHorizontalSNudge` | `6px` | Nudged small |
| `spacingHorizontalS` | `8px` | Small gaps |
| `spacingHorizontalMNudge` | `10px` | Nudged medium |
| `spacingHorizontalM` | `12px` | Standard inline |
| `spacingHorizontalL` | `16px` | Default section gap |
| `spacingHorizontalXL` | `20px` | — |
| `spacingHorizontalXXL` | `24px` | Card padding |
| `spacingHorizontalXXXL` | `32px` | Large layout gap |
| `spacingVerticalNone` | `0` | — |
| `spacingVerticalXXS` | `2px` | — |
| `spacingVerticalXS` | `4px` | — |
| `spacingVerticalSNudge` | `6px` | — |
| `spacingVerticalS` | `8px` | — |
| `spacingVerticalMNudge` | `10px` | — |
| `spacingVerticalM` | `12px` | — |
| `spacingVerticalL` | `16px` | — |
| `spacingVerticalXL` | `20px` | — |
| `spacingVerticalXXL` | `24px` | — |
| `spacingVerticalXXXL` | `32px` | — |

---

## Border Radius Tokens

| Token | Value | Usage |
|-------|-------|-------|
| `borderRadiusNone` | `0` | Square (tables, code blocks) |
| `borderRadiusSmall` | `2px` | Subtle rounding |
| `borderRadiusMedium` | `4px` | Buttons, inputs (default) |
| `borderRadiusLarge` | `6px` | Cards |
| `borderRadiusXLarge` | `8px` | Panels, dialogs |
| `borderRadiusCircular` | `10000px` | Avatars, badges, pills |

---

## Shadow / Elevation Tokens

| Token | Value | Usage |
|-------|-------|-------|
| `shadow2` | `0 1px 2px rgba(0,0,0,0.14), 0 0 2px rgba(0,0,0,0.12)` | Cards (resting) |
| `shadow4` | `0 2px 4px rgba(0,0,0,0.14), 0 0 2px rgba(0,0,0,0.12)` | Dropdowns |
| `shadow8` | `0 4px 8px rgba(0,0,0,0.14), 0 0 2px rgba(0,0,0,0.12)` | Card hover, menus |
| `shadow16` | `0 8px 16px rgba(0,0,0,0.14), 0 0 2px rgba(0,0,0,0.12)` | Dialogs |
| `shadow28` | `0 14px 28px rgba(0,0,0,0.24), 0 0 8px rgba(0,0,0,0.20)` | Popovers |
| `shadow64` | `0 32px 64px rgba(0,0,0,0.24), 0 0 8px rgba(0,0,0,0.20)` | Full-screen overlays |

---

## Duration & Easing Tokens

| Token | Value |
|-------|-------|
| `durationUltraFast` | `50ms` |
| `durationFaster` | `100ms` |
| `durationFast` | `150ms` |
| `durationNormal` | `200ms` |
| `durationGentle` | `250ms` |
| `durationSlow` | `300ms` |
| `durationSlower` | `400ms` |
| `durationUltraSlow` | `500ms` |
| `curveAccelerateMax` | `cubic-bezier(0.9, 0.1, 1, 0.2)` |
| `curveAccelerateMid` | `cubic-bezier(1, 0, 1, 1)` |
| `curveAccelerateMin` | `cubic-bezier(0.8, 0, 0.78, 1)` |
| `curveDecelerateMax` | `cubic-bezier(0.1, 0.9, 0.2, 1)` |
| `curveDecelerateMid` | `cubic-bezier(0, 0, 0, 1)` |
| `curveDecelerateMin` | `cubic-bezier(0.33, 0, 0.1, 1)` |
| `curveEasyEaseMax` | `cubic-bezier(0.8, 0, 0.2, 1)` |
| `curveEasyEase` | `cubic-bezier(0.33, 0, 0.67, 1)` |
| `curveLinear` | `cubic-bezier(0, 0, 1, 1)` |

---

## References

- [Fluent 2 Tokens](https://fluent2.microsoft.design/design-tokens)
- [Fluent UI React Tokens Package](https://react.fluentui.dev/?path=/docs/tokens--page)
