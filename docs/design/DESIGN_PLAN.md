# DBeaver Modern UI Redesign — Design Plan

## Overview

This document describes a proposed modern UI refresh for DBeaver. The goal is a clean,
minimalist aesthetic that is consistent across platforms, comfortable for extended use, and
performant (no GPU-heavy effects such as transparency, blur, or "glass"). All changes are
confined to styling, theming, and icon assets — no architectural or feature changes.

---

## Design Principles

| Principle        | Description |
|------------------|-------------|
| **Flat & clean** | No shadows, no gradients, no blur. Subtle 1 px borders only where structure is needed. |
| **Legible**      | High-contrast text on every surface. Minimum 4.5:1 contrast ratio. |
| **Consistent spacing** | 4 px grid — padding multiples of 4. Default row height 28 px, compact 24 px. |
| **Purposeful colour** | One brand accent (`#2A7FE8`). Semantic colours for status only (green/amber/red). |
| **System fonts** | `Segoe UI 9` on Windows, `SF Pro Text 13` on macOS, `Inter 10` on Linux. Monospace: `JetBrains Mono`. |
| **Accessible**   | Every interactive element has a keyboard accessible focus ring. |

---

## Colour Palette

### Light Theme

| Role | Hex | RGB |
|------|-----|-----|
| Canvas / page background | `#F5F6F7` | 245, 246, 247 |
| Panel / sidebar background | `#ECEDF0` | 236, 237, 240 |
| Surface (cards, tabs) | `#FFFFFF` | 255, 255, 255 |
| Border | `#D1D3D8` | 209, 211, 216 |
| Primary text | `#1A1C20` | 26, 28, 32 |
| Secondary text | `#5A5E6B` | 90, 94, 107 |
| Placeholder / disabled | `#9EA4B0` | 158, 164, 176 |
| Accent (links, active tabs, focus) | `#2A7FE8` | 42, 127, 232 |
| Accent hover | `#1A6FD4` | 26, 111, 212 |
| Selection highlight | `#D6E8FB` | 214, 232, 251 |
| Success | `#2D9E5F` | 45, 158, 95 |
| Warning | `#D97706` | 217, 119, 6 |
| Error / Danger | `#D93B3B` | 217, 59, 59 |

### Dark Theme

| Role | Hex | RGB |
|------|-----|-----|
| Canvas / page background | `#1C1E22` | 28, 30, 34 |
| Panel / sidebar background | `#15171A` | 21, 23, 26 |
| Surface (cards, tabs) | `#242629` | 36, 38, 41 |
| Border | `#333640` | 51, 54, 64 |
| Primary text | `#E2E4EA` | 226, 228, 234 |
| Secondary text | `#8C91A0` | 140, 145, 160 |
| Placeholder / disabled | `#525768` | 82, 87, 104 |
| Accent | `#4A9EF5` | 74, 158, 245 |
| Accent hover | `#6BB2F7` | 107, 178, 247 |
| Selection highlight | `#1E3A5C` | 30, 58, 92 |
| Success | `#3DBE72` | 61, 190, 114 |
| Warning | `#F59E0B` | 245, 158, 11 |
| Error / Danger | `#F05050` | 240, 80, 80 |

### Connection-type badges (both themes)

| Type | Light background | Dark background |
|------|-----------------|-----------------|
| Development (default) | `#EFF6FF` / border `#93C5FD` | `#1E2A3A` / border `#3B6EA8` |
| QA / Test | `#F0FDF4` / border `#86EFAC` | `#162A1E` / border `#2F6B45` |
| Production | `#FFF1F2` / border `#FDA4AF` | `#2A1618` / border `#7C2D2D` |

---

## Typography

| Style | Font | Weight | Size |
|-------|------|--------|------|
| UI (general) | Segoe UI / SF Pro Text / Inter | Regular (400) | 9 pt Win / 13 pt Mac / 10 pt Linux |
| UI Bold | Same | SemiBold (600) | Same |
| Code / SQL | JetBrains Mono | Regular (400) | 10 pt |
| Toolbar labels | Same as UI | Regular | 8 pt |
| Section headers (properties) | Same as UI | SemiBold | 8 pt UPPERCASE |

---

## Layout Changes

### Main Window

```
┌─────────────────────────────────────────────────────────────────────┐
│  [Logo 24px] DBeaver  ·  File  Edit  Navigate  SQL  Window  Help   │  ← Menu bar (28 px)
├────────────┬───────────────────────────────────────────────────────┤
│ [Toolbar – flat icon buttons, 20 px icons, 4 px gap]               │  ← Toolbar (32 px)
├────────────┬────────────────────────────────────────────────────────┤
│            │  [Editor tab bar — 28 px height, rounded-top 3 px]    │
│  Database  │────────────────────────────────────────────────────────│
│  Navigator │                                                        │
│  (240 px)  │     Editor / Viewer area                              │
│            │                                                        │
│  ──────── │                                                        │
│  Projects  ├────────────────────────────────────────────────────────┤
│  (below,   │  [Results / Output tab bar]                           │
│  collaps.) │  Results pane (resizable)                             │
│            │                                                        │
├────────────┴────────────────────────────────────────────────────────┤
│  Status bar (20 px): connection • schema • row count • progress    │
└─────────────────────────────────────────────────────────────────────┘
```

Changes:
- Sidebar width default 240 px (currently wider)
- Toolbar height 32 px (from ~40 px)
- Toolbars: icon-only by default, tooltip on hover
- Tab bar: flat, no gradient, 1 px bottom border accent on active tab
- Status bar: condensed, monospace numbers

