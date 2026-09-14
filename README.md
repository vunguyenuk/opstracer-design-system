# OpsTracer Design System

- `design-tokens.css` is the shared foundation for Product and Marketing.
- `summation-app.css` and `index.html` contain the authenticated product system.
- `summation-refresh.css` and `landing.html` contain the Marketing System.
- `web-app.html` and `web-app.css` contain the interactive DS-02 rebuild of the audited OpsTracer application.
- `WEB-APP-REBUILD-AUDIT.md` records the complete route/function inventory, severity findings, and component remapping contract.
- `WEB-APP-UX-AUDIT.md` records the post-rebuild UI/UX, accessibility, responsive, icon, and heuristic audit.
- `billing-system.css` is the shared Summation-audited billing component family used by both the product system and rebuilt app.

Marketing may use a larger editorial type scale and more generous section rhythm, but it must reuse the shared colour, spacing, radius, control, elevation, typography, and semantic-state tokens. Add reusable measurements to the shared foundation instead of restating them in page-level selectors.
