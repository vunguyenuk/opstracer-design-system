# OpsTracer web app — live parity audit

Read-only audit on 18 Sep 2026 of `https://app.opstracer.ai` (org c0x12c) compared with `web-app.html`. Nothing in the live app was created, changed or submitted.

## 1. Alignment fixes

| Issue | Fix |
| --- | --- |
| The org switcher's centre sat about 3px above the breadcrumb's centre. | `.wa-sidebar>.workspace` now uses `height: var(--app-topbar-h)` and a .5px top offset. Both now share one centre line at y=35px. |
| The P1–P5 segmented control stretched to fill the whole row. | `.segmented` is now `inline-flex` with `width: fit-content` and a 2px gap. |

## 2. Gaps closed in `web-app.html`

### Schedule detail (the page that was audited)

- **Calendar is now interactive.** It renders from the rotation data (daily turns from Tue 15 Sep, 00:00, six people). Week and Month views both work, and Previous, Next and Today all work.
- **Visual states.** Each person has a colour. "Nobody on call" shows as a red block, override cover uses a striped fill, the current shift has a green outline, and there is a current-time line. The legend in the footer matches these states.
- **Shift drawer.** Clicking any shift opens a drawer. It shows the shift's status (running now, upcoming or ended) and its start and end times in Asia/Saigon. It also has a "Create an override" form: who takes the shift, from, until, and a summary line. "Arrange cover" adds the override and the calendar redraws.
- **Overrides list** in the side panel. Each override can be removed.
- **Import from calendar** drawer, with an .ics drop zone.
- **Edit schedule** drawer, rebuilt to match the live one. It has name, description and time zone (with the detected zone shown), then the on-call layers (1/10). Each layer has a label, an ordered list of people with their colour bars, handover timezone, turn length and first handover. It also has "Always in effect / Restrict to certain times", plus "Add layer" and "Follow-the-sun template".

### Other pages

- **Overview.** New "8 people cannot be woken" panel with a table. Source health now reads "2 sources are not reporting". Open incidents shows "Waiting for someone" and "Acknowledged" counts. On call now lists all 3 schedules, including one person covering another shift.
- **Unreachable people** (`/unreachable-people`). New page with three stat tiles and the full table of people who cannot be woken.
- **Incidents.** Added a "Page someone" button with its drawer: title, description, severity, who to page (person, team, schedule or policy) and a channel override. Tabs now show counts. Added filters for when it was opened, priority and service, plus rows per page.
- **Incident detail.** New Alert context section: service, tagged, via, state, monitor, scope and fired. Below that are the query, the tags as chips, and a collapsible raw webhook payload. Also added the "How an incident closes" explainer and the "Page someone" action.
- **Services and service detail.** Now show the live data, the notice that a routing rule may send some alerts elsewhere, and who created the service and when.
- **Escalation policies.** Added an Actions column with "Set as default" and "Current default". Policy detail gains "Make default", "Used by", targets as removable chips, and "Repeat up to 5×".
- **Routing rules.** An ordered rule list (condition chips, the policy it pages, enabled state, reorder buttons) replaces the empty state. The New rule drawer now covers tags, routing hints, source type, and the warning shown when a rule matches every alert.
- **Integrations.** Now shows the 3 live sources, including Grafana. The Add integration picker has Grafana enabled and Alertmanager marked "Soon". Integration detail shows the delivery log instead of the "Listening for events" state.
- **On-call list.** Now shows the 3 live schedules.
- **My shifts.** Copy updated to match the live app.
- **Contact methods.** Added a "Paired iOS devices" section. The add drawer now offers Slack, Telegram, iOS device, Voice and SMS.
- **Settings.**
  - General: added Slug, the detected timezone and the save state.
  - Members: 15 members, a contact-methods popover, and a row menu with "Remove from organization".
  - Teams: 4 team cards with avatar stacks.
  - Invitations: added a role filter.
  - Notifications: a full editor for ordered delivery tiers.
- **Member detail** (Person). Added contact methods (Email, Voice, iOS device) and "Remove from organization". **Team detail**: added "Invite member", "Add members", a Status column and Remove.
- **Billing.** Now shows the Business plan as active, live usage, and the six-column plan table with Enterprise.
- **Shell.**
  - Breadcrumbs now show the full path, e.g. "OpsTracer / Alert routing / Services / Service detail".
  - The sidebar can collapse to a 64px rail.
  - The workspace switcher lists c0x12c and Spartan, plus "Create organization".
