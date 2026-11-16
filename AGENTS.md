# AGENTS.md — OpenGP Proposal Showcase Website

Instructions for any AI agent working in this repo. Read this file fully, then read `PLAN.md` and execute it phase by phase.

## 1. Project

Static showcase website for our final-year project proposal, hosted on **GitHub Pages**.

- **Project:** Peer-to-Peer Volunteer Computing System for Distributed LLM Inference
- **Team:** OpenGP — Pratheep S. (220489C), Sirithar R. (220614H), Suriya G. (220628E), Vithurshan S. (220672G)
- **Supervisor:** Dr. Adeesha Wijayasiri
- **Source of truth for content:** `source/OpenGP_-_Prposal_Presentation-Presented.pdf` (the presented proposal slides). Do not invent facts, numbers, citations or claims. If something is missing or unclear, add a `TODO(content):` note in `docs/open-questions.md` and ask the user.

## 2. Hard constraints

- **Plain HTML + CSS + vanilla JS only.** No frameworks, no build step, no bundlers, no npm runtime dependencies. (Dev-only tooling such as Playwright/Lighthouse is allowed via `npx`, and must not be required to serve the site.)
- Site root is the repo root (`index.html`) so GitHub Pages serves it directly. Use **relative paths only** (the site is served under `/<repo>/`, so never use `/assets/...`).
- No external runtime requests (CDNs, Google Fonts, analytics). Self-host fonts (woff2) and assets in `assets/`. Exception: none without user approval.
- Works offline, in the latest Chrome, Firefox, Safari, and on mobile (≥ 360px wide).
- Accessibility: WCAG AA contrast, semantic landmarks, one `h1`, logical heading order, alt text, visible focus, keyboard navigable, `prefers-reduced-motion` respected.
- Performance: no image > 300 KB (convert to WebP/SVG), total page weight < 1.5 MB excluding the optional PDF, Lighthouse ≥ 95 on Performance/Accessibility/Best Practices/SEO.
- Prefer inline SVG / CSS for diagrams over raster screenshots of slides.

## 3. Repo layout

```
index.html            # entry point
assets/css/           # tokens.css (design tokens), base.css, components.css
assets/js/            # main.js (small, modular, no globals)
assets/img/           # optimised images, SVG diagrams
assets/fonts/         # self-hosted woff2
source/               # original PDF (read-only reference)
docs/                 # open-questions.md, design-notes.md, review reports
tests/                # smoke/visual/a11y scripts (optional dev tooling)
PLAN.md  AGENTS.md  README.md  .gitignore  .nojekyll
```

## 4. Git workflow (mandatory)

The repo is initialised on `main` with **no commits**. Rules:

1. First commit: only `AGENTS.md`, `PLAN.md`, `.gitignore`, `README.md` skeleton, `.nojekyll` — message `chore: initialise repository with agent instructions`.
2. Never work directly on `main`. Create a branch per phase/feature: `feat/<topic>`, `fix/<topic>`, `docs/<topic>`, `chore/<topic>`. Merge into `main` with `git merge --no-ff` (or squash if user prefers) **only after the review gate in §6 passes**.
3. **Small, atomic commits** — one logical change each; the site must work at every commit. Never mix formatting, refactors and features.
4. **Conventional Commits**: `type(scope): imperative summary` (≤ 72 chars), with a body explaining *why* when non-trivial. Types: `feat fix docs style refactor perf test chore`.
5. Before every commit: `git status`, `git diff --staged` — review what you are committing. Stage explicitly (`git add <paths>`); never `git add -A` blindly.
6. Never commit: secrets, `.DS_Store`, `node_modules/`, screenshots/test output, large unoptimised files, or generated reports (put those in an ignored `reports/`).
7. Never rewrite published history: no `push --force`, no `reset --hard` on shared work, no `--no-verify`. If you made a mistake, add a fixing commit (or `git revert`).
8. Do not push or enable Pages until the user says so. When asked: push `main`, then Settings → Pages → Deploy from branch `main` / root (or tell the user to do it).
9. Tag the reviewed release: `v1.0.0` (annotated).
10. Keep the working tree clean at the end of each phase; report `git log --oneline` in your summary.

## 5. Code standards

- HTML5 semantic, valid (no validator errors). 2-space indent, lowercase, double quotes.
- CSS: design tokens as custom properties in `tokens.css` (colours, spacing, type scale, radius). Mobile-first, `clamp()` for fluid type, CSS Grid/Flex, BEM-ish class names. No `!important`, no inline styles.
- JS: ES modules or one IIFE, `const`/`let`, no globals, progressive enhancement (site fully readable with JS disabled). Only for: nav highlighting, scroll reveal, theme/slide-like interactions, mobile menu.
- Comments explain *why*, not *what*. No dead code, no commented-out blocks.
- Images: explicit `width`/`height`, `loading="lazy"` below the fold, descriptive `alt`.
- Add `<title>`, meta description, Open Graph tags, favicon (SVG), `lang="en"`.

## 6. Verification & review (mandatory before each merge)

You must **prove** the work, not claim it. Run the gate in `PLAN.md` §Verification and record results in `docs/review-<phase>.md` (commit it only if the user wants; otherwise keep in `reports/`).

Minimum gate:

1. **Serve locally**: `python3 -m http.server 8000` from repo root and test at `http://localhost:8000/` **and** under a sub-path simulation (e.g. serve parent dir, open `/proposal-showcase/`) to catch absolute-path bugs.
2. **Automated checks**: HTML validation (`npx html-validate index.html`), broken links/asset check (script or `npx linkinator http://localhost:8000 --recurse`), Lighthouse (`npx lighthouse ... `), axe accessibility (`@axe-core/cli` or Playwright + axe).
3. **Visual check** with the browser subagent at **1440, 1024, 768, 390 px**: screenshot every section, inspect for overflow, clipping, overlap, unreadable contrast, layout shift. Fix, then re-screenshot.
4. **Content fidelity check**: every claim/figure on the site traces to a slide in the PDF. List slide → section mapping in the review file.
5. **Independent review step**: spawn a *separate* reviewer subagent (`research` type, read-only) that did not write the code. Give it this file, `PLAN.md`, and the diff; ask for a list of defects by severity (blocker/major/minor) covering: requirements in §2, a11y, responsiveness, content accuracy, code quality, commit hygiene (`git log`). Fix all blockers and majors, then re-run the gate. Repeat until zero blockers/majors.
6. **No-JS and reduced-motion check**: disable JS, content must still be fully readable.

A phase is "done" only when the gate passes and the tree is committed.

## 7. Working agreements

- Ask the user before: adding a dependency, changing the visual direction, adding any external request, pushing, or deleting anything.
- Don't touch `source/` contents. Don't commit the PDF if the user marks it private (ask first; default: commit it).
- Keep summaries concise: what changed, how it was verified (with evidence), what's open.
- If blocked (tool missing, permission denied), report it and stop rather than working around it.
