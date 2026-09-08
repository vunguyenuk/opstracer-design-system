## Design Context

### Users
OpsTracer serves on-call engineers, SRE and platform leads, and engineering leaders who route incidents, preserve evidence, and explain root cause under time pressure.

### Brand Personality
Technical, decisive, and evidence-led. The interface should feel like a precise operational instrument: dense enough to prove capability, calm enough to trust at 3am, and never decorative without meaning. Positioning is on-call plus AI root cause analysis, sold demo-led.

### Aesthetic Direction
Two bands, both on screen at once. A warm paper shell (`#ECEBE5`) carries editorial sections; the SRE Lead dark ramp (`#100E0D` ground, `#F4F1EB` ink) carries the nav, the hero and every product panel — applied as a scoped token override, not a theme flip. Host Grotesk carries prose, navigation, actions, field labels, tabs, status words, and any sentence-case or lowercase UI text. DM Mono is reserved for code and machine-readable facts such as timestamps, IDs, paths, tokens, measurements, and technical keys; its UI-facing text is uppercase with tabular figures and ligatures off, while literal source code preserves syntax casing. Square geometry with one corner notched (14px on actions and cards, 18px on product panels, 24px on the wide case card), and the notch always keeps its outline. Product examples are real panels at real density.

### Design Principles
- Mirror the landing page, resolved in a browser. Its stylesheet is 22 override layers deep — the base `:root` block is superseded and reading it gives the wrong system. When this repo and the rendered page disagree, the page wins.
- One action colour: brand amber `#F0A439` with an ink `#171411` label, on both bands. There is no blue primary.
- Four signal hues, three values each: amber `#F0A43A`, red `#EF6257`, green `#37C993`, blue `#6790FF` — each with a darkened line and a deep ground. Two contextual source hues: logs `#57B1FF`, code `#B78EFF`.
- Colour never carries a state alone; every dot and chip has a word beside it.
- Radius 0. Repaint the notch outline with a gradient iso-line wherever clip-path cuts a border.
- Depth is a border change. Shadow means genuine overlap, nothing else.
- Two type scales: fluid marketing type (68px display, 55/65 section) and five fixed product steps (11 / 13 / 14 / 15 / 18 / 24).
- Every band opens with a rail marker, caps its headline near 20ch, and carries at most one primary action.
- Product primitives now include tab navigation, text and select fields, an operational data table, code samples, and time-series analytics. Their structure is adapted from the non-trading parts of The Grid consumption/dashboard surfaces, while all colour, type, geometry, and state semantics remain OpsTracer-native.
- Trading, procurement, credits, billing, pricing, balances, and market instruments are explicitly outside the system. New UI must serve on-call response, coding workflows, or operational analytics.
- Semantic HTML, a single amber focus ring at 3px offset, tabular figures, and a reduced-motion guard on everything that animates.
- Icons are outline only. One stroked Nucleo family (UI/Core outline), exported locally to `assets/icons/` and `assets/figma-components/`. Pixel-grid families such as Nucleo Arcade are out of the system, and so are filled, duotone and multicolour glyphs, Unicode symbols and emoji. Never mix a second family.
- Icon delivery: resolve SVG CSS variables, normalise visible ink to white with preserved alpha, embed as a data URI, and use alpha mask mode so `file://`, WebKit and `srcdoc` all render. Operational icons are one-colour `currentColor` masks, square corners, on the fixed 14 / 16 / 20 / 24px display scale only. Social glyphs such as GitHub follow the same delivery while keeping their original geometry.
- The masks live in one generated block in `index.html`, between `/* icon-tokens:start */` and `/* icon-tokens:end */`, as `--ico-<name>` tokens with a matching `.icon-<name>` class. Do not hand-edit it: drop SVGs into the two asset folders and run `node tools/build-icons.mjs` (`--check` verifies without writing). The builder refuses pixel-grid artwork, so the outline family cannot regress.
- No decorative marks. The 10px amber cross beside the hero eyebrow and the 5px square before a proof index were pixel shapes and are gone; type, colour and the notch carry the technical voice on their own.
