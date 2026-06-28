# Material Design 3 — Components

> **Version:** Material Design 3 (M3) / Material You · Updated June 2026  
> **Source:** [m3.material.io/components](https://m3.material.io/components) · Google LLC  
> **Scope:** Full component library — anatomy, variants, states, specs, usage rules

---

## Table of Contents

1. [Component Anatomy Conventions](#component-anatomy-conventions)
2. [Action Components](#action-components)
   - [Buttons](#buttons)
   - [Floating Action Button (FAB)](#floating-action-button-fab)
   - [Icon Button](#icon-button)
   - [Segmented Button](#segmented-button)
   - [Button Groups (M3 Expressive)](#button-groups)
   - [Split Button (M3 Expressive)](#split-button)
3. [Communication Components](#communication-components)
   - [Badges](#badges)
   - [Progress Indicators](#progress-indicators)
   - [Snackbar](#snackbar)
   - [Tooltips](#tooltips)
4. [Containment Components](#containment-components)
   - [Cards](#cards)
   - [Dialogs](#dialogs)
   - [Bottom Sheets](#bottom-sheets)
   - [Side Sheets](#side-sheets)
5. [Navigation Components](#navigation-components)
   - [Navigation Bar](#navigation-bar)
   - [Navigation Drawer](#navigation-drawer)
   - [Navigation Rail](#navigation-rail)
   - [Top App Bar](#top-app-bar)
   - [Toolbars (M3 Expressive)](#toolbars)
   - [Tabs](#tabs)
6. [Selection Components](#selection-components)
   - [Checkbox](#checkbox)
   - [Chips](#chips)
   - [Date Picker](#date-picker)
   - [Menus](#menus)
   - [Radio Button](#radio-button)
   - [Slider](#slider)
   - [Switch](#switch)
   - [Time Picker](#time-picker)
7. [Text Input Components](#text-input-components)
   - [Text Fields](#text-fields)
   - [Search](#search)
8. [List Components](#list-components)
   - [Lists](#lists)
   - [Dividers](#dividers)
9. [Component States Reference](#component-states-reference)

---

## Component Anatomy Conventions

Every M3 component is described using a consistent anatomy:

- **Container** — The bounding shape that wraps the component
- **State layer** — An invisible overlay that changes opacity to communicate interaction state
- **Label** — The primary text of the component
- **Icon** — An optional leading or trailing icon
- **Supporting text** — Secondary descriptive text
- **Indicator** — A visual mark communicating selection or active state

**Layer ordering (bottom to top):** Container → Tonal surface → State layer → Content (icon + label) → Elevation shadow

---

## Action Components

### Buttons

Buttons trigger actions. M3 provides five button variants arranged by **visual emphasis**:

#### Variants Overview

| Variant | Emphasis | Container Fill | Usage |
|---|---|---|---|
| **Filled** | Highest | `primary` | Primary, most important action on a screen |
| **Filled Tonal** | High | `secondary-container` | Important actions without primary emphasis |
| **Elevated** | Medium | `surface` + elevation | Standalone actions that need separation |
| **Outlined** | Low | Transparent, `outline` border | Secondary, alternative actions |
| **Text** | Lowest | Transparent | Tertiary, inline actions, navigation |

#### Anatomy

```
┌─────────────────────────────────────┐
│  [optional icon]  [Label text]       │
└─────────────────────────────────────┘
     Leading icon    Label
```

| Element | Spec |
|---|---|
| Height | 40dp |
| Horizontal padding (no icon) | 24dp |
| Horizontal padding (with icon) | 16dp leading / 24dp trailing |
| Icon size | 18dp |
| Icon-label gap | 8dp |
| Shape | `shape.corner.full` (pill) |
| Label style | `label-large` |

#### Color Mapping by Variant

| Variant | Container | Label | Icon |
|---|---|---|---|
| Filled | `primary` | `on-primary` | `on-primary` |
| Filled Tonal | `secondary-container` | `on-secondary-container` | `on-secondary-container` |
| Elevated | `surface-container-low` | `primary` | `primary` |
| Outlined | Transparent | `primary` | `primary` |
| Text | Transparent | `primary` | `primary` |

#### Button States

| State | Layer Opacity | Additional Change |
|---|---|---|
| Enabled | 0% | — |
| Hovered | 8% (`on-*` color) | Elevation +1 (Elevated only) |
| Focused | 10% (`on-*` color) | Focus ring appears |
| Pressed | 10% (`on-*` color) | Ripple, Elevation −1 (Elevated only) |
| Disabled | Container: 12% on-surface; Label: 38% on-surface | No interaction |

#### Usage Rules

- Place the **highest-emphasis button** (Filled) last in a button row (trailing position).
- Limit one Filled button per view section — it signals the single primary action.
- Never use two Filled buttons next to each other.
- Filled Tonal is ideal for repeated actions in lists (e.g., "Add to cart").
- Text buttons inside cards or dialogs should align with the container edge.
- Minimum spacing between adjacent buttons: **8dp**.

---

### Floating Action Button (FAB)

FABs represent the primary or most common action in a view. They float above the UI surface.

#### Variants

| Variant | Size | Shape | Icon Size | Elevation |
|---|---|---|---|---|
| **FAB (default)** | 56×56dp | `shape.corner.large` (16dp) | 24dp | Level 3 |
| **Small FAB** | 40×40dp | `shape.corner.medium` (12dp) | 24dp | Level 3 |
| **Large FAB** | 96×96dp | `shape.corner.large` (28dp) | 36dp | Level 3 |
| **Extended FAB** | 56dp tall, variable width | `shape.corner.large` (16dp) | 24dp | Level 3 |

#### Extended FAB Anatomy

```
┌────────────────────────────────┐
│  [Icon]   [Label text]          │
└────────────────────────────────┘
   24dp       label-large
   Icon  8dp gap
```

- **Horizontal padding:** 16dp (icon side), 20dp (text side)
- **Extended FAB collapses to FAB** at compact breakpoints (optional behavior)

#### FAB Color

| Element | Token |
|---|---|
| Container | `primary-container` |
| Icon / Label | `on-primary-container` |
| State layer | `on-primary-container` |
| Shadow | `shadow` |

#### FAB Placement

- **Compact:** Bottom-right, 16dp from edges, above navigation bar
- **Medium:** Bottom-right, 16dp from edges, next to navigation rail
- **Expanded:** Anchor to bottom of navigation drawer or inline in content

#### FAB Usage Rules

- One FAB per screen maximum.
- FAB should represent the single most important action on a screen.
- Use Extended FAB on first launch or when the action needs labeling.
- Collapse to icon-only FAB as the user scrolls down in lists.
- Hide the FAB when a keyboard appears if it would overlap text input.

---

### Icon Button

Icon buttons perform actions represented by a single icon. They come in four variants.

| Variant | Container | Icon Color | Usage |
|---|---|---|---|
| **Standard** | None | `on-surface-variant` | Toolbar, app bar actions |
| **Filled** | `primary` | `on-primary` | High-emphasis icon action |
| **Filled Tonal** | `secondary-container` | `on-secondary-container` | Medium-emphasis |
| **Outlined** | Transparent, `outline` border | `on-surface-variant` | Toggle, selection |

#### Specs

| Property | Value |
|---|---|
| Container size | 40×40dp |
| Icon size | 24dp |
| Shape | `shape.corner.full` (circle for filled/tonal/outlined) |
| Touch target | 48×48dp |
| Toggle state | Fill changes; `FILL=1` for active |

#### Toggle Icon Button

Icon buttons can be **toggled** to represent on/off state. The icon and fill change between states:
- **Unselected:** `FILL=0`, `on-surface-variant` color
- **Selected:** `FILL=1`, `primary` color (for standard); `on-primary` (for filled)

---

### Segmented Button

Segmented buttons are a group of buttons that let users select one or multiple options. Used as an alternative to tabs for filtering or toggling views.

#### Variants

| Variant | Selection | Usage |
|---|---|---|
| **Single-select** | One option at a time | View mode, filter (mutually exclusive) |
| **Multi-select** | Multiple options | Filter (non-exclusive), attribute selection |

#### Anatomy

```
┌──────────────┬───────────────┬───────────────┐
│ ✓ [Icon] A  │    [Icon] B   │    [Icon] C   │
└──────────────┴───────────────┴───────────────┘
```

| Property | Value |
|---|---|
| Height | 40dp |
| Divider | 1dp `outline` |
| Shape | `shape.corner.full` (outer corners only) |
| Selected fill | `secondary-container` |
| Unselected fill | Transparent |
| Icon (selected) | Check + optional icon, `on-secondary-container` |
| Icon (unselected) | Icon only, `on-surface` |
| Label style | `label-large` |
| Min segment width | 48dp |
| Max segments | 5 |

#### Usage Rules

- Minimum 2, maximum 5 segments.
- All segments must have a label; icons are optional.
- Use for persistent, screen-level choices (not temporary pop-ups).
- Prefer tabs for primary navigation; use segmented buttons for filtering within a view.

---

### Button Groups

*New in M3 Expressive (2026)*

Button groups organize related buttons in a visually connected unit with shared shape and spacing. Buttons within a group **bump and morph** their shapes when adjacent buttons are pressed.

#### Specs

| Property | Value |
|---|---|
| Button height | 40dp or 56dp |
| Inter-button gap | 2dp |
| Outer shape | `shape.corner.full` (group ends) |
| Inner shape | `shape.corner.small` (shared edges) |
| Shape morph on press | Adjacent corners expand |
| Max buttons per group | 5 |

#### Usage Rules

- Use for 2–5 closely related actions.
- All buttons in a group should be the same variant (all filled or all tonal).
- Button groups are not selection controls — use segmented buttons for that.

---

### Split Button

*New in M3 Expressive (2026)*

A split button combines a primary action button with a dropdown arrow for related secondary actions. The two parts share a connected container.

#### Anatomy

```
┌──────────────────┬────┐
│  [Icon] Primary  │ ▼  │
└──────────────────┴────┘
  Primary action     Menu toggle
```

| Property | Value |
|---|---|
| Height | 40dp |
| Divider | 1dp vertical separator |
| Shape | `shape.corner.full` (outer), `shape.corner.none` (inner seam) |
| Dropdown | `shape.corner.medium` menu below button |

---

## Communication Components

### Badges

Badges are small status indicators attached to icons or buttons to communicate count or state.

#### Variants

| Variant | Size | Content | Usage |
|---|---|---|---|
| **Small (dot)** | 6×6dp | None | Binary indicator (new, unread) |
| **Large** | 16dp tall | 1–4 digit count | Numerical counts (notifications) |

#### Anatomy (Large)

```
       ┌─────┐
 Icon  │  3  │  ← Badge
       └─────┘
```

| Property | Value |
|---|---|
| Shape | `shape.corner.full` (pill) |
| Container color | `error` |
| Label color | `on-error` |
| Label style | `label-small` |
| Horizontal padding | 4dp |
| Overflow label | "999+" |
| Offset from icon | 2dp top-right of icon |

---

### Progress Indicators

Progress indicators communicate an ongoing process, either determinate (known duration) or indeterminate (unknown duration).

#### Variants

| Variant | Type | Orientation | Usage |
|---|---|---|---|
| **Linear** | Determinate or Indeterminate | Horizontal | Full-width page load, file upload |
| **Circular** | Determinate or Indeterminate | Radial | Inline with content, button loading |

#### Linear Progress Indicator

| Property | Value |
|---|---|
| Track height | 4dp |
| Shape | `shape.corner.full` |
| Active track color | `primary` |
| Track (background) color | `surface-container-highest` |
| Stop indicator | 4×4dp dot at end of determinate track |

**M3 Expressive update:** Linear indicators now support custom waveform and thickness to show progress with style. The indeterminate variant uses a three-segment animation.

#### Circular Progress Indicator

| Property | Value |
|---|---|
| Size (default) | 48dp |
| Size (small) | 24dp |
| Stroke width | 4dp (default), 3dp (small) |
| Active track color | `primary` |
| Track color | `surface-container-highest` |
| Rotation | Continuous, indeterminate: 4-phase animation |

#### Progress Indicator Usage Rules

- Prefer **circular** for inline/compact contexts; **linear** for page-level loading.
- Determinate is always preferred when progress can be calculated — it reduces perceived wait time.
- Always provide a text label for long-running operations.
- Hide progress indicators when operations complete in <1s to avoid flashing.

---

### Snackbar

Snackbars provide brief, low-priority feedback about an operation. They appear temporarily and do not require user action to dismiss (unless they contain an action).

#### Anatomy

```
┌─────────────────────────────────────────────────────┐
│  Supporting text message                [Action]    │
└─────────────────────────────────────────────────────┘
```

| Property | Value |
|---|---|
| Height | 48dp (single line), 68dp (two lines) |
| Width | 288–568dp; full-width on compact |
| Shape | `shape.corner.extra-small` (4dp) |
| Container color | `inverse-surface` |
| Text color | `inverse-on-surface` |
| Action color | `inverse-primary` |
| Label style | `body-medium` |
| Action style | `label-large` |
| Elevation | Level 3 |
| Duration | 4s (no action); 10s (with action); persistent (accessibility) |
| Max action text | 2 words recommended |
| Placement | Bottom-center (above navigation bar); leading-align on expanded |

#### Usage Rules

- Snackbars should not interrupt the user or require a decision.
- Only one snackbar at a time — queue subsequent snackbars.
- Actions should be an undo or related quick action, not "OK" or "Dismiss."
- Do not use snackbars for critical errors — use dialogs instead.

---

### Tooltips

Tooltips provide additional context for UI elements, appearing on hover, long press, or focus.

#### Variants

| Variant | Content | Usage |
|---|---|---|
| **Plain** | Short text (≤1 line) | Icon button labels |
| **Rich** | Title + body + optional action | Complex features, onboarding hints |

#### Plain Tooltip

| Property | Value |
|---|---|
| Container color | `inverse-surface` |
| Text color | `inverse-on-surface` |
| Text style | `body-small` |
| Padding | 4dp v / 8dp h |
| Shape | `shape.corner.extra-small` (4dp) |
| Max width | 200dp |
| Appear delay | 500ms (hover); 0ms (long press) |

#### Rich Tooltip

| Property | Value |
|---|---|
| Container color | `surface-container` |
| Title style | `title-small` |
| Body style | `body-medium` |
| Action style | `label-large`, `primary` color |
| Padding | 12dp |
| Shape | `shape.corner.medium` (12dp) |
| Max width | 320dp |

---

## Containment Components

### Cards

Cards group related content and actions about a single subject.

#### Variants

| Variant | Container | Elevation | Border | Usage |
|---|---|---|---|---|
| **Elevated** | `surface-container-low` | Level 1 | None | Standalone featured content |
| **Filled** | `surface-container-highest` | Level 0 | None | Dense, scannable lists |
| **Outlined** | `surface` | Level 0 | 1dp `outline` | Items requiring differentiation from background |

#### Anatomy

```
┌─────────────────────────────────────┐
│  [Media / Illustration]              │  ← Optional media area
│─────────────────────────────────────│
│  Headline                            │  ← title-large or headline-small
│  Subhead                             │  ← body-medium, on-surface-variant
│                                      │
│  [Body text content]                 │  ← body-medium
│                                      │
│                      [Action] [Action]│  ← Buttons / icon buttons
└─────────────────────────────────────┘
```

| Property | Value |
|---|---|
| Shape | `shape.corner.medium` (12dp) |
| Content padding | 16dp |
| Min height | 80dp |
| Media aspect ratio | 16:9 recommended |
| Card state layer | Applied over container color |

#### Card States

| State | Elevated | Filled | Outlined |
|---|---|---|---|
| Default | Level 1 | Level 0 | Level 0 |
| Hovered | Level 2 | Level 1 | Level 1 |
| Pressed | Level 1 | Level 0 | Level 0 |
| Dragging | Level 4 | Level 3 | Level 3 |

#### Usage Rules

- Cards should contain 2–3 content blocks maximum (media, text, actions).
- Never nest cards inside other cards.
- Cards in a list should be consistently sized within each context.
- Use elevated cards for priority items; filled cards for dense grids.

---

### Dialogs

Dialogs interrupt the user to request information, confirm actions, or communicate important messages.

#### Variants

| Variant | Usage |
|---|---|
| **Basic dialog** | Confirmation, simple information, single choice |
| **Full-screen dialog** | Complex forms requiring dedicated space |

#### Basic Dialog Anatomy

```
┌─────────────────────────────────────────────┐
│   [Optional icon]                            │
│   Headline text                              │
│─────────────────────────────────────────────│
│   Supporting text body content. This can     │
│   span multiple lines and include links.     │
│─────────────────────────────────────────────│
│                        [Cancel]  [Confirm]   │
└─────────────────────────────────────────────┘
```

| Property | Value |
|---|---|
| Width | 280dp min; 560dp max; centered |
| Shape | `shape.corner.extra-large` (28dp) |
| Container color | `surface-container-high` |
| Elevation | Level 3 |
| Padding | 24dp |
| Icon size | 24dp, optional |
| Icon color | `secondary` |
| Headline style | `headline-small` |
| Body style | `body-medium` |
| Action style | `label-large` (text buttons) |
| Scrim | 32% black |

#### Full-Screen Dialog

| Property | Value |
|---|---|
| Width | Full screen |
| Shape | None (0dp) |
| Header | Includes close icon + headline + primary action |
| Dismiss | Leading close button |
| Confirm | Trailing action (text or filled button) |

#### Dialog Usage Rules

- Use dialogs sparingly — they interrupt workflow.
- Limit to two actions maximum. The confirmatory action is placed at the trailing end.
- Dialogs with destructive actions should use `error` color on the confirmatory button.
- Dialogs should not contain another dialog (no nesting).
- Full-screen dialogs are for complex tasks that need their own screen context.

---

### Bottom Sheets

Bottom sheets display supplementary content anchored to the bottom of the screen.

#### Variants

| Variant | Behavior | Usage |
|---|---|---|
| **Modal** | Blocks content behind scrim; must be dismissed | Lists of actions, sharing menus |
| **Standard** | Coexists with content; always visible | Persistent controls, map detail |

#### Anatomy

```
 ─────────────────────────────────
 ── [Drag handle] ────────────────   ← 4×32dp, centered, 16dp from top
  [Header / Headline text]           ← optional
  ─────────────────────────────────
  [Content]
  ─────────────────────────────────
```

| Property | Value |
|---|---|
| Shape | `shape.corner.extra-large-top` (28dp top corners only) |
| Container color | `surface-container-low` |
| Elevation | Level 1 |
| Drag handle | 4dp tall × 32dp wide, `on-surface-variant` 40% opacity |
| Drag handle margin | 22dp top |
| Min peek height | 56dp |
| Max height | Full screen − status bar |
| Scrim (modal only) | 32% black |

---

### Side Sheets

Side sheets display supplementary content anchored to the trailing edge of the screen. Used on medium and expanded breakpoints.

#### Variants

| Variant | Behavior |
|---|---|
| **Modal** | Overlays content with scrim |
| **Standard** | Coexists with content |

| Property | Value |
|---|---|
| Width | 256–400dp |
| Shape | `shape.corner.large` on leading edge (16dp) |
| Container color | `surface-container-low` |
| Elevation | Level 1 (standard); Level 2 (modal) |

---

## Navigation Components

### Navigation Bar

The navigation bar is the primary navigation destination for 3–5 top-level destinations on compact screens.

#### Anatomy

```
┌──────────────────────────────────────────────────────┐
│  ┌───────┐   ┌───────┐   ┌───────┐   ┌───────┐      │
│  │  ●    │   │       │   │       │   │       │      │
│  │  Icon │   │  Icon │   │  Icon │   │  Icon │      │
│  │ Label │   │ Label │   │ Label │   │ Label │      │
│  └───────┘   └───────┘   └───────┘   └───────┘      │
└──────────────────────────────────────────────────────┘
 Active dest   Inactive destinations
```

| Property | Value |
|---|---|
| Height | 80dp |
| Container color | `surface-container` |
| Indicator shape | `shape.corner.full` (pill) |
| Indicator size | 64dp wide × 32dp tall |
| Indicator color | `secondary-container` |
| Active icon color | `on-secondary-container` |
| Active label color | `on-surface` |
| Inactive icon color | `on-surface-variant` |
| Inactive label color | `on-surface-variant` |
| Icon size | 24dp |
| Label style | `label-medium` |
| Destinations | 3–5 |
| Badge support | Yes (dot and count) |

---

### Navigation Drawer

Navigation drawers provide access to 5+ top-level destinations or nested navigation structures.

#### Variants

| Variant | Behavior | Breakpoint |
|---|---|---|
| **Modal** | Slide in from left, scrim behind | Compact, Medium |
| **Standard** | Always visible alongside content | Expanded |
| **Bottom** | Rare; large-screen variant | Expanded (specific cases) |

#### Anatomy

```
┌────────────────────────────────────┐
│  [Header: Avatar + Account info]    │  ← Optional
│────────────────────────────────────│
│  ● Active Destination               │  ← Active indicator
│    Inactive Destination             │
│    Inactive Destination             │
│────────────────────────────────────│
│    Section Label                    │
│    Destination                      │
│    Destination                      │
└────────────────────────────────────┘
```

| Property | Value |
|---|---|
| Width | 360dp (standard); full-screen on compact |
| Shape | `shape.corner.large-end` (16dp on trailing edge) |
| Container color | `surface-container-low` |
| Active indicator | `secondary-container` (336dp × 56dp) |
| Active icon color | `on-secondary-container` |
| Active label color | `on-secondary-container` |
| Inactive icon color | `on-surface-variant` |
| Inactive label color | `on-surface-variant` |
| Label style | `label-large` |
| Section header style | `title-small` |
| Horizontal padding | 16dp |
| Vertical item padding | 16dp (56dp row height) |

---

### Navigation Rail

Navigation rails provide primary navigation for medium-breakpoint layouts (600–839dp). They appear as a vertical strip on the leading edge.

#### Anatomy

```
│ ┌───────┐ │
│ │  FAB   │ │  ← Optional leading FAB
│ └───────┘ │
│────────────│
│   [●]      │  ← Active indicator
│   Icon     │
│   Label    │
│────────────│
│   Icon     │
│   Label    │
│   Icon     │
│   Label    │
```

| Property | Value |
|---|---|
| Width | 80dp |
| Shape | None (rectangular) |
| Container color | `surface-container` |
| Active indicator | `secondary-container` (56dp × 32dp, pill) |
| Active icon color | `on-secondary-container` |
| Active label color | `on-surface` |
| Inactive icon color | `on-surface-variant` |
| Icon size | 24dp |
| Label style | `label-medium` |
| Destinations | 3–7 |

---

### Top App Bar

Top app bars display the current screen title and actions.

#### Variants

| Variant | Height | Title Position | Collapse |
|---|---|---|---|
| **Center-aligned** | 64dp | Centered | Scrolling collapse supported |
| **Small** | 64dp | Leading | Scrolling collapse supported |
| **Medium** | 112dp | Bottom-leading | Collapses to Small on scroll |
| **Large** | 152dp | Bottom-leading | Collapses to Small on scroll |

#### Anatomy

```
┌─────────────────────────────────────────────────────┐
│  [Nav icon]     Title / Headline         [Actions]  │
└─────────────────────────────────────────────────────┘
```

| Property | Value |
|---|---|
| Container color | `surface` (default); `surface-container` (scrolled) |
| Title style (Small/Center) | `title-large` |
| Title style (Medium) | `headline-small` |
| Title style (Large) | `headline-medium` |
| Navigation icon size | 24dp |
| Action icon size | 24dp |
| Horizontal padding | 4dp (icons); 16dp (title) |
| Icon color | `on-surface` |
| Title color | `on-surface` |

#### Scroll Behavior

- **Lifted:** On scroll, the top app bar gains elevation (`surface-container`), creating separation.
- **Compressed:** Medium/Large variants collapse title to Small position on scroll.

---

### Toolbars

*New in M3 Expressive (2026)*

Toolbars are flexible containers for frequently used actions. They hold a variety of controls including buttons, icon buttons, and can be paired with a FAB.

| Property | Value |
|---|---|
| Height | 64dp (standard); 80dp (large) |
| Shape | `shape.corner.full` or `shape.corner.extra-large` |
| Container color | `surface-container` |
| Padding | 12dp horizontal |

Toolbars support:
- **Horizontal scroll** for overflow actions
- **FAB pairing** with connected shape treatment
- **Contextual mode** for selection-dependent actions

---

### Tabs

Tabs organize content at the same level of hierarchy. They are placed within the app bar or below it.

#### Variants

| Variant | Indicator | Scrolling |
|---|---|---|
| **Primary (fixed)** | Bottom bar | No (equal-width) |
| **Primary (scrollable)** | Bottom bar | Horizontal scroll |
| **Secondary (fixed)** | Bottom bar | No |
| **Secondary (scrollable)** | Bottom bar | Horizontal scroll |

#### Anatomy

```
┌───────────┬───────────┬───────────┐
│    Tab 1   │ ● Tab 2   │    Tab 3  │
│            │───────────│           │
└───────────┴───────────┴───────────┘
                Active tab indicator
```

| Property | Value |
|---|---|
| Height | 48dp (icon only or label only); 64dp (icon + label) |
| Active indicator height | 3dp (primary); 2dp (secondary) |
| Active indicator color | `primary` |
| Active label/icon color | `primary` |
| Inactive label/icon color | `on-surface-variant` |
| Label style | `title-small` |
| Min tab width | 90dp |
| Max tab width | 360dp |

---

## Selection Components

### Checkbox

Checkboxes allow users to select one or more items from a set, or toggle a single item on or off.

#### States

| State | Fill | Icon |
|---|---|---|
| Unchecked | Transparent | None |
| Checked | `primary` | White checkmark |
| Indeterminate | `primary` | White dash |
| Error (unchecked) | Transparent | `error` border |
| Error (checked) | `error` | White checkmark |

| Property | Value |
|---|---|
| Container size | 18×18dp |
| Touch target | 48×48dp |
| Shape | `shape.corner.extra-small` (2dp) |
| Border width | 2dp (unchecked) |
| State layer size | 40×40dp |

---

### Chips

Chips are compact elements representing inputs, attributes, or actions.

#### Variants

| Variant | Purpose | Selectable | Removable |
|---|---|---|---|
| **Assist** | Smart actions (smart suggestions) | No | No |
| **Filter** | Narrow content, toggle filtering options | Yes | Optional |
| **Input** | Represent entered values (tokens) | No | Yes |
| **Suggestion** | Offer completions or autocomplete | No | No |

#### Anatomy

```
┌─────────────────────────────────┐
│  [Icon]  Label text  [Trailing] │
└─────────────────────────────────┘
```

| Property | Value |
|---|---|
| Height | 32dp |
| Horizontal padding | 8dp (with icon), 16dp (without icon) |
| Shape | `shape.corner.small` (8dp) |
| Container (default) | `surface-container-low` + `outline` border |
| Container (selected) | `secondary-container` |
| Label (default) | `on-surface` |
| Label (selected) | `on-secondary-container` |
| Icon size | 18dp |
| Label style | `label-large` |
| Trailing icon size | 18dp |

---

### Date Picker

Date pickers let users select a date (or date range).

#### Variants

| Variant | Layout | Usage |
|---|---|---|
| **Docked** | Calendar inline with content | Primary in forms |
| **Modal** | Calendar in a dialog | Full selection with header |
| **Modal input** | Text input in a dialog | Direct date entry |

#### Modal Date Picker Anatomy

```
┌───────────────────────────────────┐
│  [Input field]     [Select year]   │
│────────────────────────────────────│
│  Mo  Tu  We  Th  Fr  Sa  Su       │
│   1   2   3   4   5   6   7       │
│   8   9  10  11  12  13  14       │
│  15  16  17  18  19  20  21       │
│  22  23  24  25  26  27  28       │
│────────────────────────────────────│
│                  [Cancel]  [OK]    │
└───────────────────────────────────┘
```

| Property | Value |
|---|---|
| Width | 328dp |
| Shape | `shape.corner.extra-large` (28dp) |
| Selected date | `primary` circle |
| Today | `primary` text color |
| Range fill | `primary-container` |
| Header style | `label-large` |
| Day style | `body-large` |

---

### Menus

Menus display a list of choices triggered by an action.

#### Variants

| Variant | Usage |
|---|---|
| **Dropdown menu** | Selection from a list (overflow or context actions) |
| **Exposed dropdown** | Text field variant showing current selection |
| **Context menu** | Right-click / long-press contextual actions |

#### Dropdown Menu

| Property | Value |
|---|---|
| Min width | 112dp |
| Max width | 280dp |
| Shape | `shape.corner.extra-small` (4dp) |
| Container color | `surface-container` |
| Elevation | Level 2 |
| Item height | 48dp |
| Item padding | 12dp v / 16dp h |
| Label style | `body-large` |
| Leading icon size | 24dp |
| Trailing icon / checkmark | 24dp |
| Divider thickness | 1dp `surface-variant` |

---

### Radio Button

Radio buttons allow users to select exactly one option from a set.

| Property | Value |
|---|---|
| Outer circle size | 20dp |
| Inner circle size | 10dp (selected) |
| Touch target | 48×48dp |
| Selected color | `primary` |
| Unselected color | `on-surface-variant` |
| State layer size | 40×40dp |

---

### Slider

Sliders allow users to make selections from a range of values.

#### Variants

| Variant | Description |
|---|---|
| **Continuous** | Any value within the range |
| **Discrete** | Predefined set of values (tick marks) |
| **Range** | Two handles for min/max selection |

| Property | Value |
|---|---|
| Track height | 4dp (inactive); 4dp (active) |
| Handle size | 20dp circle |
| Active track color | `primary` |
| Inactive track color | `surface-container-highest` |
| Handle color | `primary` |
| Handle shape | `shape.corner.full` |
| Tick mark size | 4dp (discrete) |
| Value label | Tooltip above handle (optional, `primary-container`) |
| Touch target | 48dp tall |
| Min track width | 4dp stop indicator at ends |

---

### Switch

Switches toggle a single boolean setting on or off.

| Property | Value |
|---|---|
| Track size | 52×32dp |
| Handle size | 16dp (off); 24dp (on) |
| Track color (on) | `primary` |
| Track color (off) | `surface-container-highest` + `outline` border |
| Handle color (on) | `on-primary` |
| Handle color (off) | `outline` |
| With icon (on) | 24dp checkmark icon |
| With icon (off) | 16dp × icon |
| Touch target | 52×48dp |

---

### Time Picker

Time pickers let users select a time.

#### Variants

| Variant | Usage |
|---|---|
| **Dial** | Clock face interaction, default |
| **Input** | Text field for direct time entry |

| Property | Value |
|---|---|
| Dial diameter | 256dp |
| Shape (container) | `shape.corner.extra-large` (28dp) |
| Active segment color | `primary-container` |
| Period toggle | `on-surface-variant` |

---

## Text Input Components

### Text Fields

Text fields let users enter and edit text.

#### Variants

| Variant | Container | Usage |
|---|---|---|
| **Filled** | `surface-container-highest` with bottom border only | Default; forms, search |
| **Outlined** | Transparent with full `outline` border | Emphasized, dense forms |

#### Anatomy

```
┌───────────────────────────────────────────┐
│ [Leading icon]  Label text  [Trailing icon]│  ← Label floats on focus/fill
│  Input text value                          │
│─────────────────────────────────────────── │  ← Active indicator (bottom border)
│ Supporting text / Error message            │
└───────────────────────────────────────────┘
```

| Property | Value |
|---|---|
| Height | 56dp (without supporting text) |
| Shape (filled) | `shape.corner.extra-small-top` (4dp top corners) |
| Shape (outlined) | `shape.corner.extra-small` (4dp) |
| Active indicator height | 2dp (focused) |
| Active indicator color | `primary` |
| Label (unfocused) | `body-large`, `on-surface-variant` |
| Label (focused / filled) | `body-small`, `primary` |
| Input style | `body-large`, `on-surface` |
| Supporting text style | `body-small`, `on-surface-variant` |
| Error state | `error` active indicator, `error` label, `error` supporting text |
| Leading icon size | 24dp |
| Trailing icon size | 24dp |
| Horizontal padding | 16dp |

#### Text Field States

| State | Active Indicator | Label | Border (Outlined) |
|---|---|---|---|
| Enabled (empty) | 1dp | Body-large, floating center | 1dp `outline` |
| Focused | 2dp `primary` | Body-small, top, `primary` | 2dp `primary` |
| Error | 2dp `error` | Body-small, top, `error` | 2dp `error` |
| Disabled | 1dp, 38% opacity | 38% opacity | 1dp, 12% on-surface |

---

### Search

Search allows users to explore content. M3 provides two search patterns.

#### Variants

| Variant | Usage |
|---|---|
| **Search bar** | Persistent top-of-screen search, replaces or supplements app bar |
| **Search view** | Expanded search experience with results and suggestions |

#### Search Bar

```
┌─────────────────────────────────────────────────┐
│  🔍   Search...                        [Avatar] │
└─────────────────────────────────────────────────┘
```

| Property | Value |
|---|---|
| Height | 56dp |
| Shape | `shape.corner.full` |
| Container color | `surface-container-high` |
| Placeholder style | `body-large`, `on-surface-variant` |
| Leading icon | Search icon or navigation icon |
| Trailing | Avatar, clear, or mic |
| Elevation | Level 0 (flat); Level 2 (floating) |

#### Search View

Expands from the search bar to fill the screen:
- Shows search history and suggestions above the keyboard
- Typing updates results in real time
- Back navigation collapses to the search bar

---

## List Components

### Lists

Lists display related items in a vertical arrangement.

#### Variants (by line count)

| Variant | Height | Content |
|---|---|---|
| **One line** | 56dp | Headline only |
| **Two line** | 72dp | Headline + supporting text |
| **Three line** | 88dp | Headline + 2 lines supporting text |

#### Anatomy

```
┌────────────────────────────────────────────────────┐
│  [Leading]  Headline text              [Trailing]  │
│             Supporting text                        │
└────────────────────────────────────────────────────┘
  Avatar/     Body-large / Label-large    Icon/
  Icon/                                  Switch/
  Checkbox                               Checkbox
```

| Property | Value |
|---|---|
| Horizontal padding | 16dp |
| Vertical padding | 8dp |
| Leading element size | 40dp (avatar) / 24dp (icon) / 18dp (checkbox) |
| Leading-to-content gap | 16dp |
| Content-to-trailing gap | 16dp |
| Headline style | `body-large` |
| Supporting text style | `body-medium`, `on-surface-variant` |
| Trailing icon size | 24dp |

#### Usage Rules

- Lists should not exceed 3 lines — use a detail view for longer content.
- Consistent leading elements within a list group.
- Three-line lists should left-align trailing elements to the top.
- Interactive list items should have 48dp minimum touch height.

---

### Dividers

Dividers visually separate content within lists, forms, and layouts.

#### Variants

| Variant | Usage |
|---|---|
| **Full-width** | Between major layout sections |
| **Inset** | Within lists, aligned with content left edge |
| **Middle inset** | Both leading and trailing insets |

| Property | Value |
|---|---|
| Thickness | 1dp |
| Color | `outline-variant` |
| Inset offset | 16dp (standard) / 72dp (with avatar) |

---

## Component States Reference

All interactive M3 components support the following interaction states. State layers are transparent overlays using the component's `on-*` color.

| State | Layer Opacity | Description |
|---|---|---|
| **Default** | 0% | Component at rest |
| **Hovered** | 8% | Pointer hovering over the component |
| **Focused** | 10% | Component has keyboard/accessibility focus |
| **Pressed** | 10% | Component being tapped or clicked |
| **Dragged** | 16% | Component being dragged (elevated cards, list reorder) |
| **Selected** | 0% + visual indicator | Component in a selected state |
| **Activated** | 0% + indicator fill | Active navigation destination |
| **Disabled** | Container: 12% on-surface; Content: 38% on-surface | Not interactable |

### State Layer Application

```css
/* Example: Hover state on a filled button */
.md-filled-button:hover::after {
  content: '';
  background-color: var(--md-sys-color-on-primary);
  opacity: 0.08;
  border-radius: inherit;
  position: absolute;
  inset: 0;
}
```

### Focus Ring Specification

All focused elements display a visible focus ring:

| Property | Value |
|---|---|
| Color | `secondary` |
| Width | 3dp |
| Offset | 2dp from component edge |
| Shape | Matches component shape |

---

*Last updated: June 2026 · Material Design 3 Expressive*