- **New pages.** Welcome (`/welcome`, inside the app shell), plus three full-screen pages outside the shell: Create organization (`/orgs/new`), accepting an invitation (`/invite/$token`) and No organization (`/no-organization`).

### Second pass (same day)

- **Open incident state** (new `incident-open` view). Open rows in Incidents and Overview now open it. It has Acknowledge, Resolve (with a resolve drawer) and Page someone, shows the source state as "Triggered", and its timeline includes on-call assignments that came through an override.
- **Service detail → Integrations tab.** Now shows live data and an Actions column (Setup guide).
- **Service detail → Event handling.** Now has the organization-wide grouping-key explainer, the per-integration grouping key read by the Datadog reader, the vendor values seen in the last 30 days, severity mapping with its help text, and the table of grouping keys → incidents.
- **Drawers updated to the live copy.**
  - Mute service: 1 h / 4 h / 8 h / until 09:00 tomorrow / pick a time, plus a reason.
  - Conditional suppression: every condition, start/end window with a 30-day maximum, a reason and "Preview matches".
  - Datadog setup: four steps, each with a copy/download block for the ingest URL, custom header and payload template.
  - Rotate key: the old key keeps working for 24 h.
  - Edit member access: multi-team select.
  - Upgrade plan: Team, Business and Enterprise.
- **Responsive.**
  - The collapse-rail button is hidden at ≤820px, where the hamburger takes over.
  - Incident filters wrap at ≤680px.
  - The schedule side panel uses a 2×2 grid at ≤1080px.
- **Demo-only controls.** Buttons that only demonstrate an action (Refresh, Slack help, removing a target, reordering rules) now show a toast instead of doing nothing.

Not rebuilt on purpose: `/data-deletion`, `/privacy` and `/terms` (marketing and legal content).

## 5. UX/UI review pass (fields, spacing, noise, incident detail, billing)

No UX-review skill is installed in this workspace, so this pass is a manual heuristic review: Nielsen's heuristics, visual hierarchy, and DS-02 token conformance.

**Fields.** Three problems. (1) Sheets mixed 44px and 32px controls in the same form (Edit schedule: name 44, layer label 32, layer selects 32). (2) Help text under one field pushed its row neighbour out of line, because `.form-grid` aligned to the end. (3) Label-to-control gaps varied from 6 to 12px. The rule is now one height per context: 44px, 14/21 text in sheets and page forms; 32px only in dense rows (settings definition lists, table toolbars). Labels are 13/18 500 with an 8px gap to the control, help text is 12/17 muted, fields are 20px apart, and grids align to the top.

**Spacing.** The page rhythm is now 8 · 16 · 24 · 32. Detail meta sits 8px under the title, with 8px between items. Panel heads use 16px padding everywhere; billing had 20px. Section headings are 16/22 600, and sub-sections start 32px down. Definition rows are 64px with 12/16 padding. The service "Created … by" line moved into the header meta instead of floating between blocks.

**Alert bars (noise).** A developer read the amber routing-rule notice and the amber delete zone as warnings, although neither is one.
- The routing-rule notice is now an inline muted hint with a route icon: "1 routing rule can send some of these alerts to *Khoa Routing Rule Policy* instead. View rule →".
- The delete zone is now a calm neutral card at the bottom, with a bordered button whose label is red; it fills red only on hover.
- Every `.notice` defaults to neutral: subtle surface, muted text. Amber is reserved for `.notice.warning` (real risk, e.g. a rule that matches every alert), and red for `.notice.danger`.

**Incident detail.**
- It is now two columns. The main column holds the alert context and the timeline. A sticky right column holds:
  - a Summary: status, priority, service, escalation policy, step reached, who was paged, opened/acknowledged/resolved, and duration or time open;
  - Deliveries, the latest result per channel, so "Voice not delivered" is visible without scrolling;
  - "Same fault before", other incidents from the same grouping key (resolved example only).
- Every timeline event can be clicked. It opens the absolute timestamp and the event's facts. A delivery shows its channel, address, provider id and attempt. An undelivered one shows the reason and a fix link. A state change shows the payload, as in the app.dev timeline.
- An All / Deliveries / State filter trims long timelines.
- The "How an incident closes" box became a one-line footnote with a link.

**Billing** now uses the app.dev content:
- Free plan, active, $0.00 per responder per month.
- Usage: SMS 30/30, Voice 10/10 and Responders 5/5. All three meters are at their limit, so they are red with "Limit reached".
- Subscription and billing → Manage subscription.
- Plan table: Team and Business have Upgrade buttons; Enterprise has Contact us.

