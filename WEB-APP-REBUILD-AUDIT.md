# OpsTracer dev web application — complete UI audit

Read-only audit completed 13 September 2026 against `https://app.dev.opstracer.ai/` in the authenticated Chrome session. No create, update, billing, resolve, delete, rotate-key, mute, disable, or invitation action was submitted.

## Executive score

| Area | Score / 20 | Main issue |
| --- | ---: | --- |
| Accessibility | 15 | Good accessible names and native roles; loading states and some icon-only controls need clearer announcements |
| Performance perception | 13 | Several route transitions expose large skeleton blocks or a lone spinner without persistent page structure |
| Responsive design | 14 | Sidebar and tables are structurally sound, but dense tables/calendar require explicit small-screen alternatives |
| Theming | 11 | Light/dark/system control exists, while component styling is still coupled to the old warm-gray visual language |
| Design-system consistency | 9 | Competing border, radius, field, button, tab, panel, and spacing treatments remain across routes |
| **Total** | **62 / 100** | Functionally broad, visually fragmented |

## Complete function and route inventory

| Area | Route/state inspected | Functions retained in the rebuild |
| --- | --- | --- |
| Overview | `/dashboard` | reachability, source health, incident summary, current on-call, deep links |
| Incidents | `/incidents`, All/Open/Acknowledged/Resolved, service filter | dense incident list, priority/status/escalation metadata, paging, detail navigation |
| Incident detail | `/incidents/:id` | resolve action, alert context empty state, delivery/assignment/acknowledgement/paging timeline |
| Services | `/services`, New service | service table, receiving state, create form, policy binding, description |
| Service detail | `/services/:id` | Details, Integrations, Event handling, Suppressions; edit/save/mute/delete states |
| Escalation policies | `/escalation-policies`, New policy | policy table, default/repeat states, create form |
| Policy detail | `/escalation-policies/:id` | level wait/unit/targets, add level, repeat chain, acknowledgement timeout, save/delete |
| Routing rules | `/routing-rules`, New rule | empty/loading states; severity, integration, source, tags, hints, target policy and enabled conditions |
| Integrations | `/integrations`, Add integration | source cards, provider/type choice, disabled coming-soon sources, service/severity/key metadata |
| Integration detail | `/integrations/:id` | setup guide, test event, rotate key, delete, status, default severity, rate limits, deliveries/refresh/empty state |
| On-call | `/schedules`, New schedule | schedules table; name, timezone, layers, targets, duration, first handover, restricted time window |
| Schedule detail | `/schedules/:id` | current responder summary, edit/delete, month/week calendar, previous/next/today, shift and override semantics |
| My shifts | `/my-shifts` | personal no-schedule empty state |
| Contact methods | `/contact-methods`, Add method | registered methods, verified state, remove, channel/address/help/create flow |
| Billing | `/billing` | plan state, manage billing, SMS/voice/responder meters, monthly/yearly switch, comparison table, upgrade/contact |
| Organization settings | `/organization` | General, Members, Teams, Invitations, Notifications |
| General | `?tab=general` | role, org name, timezone, Slack token/help, slug, org ID/copy, save state |
| Members | `?tab=members` | search, role filter, member table, contact-method popover, row actions/detail |
| Teams | `?tab=teams` | team list, member count, create/detail |
| Invitations | `?tab=invitations` | search, role filter, empty state, invite email/role/teams form |
| Notifications | `?tab=notifications` | ordered delivery toggle, delivery preview, limitation/help copy, save state |
| Shell | all routes | workspace switcher, collapsible sidebar, account menu, organization settings/sign out, light/dark/system selector |

## Component remapping contract

The live application is the source for workflows, labels, domain semantics, and information architecture. It is not the style source. Every retained function is mapped to DS-02:

