# DBeaver Modern UI Redesign — Design Plan (v2)

> **Inspiration:** Zed Editor · VS Code · Slack · Linear
> **Design philosophy:** High-density but airy. Meaningful colour only. Every pixel earns its place.

---

## What changed in v2

The v1 mockups were still "old looking" because they used:
- Emoji characters as UI icons (`🗄 📁 ▦ 👁`)
- Box-drawing expand chevrons (`▾ ▸`)
- Traditional top-only toolbar without an activity bar
- Gradients and outer tab borders

v2 addresses all of this with a fully modern design system.

---

## Design Principles

| Principle | Implementation |
|-----------|---------------|
| **Activity bar** | 48 px icon-only left rail (VS Code convention); separates navigation from editing |
| **SVG-only icons** | All icons are inline `currentColor` SVGs — no emoji, no PNG sprites |
| **Flat surfaces** | No shadows on chrome elements; one subtle `box-shadow` on modal dialogs only |
| **Accent underline tabs** | Tabs sit flat; active tab = 1 px accent top border (VS Code style) |
| **Breadcrumb navigation** | Path shown below tabs: Connection › Schema › Table › file.sql |
| **Semantic colours** | Accent for actions; success/warn/error for status; never decorative |
| **Dense but breathable** | 22 px tree rows, 26 px table rows, 35 px tabs; 4 px grid spacing |

---

## Colour Tokens

### Light Theme (VS Code Light+ inspired)

| Token | Hex | Usage |
|-------|-----|-------|
| `--editor-bg`    | `#ffffff` | Editor canvas, active tab |
| `--sidebar-bg`   | `#f3f3f3` | Tree view, sidebar |
| `--actbar-bg`    | `#2c2c2c` | Activity bar (intentionally dark) |
| `--tab-inactive` | `#ececec` | Inactive tabs |
| `--toolbar-bg`   | `#f8f8f8` | Editor toolbar |
| `--panel-bg`     | `#f3f3f3` | Result tab bar, table headers |
| `--statusbar-bg` | `#005fb8` | Status bar (solid accent) |
| `--border`       | `#e5e5e5` | Subtle dividers |
| `--border-mid`   | `#d0d0d0` | Input borders, active separators |
| `--text`         | `#1e1e1e` | Primary text |
| `--text2`        | `#616161` | Secondary / labels |
| `--text3`        | `#a0a0a0` | Placeholders, line numbers |
| `--accent`       | `#005fb8` | Links, active states, run button |
| `--accent-bg`    | `#e8f2fd` | Selection highlight |
| `--success`      | `#14854f` | Connected badge, commit |
| `--warn`         | `#b46200` | Warning states |
| `--error`        | `#bf1717` | Error states |

### Dark Theme (Zed / VS Code Dark+ inspired)

| Token | Hex | Usage |
|-------|-----|-------|
| `--editor-bg`    | `#111111` | Editor canvas (true dark, Zed-like) |
| `--sidebar-bg`   | `#1a1a1a` | Sidebar |
| `--actbar-bg`    | `#131313` | Activity bar |
| `--tab-inactive` | `#191919` | Inactive tabs |
| `--panel-bg`     | `#161616` | Toolbar, result bar |
| `--statusbar-bg` | `#0e639c` | Status bar |
| `--border`       | `#2a2a2a` | Dividers (very subtle) |
| `--border-mid`   | `#333333` | Input borders |
| `--text`         | `#d4d4d4` | Primary text (VS Code default) |
| `--text2`        | `#858585` | Secondary |
| `--text3`        | `#4a4a4a` | Gutters, disabled |
| `--accent`       | `#4d9ef5` | Accent |
| `--accent-bg`    | `#1a3b5e` | Selection |
| `--success`      | `#4caf74` | Connected |
| `--warn`         | `#e8a600` | Warning |
| `--error`        | `#f14c4c` | Error |

### Connection type strips

| Type | Light border | Dark border | Placement |
|------|-------------|-------------|-----------|
| Dev (default) | `#5897d5` | `#4a84c4` | 3 px left border on tree row |
| QA / Test     | `#4da86e` | `#4aaa6a` | 3 px left border on tree row |
| Production    | `#d0544a` | `#c45050` | 3 px left border on tree row |

---

## Typography