### Database Navigator

- Tree items: 24 px row height
- 16 px flat SVG icons (redesigned, see Icon System)
- Connection node: coloured left-border (4 px) indicating connection type
- Inline status dot (8 px circle): green=connected, grey=disconnected, amber=error
- Hover: `#D6E8FB` (light) / `#1E3A5C` (dark) background
- Selected: accent background, white text

### SQL Editor

- Background: white (light) / `#1C1E22` (dark)
- Line numbers: secondary text colour
- Gutter: 48 px wide
- Active line highlight: 1-2% darkening, no gradient
- Token colours follow the dark CSS already present, refined:
  - Keywords: `#3B88D8`
  - Strings: `#2D9E5F`
  - Comments: `#7C8699` italic
  - Numbers: `#D97706`
  - Functions: `#9B59B6`
  - Type names: `#C27C2C`

### Results Grid

- Header row: `#ECEDF0` (light) / `#15171A` (dark), 28 px
- Data rows: alternating `#FFFFFF` / `#F9FAFB` (light) or `#1C1E22` / `#242629` (dark)
- Cell selection: accent background
- NULL values: `#9EA4B0` italic
- Numeric: right-aligned, monospace
- Boolean: compact pill badge (green/grey)

---

## Icon System

All icons should be 16×16 px SVG, single-colour with `currentColor` fill/stroke, so they
automatically adapt to light/dark themes and text colour overrides.

Key icons to redesign (currently PNG or complex SVG):

| Icon | File(s) | New style notes |
|------|---------|-----------------|
| Database | `org.jkiss.dbeaver.ui/icons/database.svg` | Thin-stroke cylinder |
| Connect | `org.jkiss.dbeaver.ui/icons/database_connect.svg` | Cylinder + lightning bolt |
| Table | `model/icons/table.svg` | Simple 2×3 grid lines |
| Column | `model/icons/column.svg` | Single vertical line + label |
| Index | `model/icons/index.svg` | Thin funnel |
| View | `model/icons/view.svg` | Eye outline |
| Procedure | `model/icons/procedure.svg` | `{}` braces |
| Schema | `model/icons/schema.svg` | Stacked layers |
| SQL Execute | `ui/icons/misc/sql.svg` | Play triangle, clean |
| Filter | `ui/icons/misc/filter.svg` | Funnel, 2 lines |
| Expand/Collapse | `ui/icons/misc/expand.svg`, `collapse.svg` | Chevron right/down |

---

## Files to Change

### CSS / Theme files

| File | Change |
|------|--------|
| `plugins/org.jkiss.dbeaver.core/css/e4-dbeaver_prefstyle.css` | Light theme colours and layout metrics |
| `plugins/org.jkiss.dbeaver.core/css/e4-dark_dbeaver_prefstyle.css` | Dark theme colour overrides |
| `plugins/org.jkiss.dbeaver.ui/css/e4-high_contrast_dbeaver_prefstyle.css` | High-contrast overrides |
| `plugins/org.jkiss.dbeaver.ui.editors.data/css/e4-data-editor.css` | Results grid light colours |
| `plugins/org.jkiss.dbeaver.ui.editors.data/css/e4-dark-data-editor.css` | Results grid dark colours |
| `plugins/org.jkiss.dbeaver.ui.editors.sql/css/e4-dark-sql-editor.css` | SQL syntax dark colours |
| `plugins/org.jkiss.dbeaver.ui.editors.erd/css/e4-dark-erd-editor.css` | ERD diagram dark colours |

### Plugin.xml — color/font definitions

| File | Change |
|------|--------|
| `plugins/org.jkiss.dbeaver.core/plugin.xml` | Update `colorDefinition` values: accent, txn colours, connection-type colours |
| `plugins/org.jkiss.dbeaver.ui/plugin.xml` | Update font families/sizes (`Segoe UI`, `JetBrains Mono`); accent colour `#2A7FE8` |

### Icons (SVG)

All icons listed in the **Icon System** section above.

### Splash screen

| File | Change |
|------|--------|
| `plugins/org.jkiss.dbeaver.ui.app.standalone/splash.png` | New minimal splash: dark background `#1C1E22`, centred white logo, version string |

### Custom tab renderer (Java — optional, low-impact)

| File | Change |
|------|--------|
| `plugins/org.jkiss.dbeaver.core/src/.../DBeaverCTabFolderRenderer.java` | Adjust tab corner radius (3 px), remove gradient painting, draw 2 px accent underline on selected tab |

---

## Mockup Files

Interactive HTML mockups are located in `docs/design/mockups/`:

| File | Contents |
|------|----------|
| `01_main_window_light.html` | Full application window — light theme |
| `02_main_window_dark.html` | Full application window — dark theme |
| `03_sql_editor_dark.html` | SQL editor panel with syntax highlighting |
| `04_connection_wizard.html` | New connection dialog |
| `05_results_grid.html` | Data results grid with all cell states |

---

## Implementation Order (Suggested)

1. **Colour tokens** — update CSS preference files and plugin.xml color definitions
2. **Typography** — update font definitions (Segoe UI / JetBrains Mono)
3. **Results grid** — CSS is isolated; quick win
4. **SQL editor** — CSS only
5. **Icons** — replace SVGs one group at a time
6. **Tab renderer** — small Java change for tab underline / corner radius
7. **Splash screen** — PNG asset swap
8. **QA pass** — verify on Windows, macOS, Linux; light and dark; high-contrast

---

*This document accompanies the HTML mockup files. Colour values and layout dimensions should
be treated as targets; exact values may need slight adjustment after on-screen verification.*
