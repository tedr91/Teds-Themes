# Apple Design System — components.md
> Aligned to Apple Human Interface Guidelines · iOS 18 / iPadOS 18 / macOS Sequoia / visionOS 2

---

## Table of Contents

1. [Navigation Components](#1-navigation-components)
2. [Bars](#2-bars)
3. [Buttons & Controls](#3-buttons--controls)
4. [Menus & Pickers](#4-menus--pickers)
5. [Text Inputs](#5-text-inputs)
6. [Lists & Tables](#6-lists--tables)
7. [Presentations](#7-presentations)
8. [Feedback & Status](#8-feedback--status)
9. [Content Components](#9-content-components)
10. [Layout Containers](#10-layout-containers)
11. [Component Platform Matrix](#11-component-platform-matrix)

---

## 1. Navigation Components

### 1.1 Navigation Bar

The navigation bar sits at the top of a hierarchical view and provides title, back button, and contextual actions.

**Anatomy**
```
[ Back Button ] [ Large Title / Inline Title ] [ Leading Actions ] [ Trailing Actions ]
```

**Title Display Modes**

| Mode | When to Use |
|---|---|
| **Large** (`displayModeAlways`) | Top-level destination in a tab; content-first views |
| **Inline** (`displayModeNever`) | Sub-levels in a hierarchy; narrow titles; compact height |
| **Automatic** | System switches between large/inline based on scroll position |

**Rules**
- Back button label uses the **title of the parent view** (truncated to "Back" if too long).
- Limit trailing actions to **3 icons max** in a navigation bar; overflow into a menu (`...`).
- On macOS, navigation bars map to unified toolbar + title bar.
- Do not place a search bar in the navigation bar permanently — use `searchable()` which integrates below the bar.

**SwiftUI Implementation**
```swift
NavigationStack {
    ContentView()
        .navigationTitle("Inbox")
        .navigationBarTitleDisplayMode(.large)
        .toolbar {
            ToolbarItem(placement: .primaryAction) {
                Button("Compose", systemImage: "square.and.pencil") { }
            }
        }
}
```

---

### 1.2 Tab Bar

The tab bar offers top-level navigation to 2–5 distinct sections of an app.

**Anatomy**
```
[ Tab 1 Icon + Label ] [ Tab 2 Icon + Label ] [ Tab 3 Icon + Label ] [ Tab 4 Icon + Label ] [ Tab 5 Icon + Label ]
```

**Rules**
- Each tab must represent a distinct, peer-level destination (not hierarchical).
- Always show a badge for unread/pending counts; never show a badge > 99 (use 99+).
- Icon + label required for all tabs; never label only or icon only.
- In iOS 18+, support tab bar customization: users add, remove, and reorder tabs. Provide a `tabViewCustomization` configuration.
- On iPadOS in regular width, the tab bar transforms into a **sidebar** automatically.
- On visionOS, tabs are placed in a vertical **ornament** on the leading edge of the window.

**SwiftUI Implementation**
```swift
TabView {
    Tab("Home", systemImage: "house") {
        HomeView()
    }
    Tab("Search", systemImage: "magnifyingglass") {
        SearchView()
    }
    Tab("Library", systemImage: "books.vertical") {
        LibraryView()
    }
}
.tabViewStyle(.tabBarOnly)
```

---

### 1.3 Sidebar

The sidebar provides hierarchical or categorical navigation in multi-column layouts.

**Usage**
- iPadOS (regular width) and macOS: primary navigation panel.
- Width: 200–320 pt; collapsible via toggle or swipe.
- Source list style on macOS (`List(selection:).listStyle(.sidebar)`).
- Support disclosure groups for nested categories.

**Sidebar Row Anatomy**
```
[ Icon (optional) ] [ Label ] [ Badge / Count (optional) ] [ Disclosure (if nested) ]
```

**Rules**
- Selection state: selected row uses system tint background fill.
- Never remove the sidebar toggle button — users must be able to show/hide.
- On iPhone (compact width), the sidebar collapses entirely; navigation falls back to tab bar or navigation stack.

---

### 1.4 Navigation Split View

Three-column layout for complex apps on iPadOS and macOS.

| Column | Role | Width |
|---|---|---|
| Sidebar | Category / source list | 220–300 pt |
| Content | Item list within category | 320–420 pt |
| Detail | Full detail of selected item | Remaining |

Behavior: On compact, collapses to a single NavigationStack. Columns collapse from trailing to leading.

---

## 2. Bars

### 2.1 Toolbar (macOS)

The unified toolbar sits above the content area in macOS windows.

**Standard Layout**
```
[ Traffic Lights ] [ Title ] [ Flexible Space ] [ Actions ] [ Search ]
```

**Rules**
- Use `NSToolbarFlexibleSpaceItem` to push controls to the trailing edge.
- Support user customization via `NSToolbar(identifier:)` with `allowsUserCustomization = true`.
- Toolbar icons: SF Symbols at small scale, labeled when space permits.
- Destructive actions (delete) in toolbar must show confirmation before executing.

---

### 2.2 Status Bar (iOS)

System-owned bar at the top of the screen. Apps control style only.

| Style | Background | Text/Icons |
|---|---|---|
| `lightContent` | Dark views | White |
| `darkContent` | Light views | Black |
| Auto | System decides | System decides |

- Never obscure the status bar with app content unless entering full-screen video/image mode.
- Notch and Dynamic Island areas are system-reserved — do not place controls there.

---

### 2.3 Search Bar

A persistent or transient text field for filtering content.

**States**
1. Inactive — placeholder text visible
2. Active (focused) — keyboard appears, Cancel button appears
3. Active (has text) — clear button (✕) appears in trailing edge of field

**Rules**
- Use `searchable(text:)` modifier to attach search to a `NavigationStack` or `List`.
- Scope buttons appear below the search bar when 2–4 scopes are provided.
- Token-based search (`SearchToken`) allows structured search with typed filters (e.g., "From: Alice").
- Never place a search bar inside a scroll view manually; let the system handle positioning.

---

## 3. Buttons & Controls

### 3.1 Buttons

**Button Styles**
| Style | Visual | Use Case |
|---|---|---|
| `bordered` | Background fill, rounded rect | Primary actions in forms |
| `borderedProminent` | Solid tint fill | Single primary CTA per view |
| `borderless` | Text only | Tertiary actions, inline |
| `plain` | No background or border | Custom contexts |

**Button Sizes**
| Size | Height | Font |
|---|---|---|
| `large` | 50 pt | `.headline` |
| `medium` (default) | 34 pt | `.body` |
| `small` | 26 pt | `.subheadline` |
| `mini` | 22 pt | `.caption` |

**Rules**
- One `borderedProminent` button per view maximum.
- Disabled state: reduce opacity to 0.5, preserve layout.
- Destructive buttons: use `.destructive` role → red tint automatically applied.
- Loading state: show a `ProgressView` inside the button; disable the button while loading.
- Icon-only buttons must include an accessibility label.
- Minimum tap target: 44 × 44 pt (expand with `contentShape`).

---

### 3.2 Toggle

A binary switch for on/off states.

```swift
Toggle("Notifications", isOn: $notificationsEnabled)
    .toggleStyle(.switch)   // iOS default: pill-shaped switch
```

| Style | Platform | Appearance |
|---|---|---|
| `switch` | iOS, iPadOS, macOS | Pill-shaped, tint when on |
| `checkbox` | macOS | Square checkbox |
| `button` | All | Toggles button filled state |

**Rules**
- Label must describe the feature being toggled, not the action ("Notifications" not "Enable Notifications").
- Toggles take effect immediately; do not require a Save button.
- In a list, place the toggle as the trailing view of the row.

---

### 3.3 Slider

Continuous value selection within a range.

```swift
Slider(value: $brightness, in: 0...1, step: 0.01) {
    Label("Brightness", systemImage: "sun.max")
}
```

**Rules**
- Always show current value or a visual proxy (e.g., icon scale changes).
- Provide min/max label images (leading/trailing) to indicate direction.
- Step value should produce a reasonable number of distinct positions (20–100 steps max).
- Support haptic ticks for step sliders.

---

### 3.4 Stepper

Increment/decrement control for discrete integer or floating-point values.

```swift
Stepper("Quantity: \(quantity)", value: $quantity, in: 1...99)
```

**Rules**
- Display the current value in the label — the stepper itself shows no value.
- Use steppers for values where exact discrete control is important (font size, item count).
- For fine-grained continuous values, prefer a slider.

---

### 3.5 Segmented Control

Mutually exclusive selection across 2–5 options displayed inline.

```swift
Picker("View", selection: $selectedView) {
    Text("List").tag(0)
    Text("Grid").tag(1)
    Text("Map").tag(2)
}
.pickerStyle(.segmented)
```

**Rules**
- Labels must be short (1–2 words). Icons alone are acceptable if universally understood.
- All segments should have approximately equal width.
- Maximum 5 segments; beyond that, use a Picker or Menu.
- Do not use a segmented control to navigate between views — use tab bar or navigation for that.

---

## 4. Menus & Pickers

### 4.1 Context Menu

A short list of actions that appear on long press (iOS) or right-click (macOS/iPadOS).

```swift
Image("photo")
    .contextMenu {
        Button("Copy", systemImage: "doc.on.doc") { }
        Button("Share…", systemImage: "square.and.arrow.up") { }
        Divider()
        Button("Delete", systemImage: "trash", role: .destructive) { }
    } preview: {
        Image("photo").resizable().scaledToFit()
    }
```

**Rules**
- Limit to 5–7 items; use submenus for extended sets.
- Group related actions with `Divider()`.
- Destructive actions go last, below a divider.
- Provide a preview image when the context menu target is visual content.
- Never require a context menu for primary actions; it supplements, never replaces.

---

### 4.2 Menu (Pull-Down)

A button that reveals a list of choices or commands.

```swift
Menu("Options", systemImage: "ellipsis.circle") {
    Button("Edit") { }
    Button("Duplicate") { }
    Divider()
    Button("Delete", role: .destructive) { }
}
```

**Rules**
- The button label indicates the menu's purpose, not "Menu".
- Icons in menu items should be consistent (all SF Symbols or none).
- Use `primaryAction` parameter if the button has a default action when tapped directly; menu appears on long press.

---

### 4.3 Picker

Selection of a single option from a list.

| Style | Appearance | Best For |
|---|---|---|
| `automatic` | System chooses | General use |
| `segmented` | Inline segmented control | 2–5 short options |
| `wheel` | Spinning drum | Date/time, compact lists |
| `menu` | Dropdown button | Many options in compact space |
| `navigationLink` | Row that pushes detail | Long lists in navigation context |
| `inline` | Expanded in-place | Settings, forms |

---

### 4.4 Date Picker

Specialized picker for dates, times, or date-time combinations.

| Style | Description |
|---|---|
| `compact` | Single button; calendar drops down on tap |
| `graphical` | Full inline calendar grid |
| `wheel` | Scrollable drums for date components |
| `field` | Text field with formatted date (macOS) |

---

### 4.5 Color Picker

System-provided color selection UI.

```swift
ColorPicker("Accent Color", selection: $selectedColor, supportsOpacity: false)
```

Expands to a full-page color swatch grid, spectrum picker, and hex input panel. Appears as a compact button in forms; full UI in a popover/sheet.

---

## 5. Text Inputs

### 5.1 Text Field

Single-line text input.

```swift
TextField("Email", text: $email)
    .textContentType(.emailAddress)
    .keyboardType(.emailAddress)
    .submitLabel(.next)
```

**Content Types** (critical for AutoFill)
- `.username`, `.password`, `.newPassword`
- `.emailAddress`, `.telephoneNumber`
- `.name`, `.givenName`, `.familyName`
- `.streetAddressLine1`, `.postalCode`
- `.creditCardNumber`, `.creditCardExpiration`
- `.oneTimeCode` — triggers SMS/email code AutoFill

**Rules**
- Always set `textContentType` to enable AutoFill.
- Set `keyboardType` appropriate to the expected input.
- Use `submitLabel` to set the Return key label: `.next`, `.done`, `.search`, `.send`, `.go`.
- Provide clear error messaging adjacent to the field, not in an alert.
- Placeholder is **not** a replacement for a label; always provide a persistent label above.

---

### 5.2 Secure Field

Password input with hidden characters.

```swift
SecureField("Password", text: $password)
    .textContentType(.password)
```

Always pair with a show/hide toggle button (eye icon) for accessibility.

---

### 5.3 Text Editor

Multi-line text input for longer content.

```swift
TextEditor(text: $notes)
    .frame(minHeight: 120)
```

**Rules**
- Always provide a visible border or background to indicate the editable region.
- For rich text, use `AttributedString` with `TextEditor` or `UITextView`/`NSTextView`.
- iOS 18 Writing Tools integration is automatic for `TextEditor`; do not disable unless there is a strong reason.

---

### 5.4 Search Field

See [Search Bar (2.3)](#23-search-bar). Use `searchable()` rather than placing a text field manually.

---

## 6. Lists & Tables

### 6.1 List

The primary component for displaying rows of related content.

**List Styles**
| Style | Appearance | Platform |
|---|---|---|
| `insetGrouped` | Cards in groups, inset from edges | iOS default |
| `grouped` | Full-width groups | iOS (legacy) |
| `inset` | Full-width, inset rows | iOS |
| `plain` | No grouping, separator lines | iOS, macOS |
| `sidebar` | Icon + label, disclosure | macOS, iPadOS |

**Swipe Actions**
```swift
List {
    ForEach(items) { item in
        ItemRow(item: item)
            .swipeActions(edge: .trailing, allowsFullSwipe: true) {
                Button("Delete", role: .destructive) { delete(item) }
            }
            .swipeActions(edge: .leading) {
                Button("Star", systemImage: "star") { star(item) }
                    .tint(.yellow)
            }
    }
    .onMove { move($0, $1) }
    .onDelete { delete($0) }
}
.listStyle(.insetGrouped)
```

**Rules**
- Full-swipe (destructive) only for the primary trailing action.
- Show a maximum of **3 swipe actions per edge**.
- Edit mode (`.onMove`, `.onDelete`) requires explicit entry via Edit button.
- Section headers use `.footnote` size in uppercase or title case depending on style.
- Section footers provide clarifying guidance, not labels for the section.

---

### 6.2 Table (macOS / iPadOS)

Multi-column sortable table view.

```swift
Table(items, selection: $selection, sortOrder: $sortOrder) {
    TableColumn("Name", value: \.name)
    TableColumn("Date", value: \.date) { item in
        Text(item.date, style: .date)
    }
    TableColumn("Size") { item in
        Text(item.formattedSize)
    }
}
```

**Rules**
- Column headers must be tappable/clickable to sort.
- Show a sort direction indicator (chevron) in the active sort column header.
- Support multi-row selection on iPadOS with Shift+tap and Command+tap.
- Empty state: show a centered message, not an empty table with headers.

---

### 6.3 Grid (Collection View)

Uniform or variable-size item grid.

```swift
LazyVGrid(columns: [GridItem(.adaptive(minimum: 160, maximum: 200), spacing: 12)], spacing: 12) {
    ForEach(items) { item in
        ItemCard(item: item)
    }
}
```

**Rules**
- Use `.adaptive` column sizing so grid responds to width changes automatically.
- Minimum item size should ensure ≥ 2 columns at narrowest width.
- Provide selection feedback with a checkmark badge and highlighted border.
- Long press → selection mode (similar to Photos app).

---

## 7. Presentations

### 7.1 Sheet

A modal view that slides up from the bottom (iOS) or attaches to the parent (macOS).

**iOS Sheet Detents**
```swift
.sheet(isPresented: $showSheet) {
    SheetContent()
        .presentationDetents([.medium, .large])
        .presentationDragIndicator(.visible)
        .presentationCornerRadius(20)
        .presentationBackground(.regularMaterial)
}
```

| Detent | Description |
|---|---|
| `.medium` | Half-screen |
| `.large` | Full-screen (default) |
| `.fraction(0.3)` | Custom fraction of screen height |
| `.height(240)` | Fixed height |

**Rules**
- Provide a drag indicator for sheets with multiple detents.
- Do not put critical navigation (back/close) only in a sheet; users expect to dismiss by dragging.
- Use `.interactiveDismissDisabled(true)` only for unsaved-data warnings; provide an explicit Dismiss/Cancel button.
- Nested sheets (sheet on sheet) are unsupported on iOS — use navigation within the sheet.

---

### 7.2 Alert

A modal dialog for critical information requiring user decision.

```swift
.alert("Delete Note?", isPresented: $showAlert) {
    Button("Delete", role: .destructive) { deleteNote() }
    Button("Cancel", role: .cancel) { }
} message: {
    Text("This action cannot be undone.")
}
```

**Rules**
- Title: a single question or brief statement. No ending punctuation unless a question mark.
- Message: clarifying detail. Optional. Keep to 1–2 sentences.
- Buttons: maximum **3**. Destructive on the left (iOS), Cancel on the right / bottom.
- Avoid alerts for non-critical information — use banners or inline messaging instead.
- Never use alerts to display information that doesn't require an immediate decision.

---

### 7.3 Action Sheet / Confirmation Dialog

A bottom-anchored sheet of actions (iOS) or an alert with many buttons (macOS).

```swift
.confirmationDialog("What would you like to do?", isPresented: $showDialog, titleVisibility: .visible) {
    Button("Save to Photos") { }
    Button("Share…") { }
    Button("Delete", role: .destructive) { }
    Button("Cancel", role: .cancel) { }
}
```

**Rules**
- Title is optional but recommended for clarity.
- Cancel button is always present and always last.
- Maximum 6 non-Cancel options; beyond this, use a sheet with a full list.
- Destructive action is highlighted in red; positioned second-to-last above Cancel.

---

### 7.4 Popover

A floating panel anchored to a control. Non-modal on iPad/Mac; modal on iPhone.

```swift
Button("Filter") {
    showPopover = true
}
.popover(isPresented: $showPopover, arrowEdge: .top) {
    FilterView()
        .frame(minWidth: 280, maxWidth: 400)
}
```

**Rules**
- On iPhone, popovers present as sheets — design the content to work in both configurations.
- Include an explicit close button if the popover contains a form.
- Do not present navigation stacks inside popovers.

---

### 7.5 Toast / Banner (In-App Notification)

Transient, non-modal notification.

**iOS System Behavior**
- Use `UserNotifications` for system banners.
- For in-app banners, use a custom floating view that appears below the navigation bar.

**Rules**
- Auto-dismiss after 3–5 seconds.
- Allow manual dismissal via swipe or tap.
- Do not stack multiple banners — queue them or show only the most recent.
- Place at the top of the screen, below the status bar.
- Use concise language: action + result ("Photo saved to library").

---

## 8. Feedback & Status

### 8.1 Progress Indicators

| Component | Usage |
|---|---|
| `ProgressView()` | Indeterminate spinner (system activity indicator) |
| `ProgressView(value: progress)` | Determinate linear bar |
| `Gauge` | Circular or linear gauge (watchOS primary, iOS/macOS supported) |

**Rules**
- Show a spinner only when the wait exceeds 500 ms.
- For operations > 2 s, show a determinate progress bar if the completion percentage is knowable.
- Never show a spinner inside a button — disable the button and show the spinner adjacent.
- On task completion, replace the spinner with a success icon briefly, then transition to result.

---

### 8.2 Empty States

When a view has no content, an empty state explains why and offers a path forward.

**Anatomy**
```
[ Icon (SF Symbol, large scale) ]
[ Title (headline weight) ]
[ Description (body, secondaryLabel color) ]
[ CTA Button (borderedProminent) — optional ]
```

**Rules**
- Icon should be conceptually related to the empty section (not a generic empty-box icon).
- Title uses plain language: "No Messages Yet" not "Empty Inbox."
- Description provides context and next step.
- CTA is optional; only include if there is a clear action the user can take immediately.

---

### 8.3 Error States

Inline error, not alert, for form validation and data-load failures.

**Form Validation**
- Show error message directly beneath the field that failed.
- Use `.foregroundStyle(.red)` for error text.
- Re-run validation on the field when the user changes its value (not just on submit).

**Network / Data Errors**
- Show a content-area error with retry button.
- Include the error category ("No Internet Connection", "Server Error") and an action.
- Do not show raw error codes to users.

---

### 8.4 Badge

A small numeric or dot indicator on a tab bar icon, app icon, or list row.

**Rules**
- Numeric: show count of actionable items (unread messages, pending tasks).
- Display max value: **99+** — never wrap to `100`, `1k`, etc.
- Dot badge: used when there is a new item but a count isn't meaningful.
- Remove badge immediately when the user acknowledges the content.

---

## 9. Content Components

### 9.1 Image View

```swift
AsyncImage(url: url) { image in
    image.resizable().scaledToFill()
} placeholder: {
    ProgressView()
}
.frame(width: 80, height: 80)
.clipShape(RoundedRectangle(cornerRadius: 12))
```

**Rules**
- Always use `scaledToFill` with `.clipped()` or a clipping shape to prevent layout expansion.
- Provide a placeholder (skeleton or spinner) while loading.
- On failure, show a generic image placeholder icon, not a broken image indicator.
- Supply an accessibility label describing the image content.

---

### 9.2 Video Player

Use `VideoPlayer` from AVKit for standard in-line playback.

**Rules**
- Video must not autoplay with audio (respect system silent mode).
- Provide captions/subtitles support via `AVMediaSelectionOption`.
- Full-screen toggle should be present for all in-line video.
- Pause when app enters background unless the user is actively streaming audio.

---

### 9.3 Map View

```swift
Map {
    Annotation("HQ", coordinate: hqCoordinate) {
        Image(systemName: "building.2")
            .foregroundStyle(.blue)
    }
    UserAnnotation()
}
.mapStyle(.standard(elevation: .realistic))
.mapControls {
    MapCompass()
    MapUserLocationButton()
}
```

**Rules**
- Request location permission only when the user triggers a location-dependent feature.
- Always show the user location button when location is relevant.
- Use `MapCameraPosition` to programmatically control the viewport.
- Clustering: group dense annotations at low zoom; expand on zoom in.

---

### 9.4 Charts (Swift Charts)

```swift
Chart(data) { point in
    LineMark(x: .value("Date", point.date), y: .value("Value", point.value))
        .foregroundStyle(.blue)
    AreaMark(x: .value("Date", point.date), y: .value("Value", point.value))
        .foregroundStyle(.blue.opacity(0.15))
}
.chartXAxis { AxisMarks(preset: .aligned) }
.chartYAxis { AxisMarks(position: .leading) }
```

**Rules**
- Always label axes with units in the axis title or first/last tick.
- Use semantic colors from the system palette; do not use custom colors that lose meaning in dark mode.
- Support accessibility descriptions via `chartAccessibilityLabel`.
- Provide an audio graph alternative (`.accessibilityChartDescriptor`) for VoiceOver users.
- Animate chart data updates with spring transitions.

---

## 10. Layout Containers

### 10.1 Scroll View

```swift
ScrollView(.vertical, showsIndicators: true) {
    LazyVStack(spacing: 0) {
        ForEach(items) { item in
            ItemRow(item: item)
        }
    }
}
.scrollDismissesKeyboard(.interactively)
.scrollIndicatorsFlash(trigger: items.count)
```

**Rules**
- Use `LazyVStack` / `LazyHStack` inside scroll views for long content — never `VStack` alone.
- Paging scroll views (`scrollTargetBehavior(.paging)`) for card carousels and onboarding.
- Scroll position anchoring: use `scrollPosition(id:anchor:)` to restore position after navigation.

---

### 10.2 Card

Cards are not a native UIKit/SwiftUI component but a widely used layout pattern.

**Standard Card Anatomy**
```swift
VStack(alignment: .leading, spacing: 12) {
    // Header: image or icon
    // Body: title, subtitle, metadata
    // Footer: action buttons (optional)
}
.padding(16)
.background(.secondarySystemGroupedBackground)
.clipShape(RoundedRectangle(cornerRadius: 16))
.shadow(color: .black.opacity(0.06), radius: 8, y: 4)
```

**Rules**
- Corner radius: 12–16 pt for card containers; 8–10 pt for items within cards.
- Elevation: use subtle shadow (radius 8–16, opacity 4–8%) in light mode only. Dark mode: rely on background color contrast, not shadow.
- Cards are tappable — make the entire card the touch target.
- Hover effect (iPadOS/macOS): `.hoverEffect(.highlight)`.

---

### 10.3 Split View & Navigation Split View

See [Navigation Split View (1.4)](#14-navigation-split-view).

---

### 10.4 Stack Layouts

| Container | Use Case |
|---|---|
| `HStack` | Horizontal, fixed count of views |
| `VStack` | Vertical, fixed count of views |
| `ZStack` | Overlapping/layered content |
| `LazyHStack` | Horizontal, many views (inside ScrollView) |
| `LazyVStack` | Vertical, many views (inside ScrollView) |
| `Grid` | Fixed 2D tabular layout |
| `LazyVGrid` | Adaptive columns for collections |
| `ViewThatFits` | Chooses layout that fits available space |

**Rules**
- Prefer `ViewThatFits` over conditional layout checks for Dynamic Type overflow.
- `ZStack` with `alignment` controls where overlapping content anchors.
- Avoid deeply nested stacks — flatten with `Group` or extract subviews.

---

## 11. Component Platform Matrix

| Component | iOS | iPadOS | macOS | visionOS | watchOS |
|---|---|---|---|---|---|
| Navigation Bar | ✅ | ✅ | ✅ (Toolbar) | ✅ | ✅ (nav bar) |
| Tab Bar | ✅ | ✅ (→ Sidebar) | ❌ (→ Sidebar) | ✅ (Ornament) | ❌ |
| Sidebar | ❌ (sheet) | ✅ | ✅ | ✅ | ❌ |
| Toolbar | ❌ (nav bar) | ✅ | ✅ | ✅ | ❌ |
| Context Menu | ✅ | ✅ | ✅ | ✅ | ❌ |
| Popover | ✅ (→ sheet) | ✅ | ✅ | ✅ | ❌ |
| Sheet | ✅ | ✅ | ✅ | ✅ | ✅ |
| Alert | ✅ | ✅ | ✅ | ✅ | ✅ |
| Table (multi-column) | ❌ | ✅ | ✅ | ❌ | ❌ |
| Date Picker (graphical) | ✅ | ✅ | ✅ | ✅ | ❌ |
| Color Picker | ✅ | ✅ | ✅ | ✅ | ❌ |
| Slider | ✅ | ✅ | ✅ | ✅ | ✅ |
| Toggle (switch) | ✅ | ✅ | ✅ | ✅ | ✅ |
| Toggle (checkbox) | ❌ | ❌ | ✅ | ❌ | ❌ |
| SF Charts | ✅ | ✅ | ✅ | ✅ | ✅ |
| Map View | ✅ | ✅ | ✅ | ✅ | ✅ |
| Video Player | ✅ | ✅ | ✅ | ✅ | ❌ |

---

*Last updated: June 2025 · Source: Apple Human Interface Guidelines, WWDC 2024–2025*
