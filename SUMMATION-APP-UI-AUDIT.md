# Summation product UI audit

Clean-room audit of the authenticated Summation application, measured on 12–13 September 2026 at 1512×823 and 2560×1318 CSS viewports. Only rendered behaviour and geometry were recorded. No source components, proprietary assets, application data, or hosted font files were copied.

## Scope inspected

| Route / surface | Patterns inspected |
| --- | --- |
| Home | chat composer, recent projects, resource cards, connect-data prompt |
| Get started | category tabs, function filter, skill gallery, search, secondary action cards |
| Projects | project list/cards, route action hierarchy |
| Reports and Decks | compact list headers, rows, create actions, empty/populated states |
| Dashboards | list, dashboard toolbar, narrative, metrics, charts, filter, view tabs |
| Tables | 78px data rows, metadata, tags, filter/search toolbar, pagination |
| Playbooks | tabs, search/filter, three-column cards, metadata and actions |
| Workflows | list, dotted canvas, node stack, connectors, inspector panel |
| Connectors | rounded data table, source-picker cards, 44px configuration fields, 32px schedule controls, select menu and 16px dialog |
| Settings | 1200px dialog, 200px grouped rail, General/Profile, Addison, Integrations, Users, Usage, Billing, developer empty state and loading skeleton |
| Overlays | account menu, action menu, filter menu, date-range menu and calendar |

Inspection was read-only. The Settings preference editor was opened and cancelled; no user or application data was changed.

## Architecture finding and application rule

The audit is a geometry reference, not the identity source. The public site, authenticated Summation app, and OpsTracer domain language must not be copied into one another wholesale. `index.html` therefore applies a deliberate mapping:

- **Retain from OpsTracer:** production logo SVGs, Host Grotesk + DM Mono, brand amber, warm black, semantic incident colours, evidence timelines, escalation and on-call concepts.
- **Adapt from the authenticated app:** shell composition, density, spacing, radius ladder, restrained borders/elevation, list/card hierarchy, dashboard, menus, fields and workflow geometry.
- **Retire only the old implementations:** decorative notch systems, oversized fields, marketing capsules in product UI, double borders, uncontrolled dark backgrounds, duplicate selectors and conflicting token layers. The approved compact evidence panel keeps one functional 18px terminus cut as an isolated exception.

Product styles live in `summation-app.css`; marketing remains in `landing.html` with `summation-refresh.css`. The later live-product audit in `OPSTRACER-DEV-UI-AUDIT.md` supplies missing workflows and information structure only; it does not supersede the Summation component styling.

### Reuse-first implementation rule

Every generated product component must first reuse the foundations in `design-tokens.css` and compose the component families already present in `summation-app.css` and `index.html`. A missing OpsTracer workflow does not authorize a second visual system: import its information structure and states, then render them through the existing Summation-derived geometry and OpsTracer identity layer. If no suitable primitive exists, add the measurement or variant once at the shared component layer and document the gap; do not introduce page-local field, button, card, navigation, icon, spacing, border, radius, or typography rules.

Before a component is accepted, compare its applicable interaction states and responsive behaviour with the shared family. Summation branding, assets, copy, and account data remain out of scope; only visible geometry and interaction behaviour are reference material.

## Measured reference foundations

The values below document the inspected Summation source. They are evidence for geometry and behaviour; typography and brand colour are intentionally remapped to OpsTracer in the resulting DS-02.

### Source typography

| Role | Rendered specification |
| --- | --- |
| Family | `Inter Variable`, Inter, metric-adjusted sans-serif fallback |
| Page title | 20px / 28–30px, weight 500–600 |
| Dashboard value | 28px / 42px, weight 500 |
| Body/navigation | 13px / 19.5px, weight 400–500 |
| Field content | 13px / 18–19.5px |
| Textarea | 14px / 20px |
| Metadata/tag/calendar | 11–12px / 16.5–18px |

Observed fallback metric overrides: ascent 90.49%, descent 22.56%, line gap 0%, size adjust 107.64%. Licensed or private font files are not linked.

### Source colour, radius, border, and elevation

| Token | Value | Purpose |
| --- | --- | --- |
| Surface | `#FFFFFF` | shell, controls, panels |
| Subtle surface | `#FAFAFA` | canvas/inspector wells |
| Component fill | `#F6F6F6` | secondary buttons, tags, hover |
| Active fill | `#EEEEEE` | selected navigation |
| Primary text | `#363636` | product content |
| Secondary text | `#6D6D6D` | metadata/helper copy |
| Control border | `#D1D1D1` | fields and bounded controls |
| Divider | `#EEEEEE` | row and section separation |
| Primary action | `#000000` | create/commit action |
| Accent | `#FF4800` | Addison/product accent only |

