# Mandatory web-app and design-system rules

These instructions apply to every human or coding agent that edits product UI in this repository. Read this file, `.impeccable.md`, `design-tokens.css`, and the relevant rendered page before changing HTML, CSS, JavaScript, assets or copy. “Looks close” is not acceptance criteria.

## Source-of-truth order

When references disagree, use this order:

1. The user's latest explicit direction and approved rendered state.
2. This file and `.impeccable.md`.
3. `design-tokens.css` for foundations.
4. Shared primitives in `summation-app.css` / `index.html`, then reusable web-app primitives in `web-app.css`.
5. `SUMMATION-APP-UI-AUDIT.md` for geometry and `OPSTRACER-DEV-UI-AUDIT.md` for domain structure.
6. Screenshots are visual references, never a license to hard-code pixels or copy branding/data.

If a requested value is not represented by a token or shared variant, document the gap and add the reusable token/variant first. Do not hide uncertainty with a page-specific override.

## Required implementation workflow

Before editing:

1. Inspect the existing primitive and every selector that currently overrides it.
2. Identify the relevant token for spacing, type, colour, radius, control height and icon size.
3. Check default, hover, focus-visible, active/selected, disabled, loading, empty, error and expanded states that apply.
4. Check desktop and narrow layouts. Page actions and tab navigation share one row when space allows and wrap intentionally on narrow screens.

After editing:

1. Reload the rendered page; do not trust a stale `file://` tab or cached stylesheet.
2. Measure computed dimensions in the browser. Do not approve spacing by eye alone.
3. Exercise every affected tab, dialog, menu, details/summary and keyboard path.
4. Run `git diff --check`, a CSS brace-balance check and `node tools/build-icons.mjs --check` when icons are involved.
5. Confirm zero horizontal overflow, no duplicate IDs, named controls, visible focus and correct heading order.
6. Bump the stylesheet query version in the consuming HTML whenever a CSS change could otherwise be cached.

## Foundations that may not drift

### Colour

- Use only semantic tokens from `design-tokens.css`; do not add raw hex, RGB or HSL values in page CSS.
- Product neutrals are `--surface`, `--surface-subtle`, `--component`, `--active`, `--border`, `--divider`, `--text`, `--muted` and `--disabled`.
- Brand amber is reserved for the single consequential primary action. Secondary actions remain neutral and outlined.
- State colour never stands alone. Pair it with a written label, icon shape or both.
- All table headers use `--table-head-bg`/`--surface-subtle`; do not ship white table-header rows.
- Do not introduce gradient text, decorative gradients, glassmorphism or coloured side stripes.

### Spacing and padding

- Use the 4px scale only: `--space-2xs` 4px, `--space-xs` 8px, `--space-sm` 12px, `--space-md` 16px, `--space-lg` 24px, `--space-xl` 32px, `--space-2xl` 48px, `--space-3xl` 64px and `--space-4xl` 96px.
- Use `gap` for sibling spacing. Avoid margins used only to simulate a gap.
- Related title/metadata content uses a tight 8px rhythm. Card header/body separation uses 12–16px depending on density.
- Do not add arbitrary values such as 5px, 7px, 10px, 14px, 18px or 22px unless they are derived geometry inside a documented component variable.
- Do not change spacing on only one page when the same component exists elsewhere.

### Typography

- Human-language UI uses Host Grotesk. IDs, timestamps, hashes, code and machine facts use DM Mono.
- Product body text is 13px with approximately 18–20px line-height; form controls are 14px.
- Page/detail `h1`: 28/34px, weight 500. Section title and standard panel `h2`: 16/22px, weight 600.
- Secondary/helper copy must use `--muted` and remain at least 12px/17px.
- Web-app page and detail headers start with the actual `h1`; do not add page-type eyebrow labels such as `Service`, `Person`, `Team` or `Escalation policy` above it.
- Omit empty-description placeholders such as `No description yet` from headers and list rows. When an editable detail field must remain visible, render its empty value as an em dash.
- Do not invent isolated type sizes or use font size to compensate for incorrect spacing.
- Preserve real text; do not replace a logo with typed brand text.

### Fields and forms

- Every text, search, select, date, time and number field is `--control-h` (36px), 14px text, `--control-pad` (12px) inline padding and `--control-radius` (8px).
- Leading-icon fields reserve 36px on the leading side. Selects reserve space for the 16px chevron.
- Textareas use the same padding/radius and a minimum height of 88px.
- Label → control → helper spacing is 6px. Every control needs a programmatic label, visible focus, disabled state and validation/error message where applicable.
- `--control-h-compact` is for icon buttons/menu rows only. Never create a compact form field.
- Buttons aligned with a field must match its 36px height.

### Buttons, tags and actions

- Use the shared 36/32/28px hierarchy: primary 36px, default 32px, compact 28px.
- Primary action: 8px radius and 12px inline padding. Default/compact: 6px radius and 10/8px inline padding.
- Do not make every action primary. Use one consequential primary action per context.
- Tags are labels, not buttons: minimum 18px height, 6px inline padding, `--tag-radius` (4px), nowrap and semantic written text. Do not turn them back into fully rounded pills.
- Visually compact controls must retain an adequate hit target. Minimum WCAG target is 24px; target 44px on touch layouts using padding or pseudo-elements.
- Header actions must align on the same row and top edge as their associated title/status on desktop.

### Icons and brand assets

- Use the existing generated Lucide-style outline token family. Do not use emoji, Unicode stand-ins, CSS-drawn approximations or a second outline family.
- Run `node tools/build-icons.mjs` after editing local icon SVG sources.
- Standard icon size is 14–16px. Icon buttons use explicit 24/32/36px hit boxes with a centered icon.
- Brand actions must use the approved SVG in `assets/integrations/`; never approximate GitHub, Slack, Datadog or another brand with a generic glyph.
- Icon colour inherits text unless colour communicates a documented semantic state.

