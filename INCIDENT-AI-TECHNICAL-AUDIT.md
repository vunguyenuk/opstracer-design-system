# Incident AI page — technical UX/UI audit

Audit date: 20 September 2026  
Route: `web-app.html#incident-ai`  
Scope: all five page tabs, header, context strip, utility rail, dialogs/actions, timeline collapsed and expanded states, shared tokens and responsive CSS.

## Audit health score

| # | Dimension | Score | Key finding |
|---|---|---:|---|
| 1 | Accessibility | 3/4 | Names, semantics, focus, hierarchy and contrast pass; several inline targets are too small. |
| 2 | Performance | 2/4 | The static prototype mounts all 25 routes: 3,816 DOM nodes, of which 24 pages are hidden. |
| 3 | Responsive design | 3/4 | Layout has explicit 1180/720/680px adaptations and no desktop overflow; compact targets remain weak for touch. |
| 4 | Theming | 3/4 | The incident page mostly uses tokens; legacy hard-coded theme values and page-local measurements remain. |
| 5 | Anti-patterns | 3/4 | The current page is restrained and operational, but component overrides are starting to fork the shared system. |
| **Total** |  | **14/20** | **Good — address the P1 and systemic P2 items before release.** |

Issue count: **0 P0 / 1 P1 / 5 P2 / 2 P3**.

## Anti-pattern verdict

**Pass.** The current incident page does not read as generic AI-generated UI. It avoids gradient text, decorative metric heroes, oversized icon tiles and arbitrary semantic colour. Cards are mostly justified by distinct incident-response objects. The main risk is no longer visual style; it is drift caused by page-local overrides and runtime relabelling of legacy markup.

## Executive summary

The page is visually coherent, compact and substantially more usable than the earlier long-form implementation. All five tabs reveal mutually exclusive content. Runtime checks found no duplicate IDs, unnamed visible controls, missing visible image `alt` attributes or desktop horizontal overflow. Keyboard arrow navigation updates focus, `aria-selected` and `aria-labelledby` together. Token contrast ranges from **4.92:1 to 12.08:1**, meeting WCAG AA for the tested text/status pairs.

Before release, correct the timeline data model. The script currently rewrites each legacy timeline summary but leaves its original expanded detail in place. That can show facts belonging to a different event under the new title. Then address target size, expansion affordance, reduced motion and component-token drift.

## Detailed findings

### [P1] Timeline summaries and expanded details can describe different events

- **Location:** `web-app.html:365`, `web-app.html:676-683`
- **Category:** Accessibility / information integrity
- **Impact:** An on-call engineer can open “Truc Le acknowledged” or “Cause confirmed” and receive detail inherited from a different legacy event. In an incident workflow, incorrect evidence is more harmful than missing decoration.
- **Standard:** WCAG 3.2.4 Consistent Identification; product evidence-integrity requirement.
- **Recommendation:** Use one timeline data object for time, icon, title, note and detail. Render the whole row from that object, or update both summary and detail atomically. Add a test that asserts every expanded detail matches its row ID/type.
- **Suggested command:** `/harden`, then `/polish`.

### [P2] Expandable timeline rows hide their only expansion affordance

- **Location:** `web-app.css:6991`
- **Category:** Accessibility / interaction
- **Impact:** The `<summary>` remains clickable and keyboard-expandable while `.tl-chevron` is hidden. Expansion is therefore surprising and was already reported as a visual bug.
- **Standard:** WCAG 3.2.4; affordance consistency.
- **Recommendation:** Either restore a compact 14px chevron with `aria-expanded` semantics supplied by `<details>`, or make rows non-expandable and remove the hidden details interaction.
- **Suggested command:** `/clarify`, then `/polish`.

### [P2] Citation and inline-link targets are below the product touch target

- **Location:** `web-app.css:5986` and inline `.btn.text`/`.btn.link` actions
- **Category:** Accessibility / responsive
- **Impact:** Citation chips render at roughly 18px high. They are difficult to acquire on touch screens and for users with motor impairments.
- **Standard:** WCAG 2.2 SC 2.5.8 Target Size (Minimum); project mobile target is 44px where space permits.
- **Recommendation:** Keep the visual chip compact but enlarge the hit area with padding or a pseudo-element. Never reduce a button to text line-height alone.
- **Suggested command:** `/adapt`.

### [P2] Smooth evidence jumps ignore reduced-motion preference