The prod-only blocks (estimated invoice, analysis overage limit, cancel plan) were removed.

**Second sweep (automated).** A script opened every page, every tab and every sheet, and checked control height, font size and label gap against the rule above. It also looked for overflow and for text below 12px. It fixed:
- the target picker inside the token field (back to a 32px borderless select);
- Create organization fields (14px);
- tag chips and schedule member captions (12px);
- the member-detail email overflow (it now truncates);
- the duplicated helper line in the New rule sheet;
- check rows, where title and help were running together on one line.

Every "Type DELETE" confirmation now states the real consequence instead of placeholder copy, and its submit stays disabled until DELETE is typed. On mobile (375px), panel heads, the delete card and incident actions stack instead of squeezing.

## 3. Visual style aligned to the live app

Computed styles were measured on the live app at 1440px (light theme) and matched in `web-app.css` (the "Live style alignment" block at the end).

| Area | Live value now used |
| --- | --- |
| Base type | 13/18 Host Grotesk; `small` 12/17; page title 28/34 500 with −0.7px tracking |
| Page frame | 24px inset from the shell card, 24px above the title, 28px between the header and the content |
| Primary button | Dark action (#171411 on white text; #f4f1eb on #171411 in dark), 36px, 8px radius. Amber is no longer used for buttons. |
| Secondary, ghost and danger buttons | White with a .5px #d1d1d1 border; transparent ghost; soft red for danger (32px, 6px radius) |
| Links in panel footers | Amber, underlined, with an arrow ("Add your contact method →") |
| Tables | Head row has no fill: 40px, 12px muted text. Rows are 58px. The last column is a chevron. The table sits in a 12px card with a .5px divider border. |
| Incidents toolbar | Tabs on their own row, then search, date, priority and service filters, with refresh on the right |
| Sidebar | 8px padding. The header holds the org switcher (up-down chevrons) and a collapse button. The footer holds the account and a Theme button, with a divider above. Alert routing is open only while one of its pages is active. |
| Eyebrow | DM Mono 12/17, 0.08em tracking, uppercase |
| Danger zone | Amber notice row: 8px radius, 10/16 padding |
| Sheets | 480px by default. New rule, Add integration, New/Edit schedule and Import use 680px; Datadog setup uses 576px. Title 22/28 500, help 13/18. 32px bordered close button, subtle footer. |
| Page someone | Centred 512px modal: amber "PAGE SOMEONE" eyebrow, 28/34 title, severity select, channel-override checkbox cards |
| Header alignment | The sidebar header is 49px, starting at the shell gutter plus .5px, so the org switcher, the collapse button and the breadcrumb share one centre line (y = 35px). |
| Sidebar rhythm | Label 16px, 6px gap, 32px rows 1px apart, 32px between groups; text at x=16 (labels) and x=40 (items). The Alert routing children sit on a 1px rail (15px margin, 9px padding). Integrations is not in the rail, matching live. |
| Icons | Lucide, the same set the live app uses (lucide-static 1.47, ISC). They are embedded as data-URI masks, so they follow the text colour and switch with the theme. Sidebar: layout-dashboard, siren, route, boxes, waypoints, split, calendar-clock, calendar-check, contact-round, credit-card, settings. Header: chevrons-up-down, panel-left-close, layout-grid, sun/moon. Actions: plus, phone-call, check, check-check, calendar-plus, pencil, trash-2, bell-off, book-open, send, refresh-cw, external-link, copy, arrow-right, chevron-left/right, arrow-up-down, grip-vertical, ellipsis, mail, phone, smartphone, inbox, info. |
| Themes | Light / Dark / System from the sidebar footer, using the live dark tokens (#100e0d page, #151210 card, #39322d line, #f4f1eb text, sre-* status colours). The choice is remembered in the browser. |

## 4. Bugs seen in the live app, for the dev team

1. **Schedule week view:** the current-time label (11:22) overlaps the 12:00 axis label. The prototype now hides any axis label within about 1.4h of the current time.
2. **Schedule week view at 1440px:** the Sun column is clipped by the page edge. The prototype lowers the minimum grid width to 700px.
3. **Schedule header:** all three actions (Import, Edit, Delete) render as filled dark buttons. DS-02 uses secondary buttons for these and a danger style only for Delete.
4. **Members:** a member with no name set shows as a bare email with an "N" initial. Live Unreachable uses "Member with no name set" for this case; the two places should match.
