# Design Notes — OpenGP Proposal Showcase

## Approved Visual Direction: Option A (Slide-Match)

- **Palette:** Deep navy background (`#000b3b`) matching the presented proposal slides, elevated navy surfaces (`#07154f`, `#0d1f66`), white primary text (`#ffffff`), soft periwinkle secondary text (`#c9d3f5`), and a single bright blue accent (`#5ea0ff`).
- **Typography:** Self-hosted `InterVariable.woff2` (`assets/fonts/InterVariable.woff2`) with system sans-serif fallbacks. Fluid heading and body sizing via `clamp()`.
- **Layout:** Single scrolling page with a sticky top navigation bar, full-viewport hero (with subtle node-graph SVG motif within the navy/blue palette), and slide-like sections with generous vertical rhythm.
- **Accessibility:** All foreground/background pairs meet or exceed WCAG AA contrast. Visible `:focus-visible` outlines, semantic landmarks, single `<h1>`, and `prefers-reduced-motion` support.
- **Diagrams:** Architecture and experiment visuals are authored as inline SVGs themed by CSS custom properties rather than raster slide screenshots.
