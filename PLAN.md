# PLAN.md — Build the OpenGP Proposal Showcase

Read `AGENTS.md` first. All rules there apply. Execute phases in order; each phase = its own branch, atomic commits, verification gate, then `--no-ff` merge to `main`.

## Design direction

**Approved: Option A (slide-match)** — deep navy `#000B3B`-ish background, white text, one bright blue accent (~`#5EA0FF`), clean grotesque sans-serif (self-host **Inter** or **Barlow** woff2). Reference mock-up: `design/style-options.html` (local only). Record the choice in `docs/design-notes.md` and encode it only through tokens in `assets/css/tokens.css`. Optional subtle node-graph motif in the hero is allowed only if it stays within this palette.

Team section: four member cards with **names only (no index numbers)** plus placeholder LinkedIn/GitHub links (see AGENTS §1); supervisor shown as Dr. Adeesha Wijayasiri.

Structure: a single scrolling page with a sticky top nav, full-viewport hero, and sections that each feel like a "slide" (generous space, one idea each), plus a keyboard-friendly optional "slide mode" is **out of scope** unless time remains.

## Phases

### Phase 0 — Repo bootstrap (`chore/bootstrap`)
- `AGENTS.md`, `PLAN.md`, `.gitignore` are already committed; `source/` (PDF) and `design/` are git-ignored — verify with `git check-ignore`.
- On `chore/bootstrap`: extend `.gitignore` (`.DS_Store`, `node_modules/`, `reports/`, `*.log`), add `.nojekyll`, README skeleton (no index numbers).

### Phase 1 — Content extraction (`docs/content-outline`)
- Read **every** slide of the PDF (render pages to images, e.g. with Playwright/pdf.js or macOS Quick Look/`sips`; do not guess). The PDF has ~50 pages; text layer is font-encoded so use rendered images/OCR-by-vision.
- Produce `docs/content-outline.md`: for each slide → title, key points, figures/diagrams, numbers, references. Then a proposed section map for the site.
- Expected sections (adjust to the real deck): Hero/title+team · Problem & motivation · Background · Aim & objectives · Research gap / related work · Proposed solution & architecture · Methodology / work plan · Evaluation plan · Expected outcomes & timeline · Team & supervisor · References (no PDF download — the slides are private).
- Unknowns → `docs/open-questions.md`. Get user confirmation of the outline before Phase 3 if there are open questions.

### Phase 2 — Design system (`feat/design-system`)
- `tokens.css`, `base.css` (reset, typography, focus, reduced motion), `components.css` (nav, section, card, timeline, figure, footer, button).
- Self-hosted fonts. Light/dark not required; navy theme only (ensure AA contrast).
- Commit: a kitchen-sink `index.html` skeleton proving tokens work.

### Phase 3 — Build pages (`feat/sections-*`, one branch or one commit group per section)
- Implement sections from the outline in order, one commit per section.
- Re-draw architecture/flow diagrams as **inline SVG** (themed by CSS variables), not slide screenshots.
- Minimal JS: active nav link via IntersectionObserver, mobile menu, subtle reveal (disabled under reduced motion).
- SEO/meta, favicon, OG image (`assets/img/og.png`, < 100 KB).

### Phase 4 — Polish & performance (`perf/polish`)
- Optimise images, preload the main font, remove unused CSS, check CLS/LCP.

### Phase 5 — Verification gate & independent review (`test/review`)
Run everything in AGENTS §6 and write `reports/review-final.md` (ignored). Add two checks: `grep -rE "22[0-9]{4}[A-Z]"` over tracked files finds no index numbers, and `git ls-files` contains nothing from `source/` or `design/`.

| Check | Tool | Pass criteria |
|---|---|---|
| HTML validity | `npx html-validate` | 0 errors |
| Links/assets | `npx linkinator` on local server + sub-path | 0 broken |
| Lighthouse (mobile+desktop) | `npx lighthouse` | ≥ 95 all four |
| Accessibility | axe | 0 serious/critical |
| Responsive visuals | browser subagent @ 1440/1024/768/390 | no overflow/overlap; screenshots reviewed |
| No-JS / reduced motion | manual + browser | content readable |
| Content fidelity | slide→section table | 100% claims traced |
| Independent reviewer | separate read-only subagent | 0 blocker/major |
| Git hygiene | `git log --oneline`, `git status` | conventional commits, clean tree, no junk files |

### Phase 6 — Release (`main`)
- Update README (what it is, team, how to run locally, how deployed, licence/credits).
- Merge, tag `v1.0.0`. **Stop and ask the user** before pushing; then give Pages instructions (Settings → Pages → `main` / root) and verify the live URL once up.

## Definition of done
All phases merged, gate table all green with evidence in `docs/review-final.md`, clean tree, readable history, user has approved the design and content.