| Component | DS-02 implementation |
| --- | --- |
| App rail | 252px desktop, 64px compact, off-canvas mobile; 32px nav rows; 6px active radius |
| Content rail | single 1376px maximum with 48px desktop and 16px mobile gutters |
| Page header | open surface, one bottom divider, 28/34 title, 14/21 description |
| Buttons | 36px primary, 32px default, 28px compact; amber only for consequential primary action |
| Inputs/selects | 44px default, 32px compact, 8/6px radius, 12/10px inline padding |
| Tabs/segments | 32px shell with 28px items and no underline drift |
| Tags | minimum 18px, inline-flex center alignment, one-line text |
| Tables/lists | 40px header, 58px standard row, restrained 0.5px internal dividers |
| Cards/panels | 12px radius, one border and card shadow; no nested decorative shells |
| Dialogs | 16px radius, 520px default/680px wide, stable header/footer, internal scroll |
| Calendar | 52px toolbar, 7-column semantic grid, consistent shift chips and overflow handling |
| Progress | 6px track with semantic fill; label/value share one baseline |
| Typography | Host Grotesk for human UI; DM Mono only for IDs, keys, times, metrics and machine facts |

## Findings by severity

### P0

No destructive, privacy, or navigation-blocking defect was observed during the read-only audit.

### P1 — visual foundation is inconsistent across the complete workflow

Warm-gray backgrounds, high-frequency outlined containers, amber rail strokes, square actions, pill states, underline tabs, large skeletons, and heterogeneous card geometry coexist. The rebuild replaces them with one light neutral foundation, one radius ladder, one divider rule, and one compact control density.

### P1 — responsive fallbacks are incomplete for operational density

Incident, member, billing and integration tables assume a wide viewport. The schedule month view is inherently wider still. The rebuild supplies overflow-safe table/calendar containers, collapses the rail at tablet width, and uses an off-canvas rail plus stacked headers/forms on mobile.

### P1 — long incident content can dominate the page

Raw alert titles and machine IDs are valuable but compete with status and action hierarchy. Lists now truncate safely, while the detail route exposes the complete title and keeps machine facts on a dedicated metadata line.

### P2 — loading feedback erases context

Billing, integration detail and schedule detail briefly show a lone spinner or large blank skeleton. Loading states should preserve the page header and approximate final geometry, announce progress, and never look like missing content.

### P2 — control sizing and alignment vary by route

The old app mixes 40–48px actions, square selects, inconsistent inline padding, undersized icons, and differing label/help spacing. The rebuild uses the audited 36/32/28 action ladder and 44/32 field ladder everywhere.

### P2 — component boundaries are overdrawn

Several pages outline the page header, content region, rows, cards, and nested blocks simultaneously. The rebuild uses open page headers, borderless composition where hierarchy is already clear, and one enclosing border for data groups.

### P2 — destructive and external actions need clearer hierarchy

Resolve, rotate key, disable, mute, remove and delete are visually close to ordinary controls. DS-02 keeps one amber primary action, neutral secondary actions, and a red destructive treatment only in explicit confirmation or danger zones.

### P2 — state and filter patterns are fragmented

Tabs, pills, filters, severity selectors and notification toggles use separate geometry. They are consolidated into the same compact tab, 18px tag, 32px select and segmented-control families.

### P3 — wording is strong but metadata typography drifts

The product’s operational copy is concise and useful. Host Grotesk now owns labels and prose; DM Mono is restricted to IDs, timestamps, keys, measurements and machine-originated facts.

### P3 — theme control exceeds the current DS-02 surface contract

The live shell offers Light, Dark and System. DS-02 currently defines a complete audited light product palette only. The rebuilt prototype retains the audited light output and documents dark/system as a future token-mode extension instead of inventing a second ungoverned palette.

## Positive findings

- Navigation taxonomy maps well to incident-response mental models.
- Accessible names, tabs, dialog roles, comboboxes, tables and progress indicators are present across most audited screens.
- Form helper copy explains operational consequences instead of repeating field labels.
- Domain states are concrete and actionable.
- Detail pages keep immutable incident behaviour separate from editable configuration.

## Implemented artifacts

- `web-app.html`: interactive DS-02 rebuild covering every audited route family, route switching, tabs, filters, tables, cards, calendar, dialog flows, notifications and detail views.
- `web-app.css`: responsive application composition using the existing `design-tokens.css` foundation.
- `index.html` and `landing.html`: expose only the local rebuilt preview; the obsolete external Web app menu item has been removed.

## Boundary

This work reconstructs visible behaviour and information architecture. It does not copy private source code, hidden APIs, or implementation details from the live application. Data-changing controls in the prototype are demonstrations and do not mutate the live environment.