Radius ladder: 6px compact controls, 8px analytical panels/nodes, 12px cards/menus/shell, 16px large dialogs. Dividers and component borders render at 0.5px on the inspected 2× display.

- Standard card shadow: `0 1px 2px rgb(0 0 0 / 5%)`.
- Overlay shadow: `0 0 0 .5px rgb(0 0 0 / 16%), 0 3px 9px rgb(0 0 0 / 5%), 0 6px 18px rgb(0 0 0 / 2%)`.

### Applied OpsTracer mapping

| Layer | DS-02 decision |
| --- | --- |
| Logo | retain `assets/black-horizontal.svg` and `assets/logo-horizontal.svg` |
| Human-language type | retain self-hosted Host Grotesk 400/500 |
| Machine-fact type | retain self-hosted DM Mono 400/500 |
| Primary action | retain brand amber `#F0A439` with warm-black label |
| Secondary action | light surface with a neutral outline; large dark fills retired after the OpsTracer dev audit |
| Product neutrals | apply the measured ramp directly: `#FFFFFE`, `#FAFAFA`, `#F6F6F6`, `#EEEEEE`, `#D1D1D1`, `#363636`, `#6D6D6D` |
| Domain components | retain incident evidence, on-call, escalation, service health and operational state semantics |
| Geometry | adopt the measured 6/8/12/16px radius ladder and compact dimensions below |

## Exact component inventory

| Component | Measurement | Behaviour |
| --- | --- | --- |
| App sidebar | 252px wide | 30px rows, 8px row radius |
| Main shell | viewport less sidebar/gutters | white, radius 12px, 0.5px divider |
| Top/dashboard toolbar | 49px | 8px padding, bottom divider |
| Primary button | 36px high | 8×12px padding, radius 8px |
| Secondary/subtle button | 32px high | 4×10px padding, radius 6px |
| Icon button | 32×32px | centered 16px icon, no accidental inner padding |
| Primary connector input/select | 44px high | 0.5px border, 8px radius, 12px inline padding; ink focus border |
| Compact schedule input/select | 32px high | 0.5px border, 6px radius, 10px inline padding; disabled state lowers contrast |
| Textarea | observed 588×141px | 10×12px padding, 8px radius |
| Tag | 18px high | 0×6px padding, 6px radius, no wrap |
| Product tab switch | 32px group / 28px item | 2px group inset, quiet active surface and subtle shadow |
| Date-range icon control | 32×32px | centered 16px icon, 8px control gap |
| List header | 40px high | secondary 12–13px text |
| Table/list row | 78px high | 16×8px padding, divider only |
| Playbook card | ≈289.3×143px at 900px content width | 16px top/side, 12px bottom, radius 12px |
| Metric tile | responsive width × 120px | borderless, 16px padding, radius 8px, card shadow |
| Chart panel | responsive width × 428px | borderless, radius 8px, card shadow |
| Chart header | 36px high | 8×16px padding, 0.5px bottom divider |
| Dashboard view tab bar | 38px high | 14px inline padding, right divider |
| Workflow node | 300px wide | radius 8px, borderless, card shadow |
| Workflow node header | 52.5–53px | 6px padding, bottom divider |
| Workflow inspector | 326px panel inside 358px rail | 16px horizontal inset and padding, radius 12px; no trailing row divider |
| Inspector definition row | 32px minimum | 8px block padding, divider |
| Compact action/filter menu | 192px wide | 6px padding, 180×30px items |
| Calendar | 221×227.5–228px | 28×28px days, 8px radius |
| Settings dialog | 1200px × `min(772px, viewport − 32px)` | radius 16px, 200px grouped navigation rail |
| Settings navigation row | 167.5×32px | 8px padding/gap, 6px radius, quiet active fill |
| Settings profile rail | 668px content width | borderless 80px value rows with Copy, verified/locked and reset actions |
| Settings integration row | 668×72px | 32px mark, descriptive copy and trailing action |
| Settings member table | 807px wide | 40px header, 58px member row, 12px outer radius |
| Settings usage summary | 807×192px | 2:1 split, 6px progress meter, definition-list totals |
| Settings developer empty state | 807×180px | one restrained dashed boundary and a single primary action |
| Switch | 33×18px | 14px thumb |

## Historical dashboard reference

