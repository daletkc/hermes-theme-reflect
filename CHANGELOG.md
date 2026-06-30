# Changelog

All notable changes to the **Reflect** theme are documented here.
Format follows [Keep a Changelog](https://keepachangelog.com/). The full
commit-level history lives in git; this file is the human-readable summary.

## 2026-06-30 — Fonts matched to multica + Kanban board wrapping

### Changed
- **Fonts now match multica exactly**: body/UI font is **Inter** (reverting
  the brief switch to Geist), monospace is **Geist Mono** with multica's
  fallback chain (`ui-monospace, SFMono-Regular, Menlo, Consolas, monospace`).
  multica's third font (Source Serif 4) is onboarding/landing-only and is
  not used anywhere in the dashboard UI, so it is intentionally not loaded.
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
