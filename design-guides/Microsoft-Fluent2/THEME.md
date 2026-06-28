# THEME.md — Fluent 2 Theming Guide

## Overview

This document covers how theming is structured and applied in this project using Fluent 2's theming architecture. Themes map design tokens to visual intents, enabling light, dark, high-contrast, and custom brand experiences from a single component codebase.

---

## Theming Architecture

```
Brand Ramp (10 shades)
    ↓
Global Tokens (raw palette + scale values)
    ↓
Alias Tokens (semantic intent — e.g., "primary background")
    ↓
Component Tokens (component-scoped — e.g., "button primary background")
    ↓
Rendered UI
```

Theming works by **swapping alias token values** while keeping component and global token names unchanged. Components never reference raw colors directly.

---

## Built-in Themes

Fluent 2 ships four base themes:

| Theme | Description | CSS Class / Object |
|-------|-------------|-------------------|
| `webLightTheme` | Default light theme | `FluentProvider theme={webLightTheme}` |
| `webDarkTheme` | Default dark theme | `FluentProvider theme={webDarkTheme}` |
| `teamsLightTheme` | Teams-optimized light | `FluentProvider theme={teamsLightTheme}` |
| `teamsDarkTheme` | Teams-optimized dark | `FluentProvider theme={teamsDarkTheme}` |

High-contrast themes (`webHighContrastTheme`) are also available and should always be tested.

---

## Light Theme Reference

Key alias token mappings for the light theme:

| Alias Token | Resolved Value | Purpose |
|-------------|----------------|---------|
| `colorNeutralBackground1` | `#FFFFFF` | Base page surface |
| `colorNeutralBackground2` | `#F5F5F5` | Secondary surface |
| `colorNeutralForeground1` | `#242424` | Primary text |
| `colorNeutralForeground2` | `#424242` | Secondary text |
| `colorBrandBackground` | `#0F6CBD` | Brand fill |
| `colorBrandForeground1` | `#0F6CBD` | Brand text |
| `colorNeutralStroke1` | `#D1D1D1` | Default border |

---

## Dark Theme Reference

Key alias token overrides for dark mode:

| Alias Token | Resolved Value | Purpose |
|-------------|----------------|---------|
| `colorNeutralBackground1` | `#292929` | Base page surface |
| `colorNeutralBackground2` | `#1F1F1F` | Secondary surface |
| `colorNeutralForeground1` | `#FFFFFF` | Primary text |
| `colorNeutralForeground2` | `#D6D6D6` | Secondary text |
| `colorBrandBackground` | `#479EF5` | Brand fill (lightened for contrast) |
| `colorBrandForeground1` | `#479EF5` | Brand text |
| `colorNeutralStroke1` | `#575757` | Default border |

---

## Custom Brand Theme

To apply a custom brand color, generate a **brand ramp** (10 shades from shade 10 to 160) and pass it to `createLightTheme` / `createDarkTheme`:

```ts
import { createLightTheme, createDarkTheme, BrandVariants } from '@fluentui/react-components';

const myBrand: BrandVariants = {
  10:  '#060102',
  20:  '#261018',
  30:  '#431426',
  40:  '#5C1732',
  50:  '#751A3E',
  60:  '#8E1C4B',
  70:  '#A71E59',
  80:  '#C02068',
  90:  '#C83E7B',
  100: '#CF598E',
  110: '#D672A1',
  120: '#DC8AB4',
  130: '#E3A2C7',
  140: '#E9BAD9',
  150: '#F0D1EB',
  160: '#F7E9F5',
};

export const myLightTheme = createLightTheme(myBrand);
export const myDarkTheme   = createDarkTheme(myBrand);
```

> Use the [Fluent Theme Designer](https://fluent2.microsoft.design/theme-designer) to generate your brand ramp from a single hex color.

---

## Applying the Theme

### React (Fluent UI React v9)

Wrap your app root in `FluentProvider`:

```tsx
import { FluentProvider, webLightTheme } from '@fluentui/react-components';

function App() {
  return (
    <FluentProvider theme={webLightTheme}>
      {/* your app */}
    </FluentProvider>
  );
}
```

### CSS Custom Properties

All alias tokens are exposed as CSS custom properties on the `FluentProvider` element:

```css
/* Access any token via var() */
.my-card {
  background-color: var(--colorNeutralBackground1);
  color:            var(--colorNeutralForeground1);
  border:           1px solid var(--colorNeutralStroke1);
  border-radius:    var(--borderRadiusLarge);
  box-shadow:       var(--shadow4);
}
```

### Theme Switching (Light / Dark)

Detect the OS preference and toggle:

```ts
const prefersDark = window.matchMedia('(prefers-color-scheme: dark)');

function getTheme() {
  return prefersDark.matches ? webDarkTheme : webLightTheme;
}

// React state example
const [theme, setTheme] = React.useState(getTheme());
prefersDark.addEventListener('change', () => setTheme(getTheme()));
```

---

## High Contrast Support

Always test in Windows High Contrast mode (Accessibility → High contrast in Windows Settings).

- Do **not** override `-ms-high-contrast` media queries in component styles.
- Avoid `background-image` for meaningful content in high-contrast mode.
- Use `forced-colors: active` media query for conditional adjustments:

```css
@media (forced-colors: active) {
  .badge {
    border: 1px solid ButtonText;
  }
}
```

---

## Token Overrides & Customization

To override individual tokens without creating a full custom theme:

```ts
import { webLightTheme } from '@fluentui/react-components';

const customTheme = {
  ...webLightTheme,
  colorBrandBackground:      '#7B2D8B', // custom purple brand
  colorBrandBackgroundHover: '#6A2478',
  borderRadiusMedium:        '8px',     // rounder buttons
};
```

> Keep overrides minimal — large sets of ad-hoc overrides undermine token consistency. Prefer brand ramp generation for color changes.

---

## Theming Checklist

- [ ] Light theme renders correctly
- [ ] Dark theme renders correctly
- [ ] High-contrast mode renders correctly
- [ ] Custom brand ramp generated and applied
- [ ] No hard-coded hex colors in component styles
- [ ] `prefers-color-scheme` detected and respected
- [ ] `prefers-reduced-motion` detected and respected
- [ ] All focus indicators visible at 3:1+ contrast ratio

---

## References

- [Fluent 2 Theme Designer](https://fluent2.microsoft.design/theme-designer)
- [createLightTheme / createDarkTheme API](https://react.fluentui.dev/?path=/docs/theme-theme-designer--page)
- [CSS Custom Properties in Fluent UI](https://react.fluentui.dev/?path=/docs/tokens--page)
- [WCAG 1.4.11 Non-text Contrast](https://www.w3.org/WAI/WCAG21/Understanding/non-text-contrast.html)