| Context | Font stack | Size | Weight |
|---------|-----------|------|--------|
| UI labels | `Inter, "Segoe UI", system-ui` | 13 px | 400 |
| Section headers / uppercase labels | Same | 11 px | 600 + uppercase + 0.08em spacing |
| Monospace (code, numbers, IDs) | `"JetBrains Mono", "Cascadia Code", Consolas` | 13 px | 400 |
| Status bar / breadcrumbs | Same as UI | 11 px | 400 |
| Tab labels | Same as UI | 12 px | 400 (active: inherit) |

---

## Layout

```
┌─────────────────────────────────────────────────────────────────────────┐
│  Title bar (30 px) — traffic lights · menu items · centred title        │
├────┬────────────────────────────────────────────────────────────────────┤
│    │  [Tab bar — 35 px, flush flat tabs, 1 px accent top on active]     │
│ A  │  [Editor toolbar — 30 px, run button + formatting actions]         │
│ c  │  [Breadcrumb — 20 px, Connection › Schema › Table › file]          │
│ t  ├────────────────────────────────────────────────────────────────────┤
│ i  │ ┌────┐ ┌─────────────────────────────────────────────────────────┐ │
│ v  │ │ ind│ │ gutter (52 px) │  code body                            │ │
│ i  │ │ (18│ │  12 px font    │  13 px JetBrains Mono, 1.65 lh        │ │
│ t  │ │ px)│ └───────────────────────────────────────────────────────┘ │ │
│ y  ├────────────────────────────────────────────────────────────────────┤
│ b  │  [Result tab bar — 30 px]                                         │
│ a  │  [Data table — sticky header, 22 px rows]                         │
│ r  │                                                                    │
├────┴────────────────────────────────────────────────────────────────────┤
│  Status bar (22 px, solid accent bg) — connection · schema · info       │
└─────────────────────────────────────────────────────────────────────────┘
```

**Activity bar icons (top → bottom):**
1. Database Navigator (cylinder icon)
2. Search (magnifier)
3. Project Explorer (folder)
4. ER Diagrams (ER diagram icon)
5. *(spacer)*
6. Preferences (gear) — pinned to bottom

**Active activity bar item:** white icon + 2 px accent strip on left edge.

---

## SQL Syntax Colours

### Light (VS Code Light+ defaults)

| Token | Colour | Notes |
|-------|--------|-------|
| Keywords (`SELECT`, `FROM`, …) | `#0000ff` | Bold |
| Strings | `#a31515` | |
| Comments | `#008000` italic | |
| Functions | `#795e26` | |
| Types | `#267f99` | |
| Numbers | `#098658` | |
| Column refs | `#0070c1` | |

### Dark (VS Code Dark+ / Zed-like)

| Token | Colour | Notes |
|-------|--------|-------|
| Keywords | `#569cd6` | Bold |
| Strings | `#ce9178` | |
| Comments | `#6a9955` italic | |
| Functions | `#dcdcaa` | |
| Types | `#4ec9b0` | |
| Numbers | `#b5cea8` | |
| Column refs | `#9cdcfe` | |

---

## Results Grid Cell States

| State | Background | Notes |
|-------|-----------|-------|
| Default | white / `#fafafa` (alt) | |
| Selected row | `#e8f2fd` (light) / `#1a3b5e` (dark) | |
| New row | `#f0fdf4` (light) | `+` in row number |
| Modified row | `#fffbeb` (light) | italic for changed cell |
| Deleted row | `#fff0f0` (light) | strike-through text |
| Cell error | `#fff0f0` (light) | error-coloured text |
| Search match | `#fef9c3` (light) | |
| NULL | `var(--text3)` italic | never blank |
| Boolean | Pill badge: green `true` / grey `false` | not raw text |
| Numeric | Right-aligned, monospace, green tint | |
| Date/time | Teal/blue tint | |
| BLOB/binary | Purple tint, size label | |

---

## Icon System

All icons: 16×16 px inline SVG, `fill="none"`, `stroke="currentColor"`, `stroke-width="1.3–1.5"`.