- **Location:** `web-app.html:670`
- **Category:** Accessibility
- **Impact:** Citation navigation always calls `scrollIntoView({behavior:'smooth'})`, even when the operating system requests reduced motion.
- **Standard:** WCAG 2.3.3 Animation from Interactions.
- **Recommendation:** Use instant scrolling under `prefers-reduced-motion: reduce`; keep the existing static focus/flash cue.
- **Suggested command:** `/animate`.

### [P2] Incident styling duplicates shared measurements instead of extending primitives

- **Location:** `web-app.css:6704-7020`
- **Category:** Theming / maintainability / anti-pattern
- **Impact:** Values such as 58x48 priority, 20px tags, 44px rail headers, 28px action rows and timeline geometry are restated in page selectors. Future agents can change one route without updating the component system.
- **Recommendation:** Promote reusable variants to the shared component layer. Keep timeline geometry as named custom properties derived from one row-size token. Prohibit raw spacing, control, icon, radius and type values in route-specific CSS.
- **Suggested command:** `/distill`.

### [P2] The prototype mounts every route and a large hidden DOM

- **Location:** `web-app.html` document architecture
- **Category:** Performance
- **Impact:** Runtime measurement found **3,816 total DOM nodes**, **1,066 incident-page nodes**, **25 mounted routes** and **24 hidden pages**. This is acceptable for a prototype but will increase style, layout and accessibility-tree cost as screens grow.
- **Recommendation:** For production, mount only the active route and lazily create heavy Evidence/Investigation content when selected. Keep the static prototype only as the visual reference fixture.
- **Suggested command:** `/optimize`.

### [P3] Tab selection always scrolls the workspace

- **Location:** `web-app.html:666`
- **Category:** Interaction
- **Impact:** Every tab click calls `panel.scrollIntoView({block:'start'})`, even when the panel is already visible. This can create avoidable viewport jumps.
- **Recommendation:** Scroll only when the selected panel is outside the viewport, and avoid animation when reduced motion is requested.
- **Suggested command:** `/polish`.

### [P3] Legacy dark-theme values are hard-coded outside the current token source

- **Location:** `web-app.css:4643-4675`
- **Category:** Theming
- **Impact:** A second hard-coded palette can diverge from `design-tokens.css` and conflicts with the current light application contract.
- **Recommendation:** Either remove the unsupported theme or promote it to an explicit token mode in `design-tokens.css` with contrast tests.
- **Suggested command:** `/distill`.

## Systemic patterns

1. **Patch layering:** the same component is defined in a base block, a “second sweep” and a late route override. Later specificity currently wins, but this is fragile.
2. **Runtime relabelling:** JavaScript converts legacy markup into the new incident design instead of rendering a consistent data model.
3. **Small interactive text:** compact visual density is sometimes implemented by shrinking the hit box instead of only shrinking the visible treatment.
4. **Prototype/production boundary:** all routes and heavy evidence content remain mounted even when hidden.

## Positive findings

- All five tabs show one content view at a time.
- Arrow-key tab navigation correctly synchronizes focus, `aria-selected` and the tabpanel label.
- Heading hierarchy is consistent: one `h1`, followed by contextual `h2`/`h3` headings.
- Visible controls have accessible names; no duplicate IDs were detected.
- Visible images have `alt` attributes; decorative brand/icon imagery is correctly empty-alt or `aria-hidden`.
- Focus uses a visible 2px blue outline.
- Tested contrast ratios: text 12.08:1, muted 5.17:1, amber 5.60:1, blue 6.06:1, green 4.92:1 and red 5.56:1.
- Desktop runtime shows no horizontal overflow.
- Header status, actions and metadata follow the requested compact alignment.
- Utility-card body spacing is 12px, timeline row rhythm is 44px, and timeline nodes are consistently 28/16px.

## Recommended actions

1. **[P1] `/harden`** — make timeline summary and detail render from the same event model.
2. **[P2] `/clarify`** — restore an expansion affordance or remove expansion.
3. **[P2] `/adapt`** — enlarge citation/inline-action hit areas without making their visuals oversized.
4. **[P2] `/distill`** — consolidate page-local overrides into shared component variants and tokens.
5. **[P2] `/optimize`** — lazy-mount routes and heavy tab content in the production implementation.
6. **[P3] `/polish`** — re-check spacing, focus, open states and responsive screenshots after fixes.

You can ask me to run these one at a time, all at once, or in any order you prefer.

Re-run `/audit` after fixes to see the score improve.
