# Changelog

All notable changes to the **Reflect** theme are documented here.
Format follows [Keep a Changelog](https://keepachangelog.com/). The full
commit-level history lives in git; this file is the human-readable summary.

## 2026-07-01 — Multica drawer: two-pane layout, status accents, document prose

A full redesign of the Kanban task drawer against multica's actual issue
detail view (`packages/views/issues/issue-detail.tsx`), plus board card
polish and several latent CSS bug fixes. Developed and verified in
`mockups/kanban-reflect-v2.html` (four toggleable variants + `?variant=final`
for the exact shipped CSS). The customCSS block is now assembled from two
readable fragments: `mockups/reflect-global.css` + `mockups/reflect-kanban-final.css`.

### Added
- **Two-pane drawer layout** (≥1100px): document column capped at 56rem +
  300px properties rail behind a full-height hairline, built with CSS grid
  on the plugin's existing DOM. Rail collapses into the flow on narrow views.
- **Status-aware accent system**: the drawer reads the task status from the
  title dot via `:has()` and drives a 2px top keyline, a status chip in the
  rail (amber running/ready, red blocked, indigo done), and a pulsing chip
  dot while the task is running (suppressed by `prefers-reduced-motion`).
- **Aurora echo** behind the drawer title, tinted by the status accent,
  scrolling away with the content.
- **Bordered record lists** for attachments and events (rounded container,
  hairline dividers, full-row hover), per multica's sub-issue lists.
- **Avatar discs on comments** (CSS-generated "@" badge) with the body
  indented beside them, per multica's comment cards.
- **Action row hierarchy**: first (primary) action stays solid; the rest
  become tonal indigo-wash buttons.
- Card polish: bulk-select checkbox revealed on hover/focus/selection;
  state chips docked to the card's right edge; warning badges rendered as
  amber/red severity chips instead of bare `⚠` / `!!` glyphs.

### Changed
- **Description is no longer a boxed panel** — it renders as open document
  prose in multica's compact tier (14px / 1.625 line-height, 72ch measure).
- **Properties** moved from a boxed metadata card to unboxed quiet rail rows
  (96px label column, hover wash), with a CSS-generated "Properties" label
  (English-only; remove the `::before` rule if i18n matters).
- **Worker log returned to Geist Mono** on a borderless muted block —
  multica's execution-log-section is `font-mono`, so this now matches the
  reference more closely than the previous sans-serif log.
- Section headers promoted to document scale (14px semibold foreground);
  separators reduced from 2px to hairline.
- Dependency chips: borderless muted pills with mono task ids.
- Drawer motion: 320ms expo slide + 200ms scrim fade; drawer head is a 48px
  toolbar with a square hover-wash close button.
- Selected cards: one primary border + wash instead of doubled inset rings.

### Fixed
- **Blanket kanban button rule swallowed quiet controls.** Its long `:not()`
  exclusion chain gave it specificity (0,13,1), so the drawer close ×,
  dependency-chip ×, section toggles, attachment links, and edit links all
  rendered as solid indigo buttons (attachment filenames rendered as giant
  full-width primary buttons). The chain is now wrapped in `:where()` (zero
  specificity) with explicit quiet styles for each excluded control.
- **Card hover lift snapped instead of animating**: a second duplicate
  `.hermes-kanban-card` block reset the transition list without `transform`.
  Blocks consolidated; the lift is removed outright (cards are drag handles)
  in favor of a border + shadow response.
- **Every card click flashed half-transparent** (`:active { opacity: 0.5 }`
  fired on plain clicks). The dim now applies only to actual drag sources.
- Home-channel subscription toggles were painted solid primary in both
  states by the blanket rule; they are now quiet chips with a distinct
  primary-wash "on" state.
- The generic `bg-card` hover-lift rule matched the kanban card's inner Card
  wrapper (double elevation); kanban internals are now excluded, and the
  inner wrapper is fully neutralized so cards carry exactly one border.
- Removed dead duplicate rules (two `.hermes-kanban-card` blocks, two
  `.hermes-kanban-section-head` blocks, two `--progress-full` blocks).

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
