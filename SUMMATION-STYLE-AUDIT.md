# Summation style extraction

Measured from the rendered public pages at [summation.com](https://www.summation.com/) on 2026-09-12. This is a clean-room specification of observable visual behavior; it does not copy Summation source components, trademarks, artwork, or licensed font files.

## What the site actually uses

- Runtime: a bespoke Framer site. The generated markup exposes Framer components and style presets; no public Material, Bootstrap, Tailwind, Radix, shadcn, or other named component system is exposed.
- Primary marketing family: `STK Bureau Sans Book` at a computed weight of `300`.
- Strong faces declared by the site: `STK Bureau Sans SemiBold`, `STK Bureau Sans Bold`, and `STK Bureau Sans Medium`.
- Additional loaded families: `STK Bureau Sans Light`, `STK Bureau Serif Light`, `Inter`, `Fragment Mono`, and `JetBrains Mono`. Inter appears in the agent-launcher integration; the main marketing hierarchy remains Bureau Sans.
- Font licensing: STK Bureau is proprietary. OpsTracer must supply its own licence and local webfont files before claiming pixel-identical typography. Until then, the implementation uses the already licensed local `Host Grotesk` as the closest fallback while preserving the measured metrics.

## Core tokens

| Role | Measured value | OpsTracer token |
| --- | --- | --- |
| Canvas / surface | `#FFFFFF` | `--bg`, `--surf` |
| Quiet section / card | `#F7F7F7` | `--bg-2` |
| Control fill | `#F4F4F4` | `--surf-2` |
| Hairline | `#EAEAEA` | `--line` |
| Strong hairline | `#D9D9D9` | `--line-2` |
| Primary ink | `#0B0B0B` | `--ink`, `--brand` |
| Secondary ink | `#444444` | `--ink-2` |
| Muted copy | `#696969` | `--muted` |
| Quiet copy | `#808080` | `--muted-2` |
| Disabled copy | `#B6B6B6` | `--muted-3` |
| Inverse | `#FFFFFF` on `#0B0B0B` | `--on-black` |

The source also declares `#969696`, `#EAEAEA`, and 10% black for secondary borders. Colour does not carry the brand. Hierarchy comes from scale, whitespace, black/white inversion, and the difference between `#F4F4F4` and `#F7F7F7`.

## Typography

All headings use Bureau Sans Book, computed weight `300`, tracking `-0.04em`.

| Role | ≥1400 | 1200–1399 | 810–1199 | ≤809 | Line height |
| --- | ---: | ---: | ---: | ---: | ---: |
| Hero / H1 | 58 | 52 | 42 | 34 | 105% |
| Section / H2 | 50 | 48 | 36 | 30 | 100% |
| Lead / H3 | 42 | 36 | 30 | 26 | 110% |
| Editorial / H4 | 30 | 26 | 24 | 20 | 120% |

Supporting roles:

- Card title: 24/27.6, `-0.02em`.
- Large body: 20/24, `-0.01em`; responsive 19, 17, 16.
- Standard body: 18/23.4, `-0.01em`; responsive 17, 16, 15.
- Action: 16/19.2, `-0.01em`; responsive 16, 15, 15.
- Navigation: 14/14, no tracking.
- Metadata: 12/16.92, no tracking.

The visual effect depends on unusually light text. Avoid substituting a 500–600 weight for routine UI labels.

## Layout and spacing

- Desktop page width: 1440px content shell with 48px gutters.
- Desktop navigation: 60px tall, 48px side padding, 16px vertical padding, 24px main gap.
- Hero: 180px top padding, 100px vertical stack gap, 64px between text groups. Measured H1 block is about 598px wide at a 1497px viewport.
- Primary section padding: 120px; large story sections commonly use 150px top and/or bottom.
- Card grid gutter: 16px.
- Panel padding: 30px. Compact content padding: 18–22px.
- Responsive bands: `0–809`, `810–1199`, `1200–1399`, and `1400+`; there are additional shell refinements at 1440 and 1600.

## Geometry, elevation, and motion

- Content card: 12px radius.
- Section/product shell: 16px radius.
- Raised inner card: 20px radius.
- Compact control: 8–12px radius.
- Pills/actions: 70–100px radius; use `100px` as the token.
- Elevation is almost absent. Most surfaces have `box-shadow: none`; the observed media shell uses only `0 0 0 .5px #D9D9D9`.
- Text links transition colour for 200ms with `cubic-bezier(.44, 0, .56, 1)`. Avoid global lift/scale effects.

## Component anatomy observed

Marketing system:

1. 60px global navigation with low-emphasis links and two pill actions.
2. Left-aligned hero with 58px headline, 20px support copy, paired 42px actions, and a large product/tour surface.
3. Quiet grey problem section with editorial copy beside dark media.
4. Three-up capability cards on `#F4F4F4`, 12px radius.
5. Large `#F7F7F7` platform shell, 16px radius, containing a 20px white detail card.
6. Use-case links arranged as repeated title/body/action rows.
7. Integration list, trust/security panel, black closing CTA, and black footer.
8. Agent handoff launcher: destination actions plus a copy-prompt fallback.

Product-tour system:

1. Sidebar/global product navigation.
2. View tabs for Home, Report, Deck, Dashboard, Workflow, and Application.
3. Prompt composer and agent action menu.
4. Resource/project rows, cards, metadata definition lists, run history, and workflow scheduling.
5. Report/document shell, chart surfaces, and dashboard tiles.

## OpsTracer coverage after extraction

Existing components retained: buttons, icon buttons, links, cards, state chips, timeline, evidence rows, evidence timeline, metrics, page headers, side navigation, app shell, and on-call workspace.

Added in this pass:

- Agent handoff launcher.
- Complete field states and inline validation.
- Semantic, horizontally scrollable data table with toolbar and pagination.
- Empty, loading, and recoverable-error states.
- Tabs and disclosure/accordion behavior.
- Overlay treatment and destructive confirmation dialog.

Still product-specific rather than universal: rich report editing, dashboard chart selection, workflow scheduling, and application-builder controls. These should be added only when an OpsTracer route needs the behavior; copying a demo surface without a product requirement would create false system coverage.

## Implementation rule

Use this order when matching the reference: typography licence and weight first, then 48px/120px/150px spatial rhythm, then 12/16/20/100px geometry, then grayscale surfaces. Decorative effects come last and should usually be omitted.
