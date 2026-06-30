# Changelog

All notable changes to the **Reflect** theme are documented here.
Format follows [Keep a Changelog](https://keepachangelog.com/). The full
commit-level history lives in git; this file is the human-readable summary.

## 2026-06-30 — Card contrast: task id + green progress pill

### Fixed
- **Task id on cards** (e.g. `t_bb27da70`) was `0.65rem` muted-grey and
  hard to read — brightened to a mid-tone (55% foreground / 45% muted),
  enlarged to `0.72rem`, weight 500.
- **Green "n/n" complete pill** rendered grey text on a solid bright-green
  background (unreadable). Now dark-green text (`#052e16`) on the green,
  bold and slightly larger — high contrast.
- Comment markdown no longer inherits the description box's border.

## 2026-06-30 — Fix: define `--color-foreground` (made the drawer boxes render)

### Fixed
- **The drawer boxes, borders, and brightened section headers were not
  rendering** because every one of those rules uses
  `color-mix(… var(--color-foreground) …)`, and `--color-foreground` was
  never actually defined: the dashboard's theme system has no `foreground`
  override key, and this theme sets `palette.foreground.alpha: 0`. A
  `color-mix` with an undefined variable is invalid CSS, so the browser
  silently dropped those declarations (boxes came out with `border: 0 none`
  and transparent backgrounds).
- Define `--color-foreground: #E6EAF2` and `--color-background: #0A0E1A`
  directly in the customCSS `:root`. Nothing else sets them, so these win;
  if a dashboard *does* define them (inline), that takes precedence — the
  override is purely a gap-filler and cannot break other setups. The
  backdrop layer (`palette.foreground`) is untouched.

## 2026-06-30 — Drawer boxes, opaque panel, brighter section headers

### Fixed
- **Detail boxes now use a real 1px border** (metadata, description, worker
  log) instead of an inset box-shadow ring that could silently fail to
  render, leaving no visible edge.
- **Drawer panel + header forced solid opaque** (blur removed from the
  header) so the issue details sit sharply in front of the blurred board
  rather than looking hazy.

### Changed
- **Section headers brightened** to a legible mid-tone (70% foreground /
  30% muted) — readable but still secondary to the white title/content, so
  the multica hierarchy stays intact.

## 2026-06-30 — Fonts matched to multica + Kanban board wrapping

### Changed
- **Fonts now match multica exactly**: body/UI font is **Inter** (reverting
  the brief switch to Geist), monospace is **Geist Mono** with multica's
  fallback chain (`ui-monospace, SFMono-Regular, Menlo, Consolas, monospace`).
  multica's third font (Source Serif 4) is onboarding/landing-only and is
  not used anywhere in the dashboard UI, so it is intentionally not loaded.
- **Base font size 15px → 16px** to match multica's root size, so equivalent
  text renders at the same size.
- **Description box** now gets the same lifted lighter card surface (rounded,
  ringed, slightly lighter fill) as the metadata box; comment markdown is
  left plain so it isn't double-boxed.
- **Kanban board columns wrap** into a responsive grid
  (`repeat(auto-fill, minmax(280px, 1fr))`) instead of scrolling
  horizontally — Done/Archived columns wrap into view.

## 2026-06-30 — Kanban task drawer redesign (multica-inspired)

A pass to bring the Kanban task detail drawer in line with a clean,
Linear/multica-style inspector.

### Added
- **Lifted "card" surfaces** for the metadata box and the worker-log box —
  a clearly lighter, opaque fill with a 1px ring and a subtle top highlight.
- **Section dividers** — a 2px hairline above each drawer section (Events,
  Worker log, etc.) for clean separation.
- **Full-row hover** on metadata rows and event rows.
- **Title Case action buttons** — Triage, Ready, Block, Complete, Archive,
  Discord, Edit, + Parent, + Child (task IDs, filenames, and dropdowns are
  left untouched).

### Changed
- **Wider drawer** — now up to `min(1320px, 94vw)`, with content held in a
  centered reading column so the panel reads clean rather than cavernous.
- **Body font switched from Inter to Geist** (the companion to the Geist
  Mono already used), for a less generic, more engineered feel. Inter is
  retained as the fallback.
- **Metadata layout** — from stacked "stat cards" to quiet label-left /
  value-right rows; labels are normal case (no uppercase / caps tracking).
- **Drawer scrim** — lighter, blurred overlay instead of a heavy black wash.
- **Surfaces use rings, not hard borders** (the log pane and comment input).
- **Worker log and events use the page sans font**; the worker-log font
  rule was also un-scoped so it actually applies in the portaled drawer.
- **Comment input, dependency chips, and tags** restyled (Inter, pill chips,
  soft focus ring).

### Fixed
- Metadata/log background appeared unchanged because the override only
  nudged the plugin's existing 4% fill by 2%; now an opaque card+12% mix.
- Dead selector `.hermes-kanban-drawer-meta-value` (never matched anything)
  corrected to the real `.hermes-kanban-meta-value`.

## Earlier

Initial Reflect theme: deep-navy palette with indigo-violet accents, aurora
bloom (`body::before`), film-grain overlay (`body::after`), glass cards,
hover elevation, `prefers-reduced-motion` support, and comprehensive Kanban
plugin styling. See git history for commit-level detail.
