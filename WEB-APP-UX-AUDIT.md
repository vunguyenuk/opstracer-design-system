# OpsTracer rebuilt web app — UI/UX audit

Audit scope: all 18 local routes in `web-app.html`, their shared navigation, controls, tables, cards, empty states, billing surface, calendar, popovers, dialogs, and responsive states. Evidence combines the supplied screenshots, source inspection, token/asset validation, contrast calculation, and the deterministic Impeccable scan.

## Audit health score

| Dimension | Score | Finding |
| --- | ---: | --- |
| Accessibility | 3/4 | Named icon controls, active navigation state, keyboard tabs, dialog focus containment, runtime form labels, and reduced-motion support are present. Real submission validation remains prototype-only. |
| Performance | 4/4 | Static local assets, no runtime framework, no layout-reading loop, and transform-only drawer motion. |
| Responsive design | 3/4 | 44px mobile targets, drawer scrim/dismissal, single-column reflow, and contained table overflow. Physical-device verification remains outstanding. |
| Theming | 4/4 | Product colour, surface, overlay, elevation, and z-index values resolve through shared tokens. |
| Anti-patterns | 4/4 | No decorative side stripe, dark evidence slab, gradient text, injected brand chrome, or speculative dashboard metrics. |
| **Total** | **18/20** | **Excellent — minor functional hardening remains.** |

## Defects found and resolved

- **P1 — Invisible icon system:** external SVG masks rendered blank when the application was opened from `file://`. Navigation, search, select, refresh, pagination, calendar, dialog, and empty-state icons now use direct SVG background rendering. All icon boxes remain 16px inside explicit control hit areas.
- **P1 — Unauthorised application chrome:** the injected OpsTracer logo and circular sidebar-collapse control did not exist in the audited application structure. Both the markup and their state logic were removed.
- **P2 — Misleading inactive control:** the theme button exposed two “Planned” modes without working theme variants. The control and dead menu branch were removed.
- **P2 — Unicode icon drift:** carets, select arrows, refresh, pagination, calendar navigation, dialog close, and state markers now resolve to the shared outline family rather than depending on font glyphs for their visible artwork.
- **P2 — Mobile reach and dismissal:** navigation and controls use 44px touch targets below 680px. The drawer now has a scrim and closes when the user selects outside it.
- **P2 — State and keyboard gaps:** active navigation exposes `aria-current`; tabs expose tab roles, selected state, arrow-key navigation and related tab panels; segmented controls expose pressed state; dialogs keep focus inside, support Escape, and return focus on close.
- **P2 — Typography floor:** functional micro-labels and avatars no longer render below 11px.
- **P3 — Token drift:** overlay colours and the z-index ladder moved into shared tokens; reduced-motion behaviour now disables non-essential smooth transitions.

## Deterministic scan

After remediation, `npx impeccable --json web-app.html` reports only `cramped-padding`. These 46 hits are false positives caused by the scanner evaluating structural wrappers without resolving the linked stylesheet: tables intentionally place padding on cells, panels on their header/row children, and tabs use the audited 2px inset switch geometry. Source inspection confirms 12–16px content inset for text-bearing children. The prior undersized-text and skipped-heading findings are resolved.

## UX critique

| Nielsen heuristic | Score | Note |
| --- | ---: | --- |
| Visibility of system status | 3/4 | Active route, selected state, status tags and toast feedback are visible. |
| Match with the real world | 4/4 | Incident, on-call, routing, source and billing terminology follows the product domain. |
| User control and freedom | 3/4 | Back, Cancel, Escape, outside-dismiss and drawer dismissal exist; destructive undo is not implemented. |
| Consistency and standards | 4/4 | One token, control, icon and density system covers all routes. |
| Error prevention | 2/4 | Constraints exist, but real validation and destructive confirmation depend on product logic outside this prototype. |
| Recognition rather than recall | 4/4 | Sidebar items retain text labels and state; icons supplement rather than replace copy. |
| Flexibility and efficiency | 2/4 | Keyboard tabs and direct routes work; bulk actions and product shortcuts are not represented. |
| Aesthetic and minimalist design | 4/4 | Unsupported logo, collapse affordance and theme control were removed. |
| Error recovery | 2/4 | Empty states guide recovery, but submitted-error flows are not connected. |
| Help and documentation | 3/4 | Inline explanations and setup guidance are present; global help is outside this surface. |
| **Total** | **31/40** | **Good.** |

Cognitive-load checklist: one of eight checks fails. Settings contains five peer tabs, but they remain chunked in one predictable switch and reveal one panel at a time. All other task decisions expose four or fewer primary choices. The resulting load is low.

## Remaining product-level work

The local prototype visually and behaviorally represents the audited routes, but it does not submit real mutations. Production integration still needs server validation, loading/error handling, destructive confirmation with actual consequences, and persistent state. These are functional implementation requirements rather than unresolved layout or design-system defects.
