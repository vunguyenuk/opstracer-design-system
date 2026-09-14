# OpsTracer dev application UI audit

> Historical first-pass audit. The complete route/function audit and rebuild mapping is in `WEB-APP-REBUILD-AUDIT.md`.

Read-only audit completed 13 September 2026 against `https://app.dev.opstracer.ai/` in the authenticated Chrome session.

## Routes inspected

| Route | Components observed |
| --- | --- |
| `/dashboard` | 256px app rail, overview header, reachability empty state, source-health warning, incident summary, on-call card |
| `/incidents` | status underline tabs, service select, dense incident feed, semantic tags, machine ID metadata |
| `/schedules` | eyebrow/page header, amber create action, explanatory copy, four-column schedule table |
| `/contact-methods` | page header/action, verified contact row, destructive text action |
| `/organization` | underline tabs with counts, role summary, settings rows, 44px text field/select, helper text, copy action, disabled save state |
| `/my-shifts` | personal page header and no-schedule empty state |
| `/billing` | active-plan summary, three usage progress meters, billing interval control, plan comparison table and upgrade actions |

No data-changing control was used.

## Measured component inventory

Measurements are taken from the rendered 2542×1536 browser capture and rounded to the nearest whole pixel where browser scaling prevents a stable subpixel reading.

The live OpsTracer application is the source for workflows, labels, row content, and information architecture. It is **not** the styling source for DS-02 controls or surfaces; those are remapped to the audited Summation `/connectors` language.

| Element | Observed in OpsTracer dev | Applied DS-02 rule |
| --- | --- | --- |
| Application sidebar | 255–256px | retain the existing 252px catalog shell token |
| Centred content rail | about 1376px | `--content-rail: 1376px` for every catalog section |
| Page header | about 168px | content retained; rebuilt as an open header with one quiet bottom divider |
| Primary action | about 40px high | content retained; remapped to the 36px DS-02 action, amber, 6px radius |
| Text input/select | about 44px high | remapped to Summation connector fields: 44px, 12px inline padding, 8px radius |
| Underline tabs | 36–44px interaction row | replaced with the Summation-style 32px surface switch and 28px items |
| Schedule row | about 47px | compact four-column table row with one divider |
| Incident row | about 118px | icon tile, multi-tag metadata, title, mono ID/time, trailing affordance |
| Status tag | 18–22px depending content | inline-flex, vertically centred, no wrapping |

## P1–P3 findings and resolution

### P1 — three competing content widths

The intro used 900px, ordinary sections 1100px, and dashboard 1400px. This caused the left and right edges to jump between sections. All three overrides were removed; intro and every section now inherit one 1376px rail.

### P1 — large black application surfaces

The reverse logo card and incident replay panel formed two large black blocks that do not match the live light application. The redundant reverse lockup was removed and the evidence timeline was remapped to the shared light surface, neutral dividers, and semantic nodes. The final approved pattern is a three-event panel with a 40px quiet header, 40px row rhythm, 42px footer, and one isolated 18px terminus cut.

### P2 — component content was incorrectly carrying legacy styling

The first reconciliation copied OpsTracer dev geometry together with its component inventory. That conflated the two sources. DS-02 now keeps the OpsTracer workflows but applies the audited Summation `/connectors` rules: 44px/8px primary fields, 32px compact fields, 36/32/28px actions, 32px surface-switch tabs, 12px rounded shells, and restrained internal dividers.

### P2 — missing live-product patterns

Added a dedicated Application patterns section with page header/action, reachability and no-schedule empty-state language, source-health warning, dense incident row, schedule table, contact method, organization settings rows, usage meters, and a plan comparison table. A subsequent Summation Settings audit adds the missing full administration shell while retaining OpsTracer identities, permissions, incident-analysis usage, and OAuth language. Every control and surface is explicitly remapped to DS-02 rather than copied visually.

### P3 — typography drift

Host Grotesk remains the only family for headings, controls, labels, and human-readable content. DM Mono is restricted to IDs, times, codes, and measurements. The new specimens follow the same rule.

## Implementation boundary

The audit records visible behaviour and information structure only. It does not copy private source code, assets, or implementation details from the dev application. Production OpsTracer identity, content semantics, fonts, and amber hierarchy remain the identity source of truth; Summation `/connectors` remains the component-style source of truth.
