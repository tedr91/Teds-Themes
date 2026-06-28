# COMPONENTS.md — Fluent 2 Component Library

## Overview

This document catalogs the UI components used in this project, their Fluent 2 variants, states, and usage guidance. Components follow Fluent 2's composable, accessible-first architecture.

---

## Component Catalog

### 1. Button

| Variant | Usage |
|---------|-------|
| `primary` | One primary action per screen (e.g., "Save", "Submit") |
| `secondary` | Secondary or alternative actions |
| `subtle` | Low-emphasis actions within dense layouts |
| `transparent` | Icon-only or inline actions |
| `outline` | Bordered, no fill; used on colored backgrounds |

**States**: default · hover · pressed · focused · disabled · loading

**Sizing**:
- `small`: height `24px`, padding `0 8px`
- `medium` (default): height `32px`, padding `0 12px`
- `large`: height `40px`, padding `0 16px`

**Accessibility**: All buttons must have an accessible label. For icon-only buttons, provide `aria-label`.

---

### 2. Input / TextField

- Use `label` elements — never rely on `placeholder` alone.
- Always pair with inline validation messages (below the field, never replacing the label).
- **States**: default · focus · filled · error · warning · success · disabled · read-only

```
[ Label              ]
[ ___________________] ← input
  Helper or error text
```

---

### 3. Checkbox & Radio

- **Checkbox**: multi-select; supports indeterminate state for parent/child groups.
- **Radio**: single-select within a group; always group with `role="radiogroup"`.
- Minimum tap target: `44×44px` (touch) — use invisible padding.

---

### 4. Dropdown / Combobox

- Use `Dropdown` for fixed option lists.
- Use `Combobox` when users can also type to filter.
- Support keyboard: `Arrow` keys to navigate, `Enter` to select, `Escape` to close.

---

### 5. Dialog & Modal

| Property | Value |
|----------|-------|
| Max width | `600px` |
| Padding | `24px` |
| Border radius | `8px` |
| Backdrop | `rgba(0,0,0,0.4)` |
| Focus trap | Required — tab cycles within dialog |

- Always include a visible **close button** (`×`) and wire `Escape` to dismiss.
- Announce dialog title via `aria-labelledby`.

---

### 6. Card

- Clickable cards: entire surface is the hit target; use `role="button"` or wrap in `<a>`.
- Non-clickable cards: informational only; no interactive role.
- Padding: `16px` (compact), `24px` (default).
- Elevation: shadow level 2 at rest, level 8 on hover (if interactive).

---

### 7. Navigation

#### Top Navigation Bar
- Contains: logo/product name, primary nav links, search, profile avatar.
- Height: `48px` (desktop), `56px` (mobile).

#### Left Navigation (Side Rail / Nav Drawer)
- Collapsed width: `48px` (icon rail).
- Expanded width: `240px`.
- Active item indicator: left-side 2px colored bar.

#### Breadcrumb
- Use for hierarchies deeper than 2 levels.
- Separate items with `/` or `›`.

---

### 8. Toast / Notification

| Variant | Semantic Color | Usage |
|---------|---------------|-------|
| `info` | Brand blue | Neutral updates |
| `success` | Green | Confirmations |
| `warning` | Yellow/Amber | Non-blocking caution |
| `error` | Red | Failures requiring action |

- Auto-dismiss after `5000ms` for info/success; do not auto-dismiss errors.
- Position: bottom-right (desktop), bottom (mobile).
- Stack up to 3; additional toasts replace oldest.

---

### 9. Table / Data Grid

- Use `<table>` with proper `<thead>`, `<tbody>`, `<th scope="col">`.
- Sortable columns: include `aria-sort` attribute.
- Row hover: subtle background highlight (`colorNeutralBackground1Hover`).
- Sticky header supported for long lists.

---

### 10. Avatar

- Sizes: `24px`, `32px`, `40px`, `48px`, `56px`, `72px`, `96px`, `120px`.
- Fallback order: photo → initials → generic icon.
- Shape: circle (people), square with rounded corners (entities/bots).

---

## Component Composition Rules

1. **One primary action** per view — never two primary buttons side-by-side.
2. Destructive actions (delete, remove) must be `subtle` or `outline` variant, never `primary`.
3. Disabled states must still be perceivable (do not hide them entirely).
4. Animations between component states should be ≤ `150ms`.

---

## References

- [Fluent 2 Components](https://fluent2.microsoft.design/components)
- [Fluent UI React v9](https://react.fluentui.dev/)