The Summation audit established a borderless analytical pattern with metric tiles and chart panels. That pattern is documented here as source research but was removed from the active `#dashboard` specimen after auditing OpsTracer dev. DS-02 now composes the dashboard from OpsTracer's actual reachability, source-health, incident, and on-call summaries; see `OPSTRACER-DEV-UI-AUDIT.md`.

## Defects resolved

### P1 — Competing legacy systems

Resolved by replacing the 3,251-line mixed cascade with a dedicated product stylesheet, then restoring the OpsTracer identity layer explicitly. Marketing is linked but no longer inherited; brand assets and domain components are retained by design rather than by accidental CSS leakage.

### P2 — Geometry, borders, and alignment

- Removed doubled/touching card borders from dashboard and workflow panels.
- The `/connectors` audit distinguishes 44px primary configuration fields from 32px compact scheduling controls. Primary actions are 36px, default actions 32px, compact actions 28px; navigation/menu rows remain 30px and tags 18px.
- Consolidated field, radius, padding, and button measurements into shared control tokens so organization settings, connector forms, selects, dialogs, and specimen controls cannot drift independently.
- Replaced the warmer inherited neutral ramp with the measured Summation control ramp; OpsTracer identity remains in its logo, Host Grotesk/DM Mono pairing, amber action, and operational state colours.
- Replaced the old five-row replay surface with the approved three-event evidence pattern: 40px header, 40px row rhythm, 42px footer, and one isolated 18px terminus cut. The track and dots use the same grid variables at every breakpoint, so their mathematical centre cannot drift.
- Gave icon buttons explicit boxes and centered 14–16px artwork.
- Centered tags with inline-flex and prevented label wrapping.
- Standardized top/right/bottom/left padding per component family.
- Kept list content flat with divider rows instead of unnecessary enclosing cards.

### P2 — Missing product patterns

The first pass added the Summation-derived inventory. The current DS-02 retains workflow, list, card, menu, calendar, and settings primitives while replacing the speculative analytics dashboard with live OpsTracer reachability, source-health, incident, on-call, contact, and organization patterns.

The follow-up Settings audit found that the catalogue still represented administration as one isolated Addison form card. That card was removed and replaced with a complete settings shell: grouped rail navigation, profile value rows, preference/default controls, integration installation, member management, usage summary/table, billing management, OAuth empty state, and the same split-panel loading skeleton used by loaded Usage content. The specimen uses OpsTracer names and semantics rather than copying account data from the inspected product.

The authenticated Billing view was then re-audited as a distinct component family. Its measured content rail is 668px inside the 1200px Settings dialog. The current-plan card uses a 12px radius, a roughly 80px header with 16px vertical/20px horizontal padding, and a two-column 160px invoice area. Management uses a separate limit card with the same header rhythm, a 6px progress track, 11px mono/tabular usage labels, and a 76px subscription row. These rules now live in `billing-system.css` and are shared by the catalogue and rebuilt application; only OpsTracer plan names and operational copy are used.

### P3 — Documentation drift

Updated the documentation to distinguish **source measurement** from **applied OpsTracer design**. After auditing the current OpsTracer dev application, evidence was moved to the shared light product surface. The former dark replay surface and decorative notch system are retired; only the user-approved compact evidence terminus cut remains.

## Verification and score

The reference render and the authenticated `/connectors` flow confirm the active field model: 44px/8px primary fields, 32px/6px compact secondary controls, 12px inline padding, an ink focus border, and matching-width 8px dropdown menus with roughly 32px rows. DS-02 preserves the 252px catalog sidebar, 18px tags, 300px workflow node, 192px menu, and 221×228px calendar while applying these rules to OpsTracer-derived content.

| Dimension | Score | Notes |
| --- | ---: | --- |
| Accessibility | 4/4 | semantic headings, labelled fields/comboboxes, named icon actions, visible focus and reduced-motion support |
| Performance | 4/4 | no remote dependency; product cascade is isolated and compact |
| Responsive design | 4/4 | sidebar drawer, collapsing grids, contained table overflow, and 44px touch targets below 820px |
| Theming | 4/4 | one product token namespace and light application model |
| Anti-patterns | 3/4 | no large dark surfaces, gradient text, accent stripes, or nested card stacks; the static catalogue still uses several specimen cards by necessity |

Total: **19/20**. P0: 0. P1: 0 remaining. P2: 0 remaining in the audited component document. P3: 1 catalogue-only opportunity to flatten additional explanatory specimens if the page grows.

## Remaining limit

This is a clean-room reconstruction of visible UI, not access to Summation's private design-system source. The follow-up OpsTracer route audit is documented separately so future application states can be checked without conflating the two sources.