### Panels, tables and lists

- Panels use a 12px radius, 0.5px divider border and `--shadow-card`. Strong shadows are overlays only.
- Do not nest decorative cards. Use spacing and dividers inside a real work object.
- Data tables/lists use a quiet gray header, one rounded outer shell and 0.5px internal dividers. Do not border individual cells.
- Member table header is 40px and member rows are 58px. Other dense rows must use an existing shared variant rather than a new height.
- Definition rows have dividers only between rows; never leave a trailing divider that looks like an empty row.

### Tabs and responsive layout

- Page-level tabs use the shared 40px underline row with muted inactive text and a 2px active indicator. Use segmented controls only for mode switches, not page navigation.
- Tabs and their context action remain in one horizontal row on desktop. On narrow widths, keep tabs horizontally scrollable and wrap/move the action intentionally.
- Desktop application geometry: 252px sidebar, 49px topbar, 10px shell gutter and 1376px content rail.
- At or below 1180px, the incident utility rail stacks below the main column. At 820px the application navigation becomes a drawer. At 720/680px analytical grids collapse and long tab rows scroll.
- Never solve responsive problems by hiding critical actions or shrinking text below the system scale.

## Approved Incident workspace details

Preserve these values unless the user explicitly changes them and the shared variant is updated:

- Every incident detail route uses the shared incident workspace shell: priority tile, title/status row, aligned header actions, five-column context strip, underline navigation and a persistent utility rail. Do not fall back to the legacy tags-above-title header.
- Standard incidents expose Overview and Timeline as real tabs; only the selected tab's main content is visible. Their context and timeline data remain incident-specific rather than copied from the AI example.
- Incident tab changes preserve the current scroll position. Do not call `scrollIntoView` or otherwise move the viewport when a tab is selected by pointer or keyboard.
- Incident status follows the final title word in normal inline flow, including when a long title wraps. Apply the shared 2px optical lift so the compact label sits visually centred against the larger title type; title-to-metadata gap is 8px.
- Header action buttons are 36px high and aligned with the title block.
- Utility rail headers use a compact 44px rhythm. `Paging progress` and `Responders` bodies start 12px below the header divider.
- Timeline rows are 44px apart. Each timeline node is 28px with a 16px Lucide icon, white `--surface` background, `--border` gray outline and `--text` icon colour.
- The timeline connector and every node derive from the same track-center variables. Expanded detail aligns with the content column; it must not introduce a second stray vertical rule.
- Timeline summary and expanded detail must be rendered from the same event object. Never relabel only the summary.
- In schedule calendars, keep the Members list neutral. Only the shift covering the current time uses its assigned `--rota` colour at full strength; its text is white and its avatar/status surfaces are white with dark text. The current-day marker uses that same member colour in Week and Month views. Keep the written “On call” label so colour is never the only state indicator.
- Only the selected Incident tab renders its corresponding main content. Do not turn the page back into one long stack of all sections.

## Approved Service detail decisions

- The service title begins the header directly, without a `Service` eyebrow or an empty-description placeholder.
- Service tabs and the active tab's primary action share one `tabbar` row on desktop. `Add integration` appears only for Integrations; `Add suppression` appears only for Suppressions.
- Event handling follows one scan path: organization-wide grouping warning → reader identity/logic → severity mapping → grouped-incident history → lifecycle explanation.
- The organization-wide grouping warning uses the amber semantic callout with a real 14–16px outline information icon. Do not replace it with plain orange text or an oversized alert.
- Datadog uses the approved purple SVG asset. In the reader summary, its vendor container is 48px and the brand mark is 32px; do not shrink it to a generic 16px app icon or redraw it.
- Reader summary content uses 16px internal padding and a 16px gap. Name/status share one row; activity is separated by a quiet divider.
- Severity mapping uses `--surface-subtle`, not an isolated custom gray. Its two controls share one row when space permits and both remain the standard 36px field height.
- Mapping/history headers use the global gray table-header token. Grouping keys and other machine values use DM Mono.
- Below 680px, mapping fields collapse to one column and the action becomes full-width; no field may become shorter.

## Approved Member detail decisions

- The member header contains avatar, name, description and email/copy action; role/remove actions align at the opposite edge on desktop. It does not use a `Person` eyebrow.
- The content layout is a two-column details/contact composition (`1.65fr / 1fr`) and collapses to one column below 1180px.
- Member card headers are 64px with a 20px outline icon and 16px internal gap.
- Member fact rows are 80px and use dividers only between rows. Fact icons occupy a 24px alignment box.
- Contact-method rows are at least 64px. Their visual icon container is 40px with an 18px outline icon; status remains a small written tag such as `Verified` or `Unverified`.
- Teams are quiet neutral chips. Do not turn role/team/status labels into oversized action buttons.
- Destructive removal remains visually distinct from neutral editing and always uses a real trash icon plus explicit text.

## Prohibited shortcuts

- No page-specific copies of an existing component measurement.
- No `!important` to win component styling; fix the cascade or create a named variant.
- No new generic class names such as `.chip`, `.card2` or `.small` that leak across systems.
- No direct editing of generated icon token blocks.
- No silent replacement of official brand assets.
- No visual-only state, inaccessible icon-only action or hidden interactive affordance.
- No shipping from a screenshot comparison alone; validate computed styles and interaction states.

## Definition of done

A UI change is complete only when it uses shared tokens/components, matches the approved rendered behavior, passes keyboard and responsive checks, contains no stale/cross-wired content, and leaves `git diff --check` clean. Record any intentional exception next to the shared primitive, not as an unexplained route override.