| UI element | Icon description |
|------------|-----------------|
| Activity bar — navigator | Cylinder (3-layer database) |
| Activity bar — search | Circle + diagonal line |
| Activity bar — projects | Folder outline |
| Activity bar — ER | Two rectangles + connecting line |
| Activity bar — settings | Gear (circle + spokes) |
| Tree — expand arrow | Right-pointing chevron; rotates 90° when open |
| Tree — connection | Cylinder, coloured by type |
| Tree — schema | Folder |
| Tree — tables group | Grid with row/column lines |
| Tree — table | Grid with row/column lines (same, leaf node) |
| Tree — view | Eye outline |
| Tree — procedure | Curly-brace outline |
| Tab — SQL file | Document with text lines |
| Tab — table viewer | Grid icon |
| Toolbar — Run | Filled play triangle |
| Toolbar — Stop | Filled square |
| Toolbar — Commit | Checkmark |
| Toolbar — Rollback | Circular arrow |
| Toolbar — Explain | Clock/timer |
| Status badge | Filled circle (green/grey/amber) |

---

## Files to Change

### CSS (Eclipse e4 theme stylesheets)

| File | Change |
|------|--------|
| `plugins/org.jkiss.dbeaver.core/css/e4-dbeaver_prefstyle.css` | Light surface colours; tab border-radius 0 (flat tabs) |
| `plugins/org.jkiss.dbeaver.core/css/e4-dark_dbeaver_prefstyle.css` | Dark token overrides |
| `plugins/org.jkiss.dbeaver.ui/css/e4-high_contrast_dbeaver_prefstyle.css` | High-contrast overrides |
| `plugins/org.jkiss.dbeaver.ui.editors.data/css/e4-data-editor.css` | Grid light colours |
| `plugins/org.jkiss.dbeaver.ui.editors.data/css/e4-dark-data-editor.css` | Grid dark colours |
| `plugins/org.jkiss.dbeaver.ui.editors.sql/css/e4-dark-sql-editor.css` | SQL dark syntax tokens |
| `plugins/org.jkiss.dbeaver.ui.editors.erd/css/e4-dark-erd-editor.css` | ERD dark colours |

### plugin.xml — colour/font definitions

| File | Change |
|------|--------|
| `plugins/org.jkiss.dbeaver.core/plugin.xml` | Connection-type colours; txn status colours |
| `plugins/org.jkiss.dbeaver.ui/plugin.xml` | Accent `#005fb8`; fonts to `Inter`/`JetBrains Mono` |

### Java (minimal, surgical)

| File | Change |
|------|--------|
| `plugins/org.jkiss.dbeaver.core/src/.../DBeaverCTabFolderRenderer.java` | Remove gradient fill; draw 1 px accent line on top of active tab instead |
| `plugins/org.jkiss.dbeaver.ui.app.standalone/src/.../ApplicationWorkbenchWindowAdvisor.java` | Add activity bar composite (or use existing perspective bar layout) |

### Icons (SVG replacements)

All icons listed in the **Icon System** table above, located in:
- `plugins/org.jkiss.dbeaver.ui/icons/` (main UI icons)
- `plugins/org.jkiss.dbeaver.model/icons/` (database object icons)
- `plugins/org.jkiss.dbeaver.ui.app.standalone/icons/` (app icons)

### Splash screen

| File | Change |
|------|--------|
| `plugins/org.jkiss.dbeaver.ui.app.standalone/splash.png` | Dark `#111111` background, centred white DBeaver wordmark, version bottom-right |

---

## Mockup Files

| File | Contents |
|------|----------|
| `docs/design/mockups/01_main_window_light.html` | Full app window, VS Code Light+ theme |
| `docs/design/mockups/02_main_window_dark.html` | Full app window, Zed/VS Code Dark+ theme |
| `docs/design/mockups/03_sql_editor_dark.html` | SQL editor with autocomplete popup, inline error tooltip, context sidebar |
| `docs/design/mockups/04_connection_wizard.html` | New connection dialog with step indicator and driver list |
| `docs/design/mockups/05_results_grid.html` | Results grid showing all cell/row states |

---

## Implementation Order

1. **CSS colour tokens** — update e4 CSS files (no Java build needed, hot-reloadable)
2. **SQL editor syntax** — update dark SQL editor CSS
3. **Results grid** — update data editor CSS
4. **Fonts** — update `plugin.xml` font definitions
5. **Icons** — replace SVG files group by group (tree icons → toolbar → status)
6. **Tab renderer** — minimal Java change (remove gradient, add accent line)
7. **Activity bar** — layout change (perspective switcher → icon-only activity bar)
8. **Splash screen** — PNG asset swap

---

*Colour values and measurements are targets; verify on-screen at 100% and 150% HiDPI scaling.*
