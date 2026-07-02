# Screenshot checklist for Reflect theme

Capture these pages to showcase the theme. All screenshots should be at 1440×900 or wider.

> **Stale as of 2026-07-01:** every live capture below predates the
> dashboard-wide pass (badge pills, pill switches, outlined buttons, chart
> re-brand, log washes) and the Kanban Multica redesign. They still show the
> theme's palette correctly but not the current component styling — retake
> when convenient. Mockup renders for the new work live in
> `mockups/screenshots/` (`dash-current.png` / `dash-proposed.png` for the
> dashboard pass, `final-*.png` for Kanban).

## Must-have screenshots

- [x] **Sessions page** — `screenshots/sessions-overview.png`. Sidebar + session list + session detail.
- [x] **Style Guide page** (`/style-guide`) — `screenshots/style-guide-full.png`. Every UI component rendered.
- [ ] **Analytics page** — charts with accent colors, stat cards with gradient backgrounds.
- [x] **Cron page** — `screenshots/cron.png`. Tabbed interface, cards, action buttons.
- [x] **Kanban page** — covered by mockup renders (see below); a live capture of the
      2026-07-01 Multica redesign would still be a nice upgrade.

## Nice-to-have screenshots

- [ ] **Sessions page - mobile** — responsive layout, hamburger menu.
- [ ] **Config page** — form inputs, dropdowns, toggles.
- [x] **Skills page** — `screenshots/skills.png`. Filter pills, category buttons.
- [x] **Logs page** — `screenshots/logs.png`. Monospace code rendering.
- [x] **Models page** — `screenshots/models.png`.
- [x] **Plugins page** — `screenshots/plugins.png`.

## Kanban renders (mockups/screenshots/)

The Kanban board + task drawer images embedded in the README are rendered from
`mockups/kanban-reflect-v2.html` (faithful plugin markup + the shipped theme
CSS) via headless Chromium, not live captures:

- `final-board.png` / `final-drawer-tall.png` — the shipped Multica design
- `current-*.png`, `v2-*.png`, `v3-*.png`, `v4-*.png` — earlier variants kept
  for before/after comparison
- `screenshots/kanban.webp` at the repo root is a live capture of the
  **pre-redesign** board (2026-06); superseded by the renders above.

To re-render after a CSS change:

```bash
chrome-headless-shell --disable-gpu --window-size=1600,2100 --hide-scrollbars \
  --virtual-time-budget=6000 --screenshot=out.png \
  "file:///.../mockups/kanban-reflect-v2.html?variant=final"
```

(`?variant=final` renders the exact shipped CSS; add `&drawer=closed` for the board.)

## How to take live captures

1. Open the dashboard at `https://hermes.makeflow.dev:9119`
2. Select "Reflect" theme from the theme switcher (bottom-left gear icon)
3. Take full-viewport screenshots (not cropped regions) for consistency
4. Name files: `sessions.png`, `style-guide.png`, `analytics.png`, `cron.png`, `kanban.png`
5. Place in this repo's `screenshots/` directory
6. Update README.md to reference them

## Theme highlight features to capture

- **Aurora bloom**: The subtle color gradient at the top of every page
- **Glass cards**: Semi-transparent card backgrounds with backdrop blur
- **Card hover lift**: Cards elevate and glow on hover (non-kanban surfaces)
- **Indigo-violet accents**: Primary actions and active states
- **Film grain**: Subtle texture on dark backgrounds (reduces banding)
- **Geist Mono code blocks**: Monospace rendering in sessions/logs
- **Kanban drawer**: two-pane Multica layout, status keyline + chip, running pulse
