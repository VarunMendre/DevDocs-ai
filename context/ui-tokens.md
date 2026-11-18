# DevDocs AI UI Tokens

## Purpose and source of truth

This document records the visual values and interaction details present in the supplied UI templates. Use it when translating those templates into the application; do not substitute approximate colors, spacing, radii, type scales, or states.

### Source files and precedence

The inspected references are:

- Landing: [code.html](../UI/devdocs_ai_landing_page/code.html), [screen.png](../UI/devdocs_ai_landing_page/screen.png), and [DESIGN.md](../UI/devdocs_ai_landing_page/DESIGN.md).
- Authentication: [code.html](../UI/devdocs_ai_authentication/code.html) and [screen.png](../UI/devdocs_ai_authentication/screen.png).
- Dashboard: [code.html](../UI/devdocs_ai_dashboard/code.html) and [screen.png](../UI/devdocs_ai_dashboard/screen.png).
- Add repository: [code.html](../UI/devdocs_ai_add_repository/code.html) and [screen.png](../UI/devdocs_ai_add_repository/screen.png).
- Repository overview: [code.html](../UI/devdocs_ai_repository_overview/code.html) and [screen.png](../UI/devdocs_ai_repository_overview/screen.png).
- Additional design references: [UI/DESIGN.md](../UI/DESIGN.md), [design.md_devdocs_ai_complete_system_specification.md](../UI/design.md_devdocs_ai_complete_system_specification.md), [precision_engineering/DESIGN.md](../UI/precision_engineering/DESIGN.md), and [technical_precision/DESIGN.md](../UI/technical_precision/DESIGN.md).

For reproducing a supplied screen, the screen's `code.html` and matching screenshot take precedence over a generic design description. The HTML defines actual structure, styles, responsive behavior, and scripted interactions; screenshots confirm the rendered composition. Design documents are guidance where they agree with the screen, not permission to replace screen-specific values.

### Important implementation fact

The supplied HTML does **not** define CSS custom properties (`--token` declarations). Its colors come from Tailwind theme extensions, Tailwind utility classes, arbitrary values, and inline SVG/CSS literals. Therefore:

1. Values below are transcribed from those sources; they are not claims that `var(--...)` already exists in the templates.
2. The variable names in this document are the recommended semantic names for the application implementation.
3. Implement CSS custom properties from these values, then reference them as `var(--color-...)`, `var(--space-...)`, `var(--radius-...)`, and `var(--font-...)` (or the matching existing design-token API).
4. Do not add new raw hex/RGB values to page/component styles. Add an intentional, sourced semantic token here first.
5. Keep the explicit page variants below. The supplied designs are not currently a single interchangeable palette; flattening them would change the screenshots.

### CSS variable naming contract

When implementing the custom-property layer, use these stable names:

| Token category | Naming form | Example |
|---|---|---|
| Shared palette | `--color-{source-role}` | `--color-primary` |
| Landing palette | `--color-landing-{role}` | `--color-landing-canvas` |
| Authentication palette | `--color-auth-{role}` | `--color-auth-canvas` |
| Overview surface override | `--color-overview-{role}` | `--color-overview-card` |
| Spacing | `--space-{scale-step}` | `--space-4` |
| Typography size | `--font-size-{size-token}` | `--font-size-type-sm` |
| Typography family | `--font-{family-role}` | `--font-mono` |
| Typography weight | `--font-weight-{weight}` | `--font-weight-semibold` |
| Line height | `--line-height-{role}` | `--line-height-relaxed` |
| Letter spacing | `--letter-spacing-{role}` | `--letter-spacing-tight` |
| Radius | `--radius-{scale-step}` | `--radius-xl` |

These are naming conventions for the CSS implementation, not declarations present in the supplied static HTML. Keep the token's mapped value and scope from the tables below.

## Palette and screen variants

### Shared application-shell palette

The Dashboard, Add Repository, and Repository Overview Tailwind theme extensions share the following palette. These values are the baseline for application-shell surfaces and semantic status colors.

| Source token | Value | Intended use |
|---|---|---|
| `surface` | `#10131a` | Default app canvas and header |
| `surface-dim` | `#10131a` | Dim surface |
| `surface-bright` | `#363940` | Brightened surface / hover |
| `surface-container-lowest` | `#0b0e14` | Deep inset, sidebar, chart wells |
| `surface-container-low` | `#191c22` | Header/search and primary panel surfaces |
| `surface-container` | `#1d2026` | Nested panels and selected navigation |
| `surface-container-high` | `#272a31` | Elevated controls and hover surfaces |
| `surface-container-highest` | `#32353c` | Highest neutral container and divider |
| `surface-variant` | `#32353c` | Alternate surface |
| `on-surface` | `#e1e2eb` | Main text and icons |
| `on-surface-variant` | `#c7c4d7` | Secondary text |
| `on-background` | `#e1e2eb` | Text on the app background |
| `background` | `#10131a` | App background |
| `outline` | `#908fa0` | Muted labels, placeholders, and icons |
| `outline-variant` | `#464554` | Alternative outline |
| `primary` | `#c0c1ff` | Primary app accent, focus, selected items |
| `on-primary` | `#1000a9` | Text/icon on primary |
| `primary-container` | `#8083ff` | Primary container |
| `on-primary-container` | `#0d0096` | Text/icon on primary container |
| `inverse-primary` | `#494bd6` | Inverse primary |
| `surface-tint` | `#c0c1ff` | Surface tint |
| `primary-fixed` | `#e1e0ff` | Fixed primary |
| `primary-fixed-dim` | `#c0c1ff` | Dim fixed primary |
| `on-primary-fixed` | `#07006c` | Text on fixed primary |
| `on-primary-fixed-variant` | `#2f2ebe` | Variant text on fixed primary |
| `secondary` | `#93ccff` | Secondary data series / active state |
| `on-secondary` | `#003351` | Text/icon on secondary |
| `secondary-container` | `#3198dc` | Secondary container |
| `on-secondary-container` | `#002c47` | Text/icon on secondary container |
| `secondary-fixed` | `#cce5ff` | Fixed secondary |
| `secondary-fixed-dim` | `#93ccff` | Dim fixed secondary |
| `on-secondary-fixed` | `#001d31` | Text on fixed secondary |
| `on-secondary-fixed-variant` | `#004b73` | Variant text on fixed secondary |
| `tertiary` | `#4edea3` | Ready, healthy, verified, success |
| `on-tertiary` | `#003824` | Text/icon on tertiary |
| `tertiary-container` | `#00885d` | Tertiary container |
| `on-tertiary-container` | `#000703` | Text/icon on tertiary container |
| `tertiary-fixed` | `#6ffbbe` | Fixed tertiary |
| `tertiary-fixed-dim` | `#4edea3` | Dim fixed tertiary |
| `on-tertiary-fixed` | `#002113` | Text on fixed tertiary |
| `on-tertiary-fixed-variant` | `#005236` | Variant text on fixed tertiary |
| `error` | `#ffb4ab` | Attention and error text/icon |
| `on-error` | `#690005` | Text/icon on error |
| `error-container` | `#93000a` | Error container |
| `on-error-container` | `#ffdad6` | Text/icon on error container |
| `inverse-surface` | `#e1e2eb` | Inverse surface |
| `inverse-on-surface` | `#2e3037` | Text on inverse surface |

Use the source role names when mapping this palette into CSS variables (for example, `primary` becomes `--color-primary`). Do not treat all elevated containers as interchangeable: use the screen's specific container level.

### Landing-page palette

The landing page intentionally uses a near-black, neutral palette with white CTA buttons and the app's lavender/emerald/amber/coral accents. Keep this variant scoped to the landing page; do not replace the application shell's lavender primary button treatment with the landing page's white CTA.

| Semantic token | Value | Use in template |
|---|---|---|
| `landing-canvas` | `#000000` | Page and code-inspector background |
| `landing-surface` | `#181818` | Navigation, cards, and section bands |
| `landing-surface-raised` | `#1F1F1F` | Nested cards, code rows, and inputs |
| `landing-outline` | `#272727` | Borders and dividers |
| `landing-outline-raised` | `#313131` | Citation and highlighted code borders |
| `landing-text-primary` | `#FFFFFF` | Headings and primary text |
| `landing-text-body` | `#DFE2EF` | Long-form body text |
| `landing-text-secondary` | `#C7C4D7` | Supporting copy |
| `landing-text-muted` | `#908FA0` | Metadata and quiet labels |
| `landing-primary` | `#C0C1FF` | Lavender accent, citations, and focus |
| `landing-primary-strong` | `#6366F1` | Indigo hover/focus border |
| `landing-success` | `#4EDEA3` | Verified and operational states |
| `landing-warning` | `#FFB95F` | Warning and highlighted features |
| `landing-error` | `#FFB4AB` | Error and comparison state |
| `landing-cta-background` | `#FFFFFF` | Landing-page CTA |
| `landing-cta-foreground` | `#000000` | Text/icon on landing CTA |
| `landing-heading-gradient-start` | `#FFFFFF` | Hero title gradient start |
| `landing-heading-gradient-end` | `#9B9B9B` | Hero title gradient end |
| `landing-reveal-muted` | `#DFE2EF` | Inactive scroll-reveal words at 30% opacity |
| `landing-selection-background` | `#313131` | Selected text background |

The landing page also uses alpha variants of its semantic colors: primary focus/hover, emerald and coral/amber glows, white overlays, and translucent neutral surfaces. Preserve the source alpha and usage when implementing the corresponding effect; do not approximate them with a different opacity.

### Authentication palette

Authentication is a separate dark enclave composition. Its template uses Tailwind Slate, Indigo, Emerald, Teal, and error utilities, with semi-transparent variants. Keep these values scoped to authentication unless the rendered screen is intentionally redesigned.

| Semantic token | Value | Source usage |
|---|---|---|
| `auth-canvas` | `#0b0e14` | Body background |
| `auth-grid-line` | `rgba(255,255,255,0.035)` | 36px technical background grid |
| `auth-card` | `rgba(15,23,42,0.8)` | Slate-900/80 card |
| `auth-card-border` | `rgba(30,41,59,0.9)` | Slate-800/90 card border |
| `auth-input` | `rgba(2,6,23,0.7)` | Slate-950/70 input |
| `auth-text-primary` | `#f1f5f9` | Slate-100 |
| `auth-text-body` | `#e2e8f0` | Slate-200 |
| `auth-text-label` | `#cbd5e1` | Slate-300 |
| `auth-text-muted` | `#94a3b8` | Slate-400 |
| `auth-text-quiet` | `#64748b` | Slate-500 |
| `auth-text-placeholder` | `#475569` | Slate-600 |
| `auth-border-control` | `#334155` | Slate-700 |
| `auth-border-panel` | `#1e293b` | Slate-800 |
| `auth-primary` | `#6366f1` | Indigo-500 focus |
| `auth-primary-strong` | `#4f46e5` | Indigo-600 CTA |
| `auth-primary-hover` | `#818cf8` | Indigo-400 hover |
| `auth-success` | `#34d399` | Emerald-400 |
| `auth-success-strong` | `#059669` | Emerald-600 loading-success state |
| `auth-success-deep` | `#0d9488` | Teal-600 loading-success state |

The Google provider mark uses its own four-color SVG brand mark (`#4285F4`, `#34A853`, `#FBBC05`, `#EA4335`); retain it as provider-brand artwork, not as application semantic colors.

### Additional Tailwind color utilities used by the templates

These are source utility colors, not replacements for the semantic palette above. When migrating a matching page, map each used value to a semantic/page token rather than leaving the Tailwind literal embedded in component code.

| Source utility | Value |
|---|---|
| `slate-100` | `#f1f5f9` |
| `slate-200` | `#e2e8f0` |
| `slate-300` | `#cbd5e1` |
| `slate-400` | `#94a3b8` |
| `slate-500` | `#64748b` |
| `slate-600` | `#475569` |
| `slate-700` | `#334155` |
| `slate-800` | `#1e293b` |
| `slate-900` | `#0f172a` |
| `slate-950` | `#020617` |
| `indigo-300` | `#a5b4fc` |
| `indigo-400` | `#818cf8` |
| `indigo-500` | `#6366f1` |
| `indigo-600` | `#4f46e5` |
| `indigo-700` | `#4338ca` |
| `emerald-300` | `#6ee7b7` |
| `emerald-400` | `#34d399` |
| `emerald-500` | `#10b981` |
| `emerald-600` | `#059669` |
| `teal-600` | `#0d9488` |
| `amber-300` | `#fcd34d` |
| `amber-400` | `#fbbf24` |
| `amber-500` | `#f59e0b` |
| `amber-950` | `#451a03` |
| `yellow-500` | `#eab308` |
| `green-500` | `#22c55e` |
| `red-500` | `#ef4444` |
| `red-600` | `#dc2626` |

### Repository Overview screen-specific surfaces

The Repository Overview uses the shared application semantic palette but overrides several neutral surfaces in its HTML. Keep these overrides for a pixel-faithful reproduction:

| Semantic token | Value | Source use |
|---|---|---|
| `overview-canvas` | `#0e1118` | Main canvas |
| `overview-sidebar` | `#0e1117` | Sidebar |
| `overview-sidebar-card` | `#13161f` | Sidebar status panel |
| `overview-card` | `#141720` | KPI, architecture, modal, and symbol cards |
| `overview-code-well` | `#0a0d13` | Dependency diagram and destructive target |
| `overview-selected-tab` | `#222530` | Selected tab |

The overview also uses explicit white-alpha borders and dividers (`0.04`, `0.05`, `0.06`, `0.08`, `0.10`, `0.14`, `0.16`) on top of these surfaces. Retain the source alpha for each control/state.

### Status colors and alpha fills

Use semantic status roles consistently within a given screen palette:

| State | App-shell color | App-shell wash examples |
|---|---|---|
| Ready / verified / healthy | `tertiary` (`#4edea3`) | `tertiary/10`, `tertiary/20`, `tertiary/30` |
| Partial / warning | Amber utility (`amber-400`, `#fbbf24`) | `amber-400/10`, `/20`, `/30`; dark warning panel `amber-950/20` |
| Error / attention | `error` (`#ffb4ab`) | `error-container/20`, `/30`; `error/20`, `/30`, `/40` |
| Informational / data series | `secondary` (`#93ccff`) | `secondary/10`, `/20`, `/30` |
| Primary / selected / focus | `primary` (`#c0c1ff`) | `primary/5`, `/10`, `/20`, `/30`, `/60` |

The dashboard warning uses Tailwind `amber-400` rather than the older design document's `#F59E0B`; do not substitute one for the other when matching the supplied dashboard.

### Source alpha and overlay modifiers

Retain the alpha channel, not only the hue. The following opacity modifiers appear in the HTML utility classes:

| Role / source utility family | Opacity values present |
|---|---|
| App `primary` | 5%, 10%, 20%, 30%, 50%, 60% |
| App `tertiary` | 5%, 10%, 15%, 20%, 25%, 30%, 70% |
| App `secondary` | 10%, 20%, 30% |
| App `error` | 20%, 30%, 40%, 90% |
| App `error-container` | 20%, 30%, 40% |
| App `surface` | 90% |
| App `surface-container-low` | 70%, 80% |
| App `surface-container-high` | 80% |
| App `surface-container-lowest` | 70%, 80% |
| App `outline` | 40%, 60%, 80%, 90% |
| App `outline-variant` | 30%, 40% |
| App `amber-400` | 10%, 20%, 30% |
| App `amber-500` | 10%, 20%, 30% |
| App `indigo-500` | 20%, 30%, 50% |
| App `indigo-600` | 15%, 20%, 25% |
| App `indigo-400` | 30% |
| App `red-500` | 70% |
| App `green-500` | 70% |
| App `yellow-500` | 70% |
| App `slate-700` | 80% |
| App `slate-800` | 50%, 60%, 70%, 80%, 90% |
| App `slate-900` | 80% |
| App `slate-950` | 40%, 50%, 70% |
| White overlay/border utilities | 3%, 4%, 5%, 6%, 8%, 10% |
| Black overlay utility | 70% |
| App `indigo-600` shadow | 20%, 25% |
| App `emerald-400` ring | 20% |
| App `secondary` ring | 30% |
| App `outline-variant` ring | 40% |
| App `white` ring | 5%, 10% |

Explicit CSS/SVG alpha colors in the HTML include `rgba(255,255,255,0.035)` for the authentication grid; `rgba(192,193,255,0.18)`, `rgba(192,193,255,0.25)`, and `rgba(192,193,255,0.7)` for landing line highlights; `rgba(78,222,163,0.18)`, `rgba(78,222,163,0.25)`, and `rgba(78,222,163,0.7)` for verified line highlights; landing effects at `rgba(78,222,163,0.1)`, `rgba(78,222,163,0.12)`, `rgba(78,222,163,0.15)`, `rgba(99,102,241,0.15)`, `rgba(255,180,171,0.15)`, and `rgba(255,185,95,0.15)`; and white/black shadows at `rgba(255,255,255,0.3)`, `rgba(255,255,255,0.35)`, and `rgba(0,0,0,0.2)`.

## Typography

### Families

| Token | Font stack | Use |
|---|---|---|
| `font-sans` | `Geist, sans-serif` | UI labels, headings, prose, controls |
| `font-mono` | `JetBrains Mono, monospace` | App technical metadata, code, identifiers, shortcuts |
| `font-landing-mono` | `Geist Mono, monospace` | Landing-page code and metadata, as imported by its HTML |
| `font-provider-icon` | Material Symbols Outlined | Interface icons in app screens that import the icon font |

Use JetBrains Mono for app templates that load it; the landing page specifically loads Geist Mono. Do not silently interchange the two.

### Font sizes present in the HTML

The following exact sizes appear in Tailwind type utilities/arbitrary values. `rem` values assume the browser's standard 16px root size.

| Size token | Value |
|---|---:|
| `type-2xs` | 9px |
| `type-xs-compact` | 10px |
| `type-xs` | 11px |
| `type-sm` | 12px |
| `type-sm-plus` | 13px |
| `type-base-sm` | 14px |
| `type-base-plus` | 15px |
| `type-base` | 16px |
| `type-md-plus` | 17px |
| `type-lg` | 18px |
| `type-lg-plus` | 19px |
| `type-xl` | 20px |
| `type-2xl` | 24px |
| `type-3xl` | 30px |
| `type-4xl` | 36px |
| `type-5xl` | 48px |
| `type-6xl` | 60px |

Most app telemetry, badges, and code metadata use 9-13px. App prose is commonly 12-14px, with 14-16px explanatory text. The landing hero uses 36px on small screens and 60px at the `md` breakpoint; its section headings use 30px/36px and 36px where specified. Preserve responsive modifiers in the source.

### Weight, line-height, tracking

- Font weights actually used include `300`, `400`, `500`, `600`, and `700`; use the weight on the corresponding source element, not one global weight.
- Tailwind leading utilities present: `leading-tight` (1.25), `leading-snug` (1.375), `leading-normal` (1.5), and `leading-relaxed` (1.625). The `DESIGN.md` references also define an app body default of 14px/22px and technical labels with expanded tracking.
- Tailwind tracking utilities present: `tracking-tight`, `tracking-wide`, `tracking-wider`, and `tracking-widest`. Their exact source utility must be preserved; use numeric `letter-spacing` from the screen's design token where one is specified, rather than approximating.
- Use tabular numerals for dynamic metrics, timestamps, counters, quotas, line numbers, and other values that update or align in columns.
- Uppercase technical kickers, category labels, and metric labels use monospace and expanded letter spacing; do not apply this treatment to ordinary prose.

The design markdown files describe additional typographic scales, but some values differ from the HTML. Where a screen uses direct Tailwind typography utilities, its HTML is authoritative; do not infer a different font-size/weight from a similarly named token in an older design description.

## Spacing and sizing

### Observed spacing rhythm

The screen HTML uses these Tailwind spacing steps. One spacing unit equals 4px; each token can be used for padding, margin, gap, or positional offsets only where the corresponding screen uses it.

| Token | Value |
|---|---:|
| `space-0-5` | 2px |
| `space-1` | 4px |
| `space-1-5` | 6px |
| `space-2` | 8px |
| `space-2-5` | 10px |
| `space-3` | 12px |
| `space-3-5` | 14px |
| `space-4` | 16px |
| `space-5` | 20px |
| `space-6` | 24px |
| `space-7` | 28px |
| `space-8` | 32px |
| `space-10` | 40px |
| `space-12` | 48px |
| `space-14` | 56px |
| `space-16` | 64px |
| `space-20` | 80px |
| `space-24` | 96px |
| `space-44` | 176px |
| `space-64` | 256px |

`space-44` and `space-64` appear as utility dimensions (for example, input padding and sidebar offset), not as general-purpose gaps. Avoid using them as generic spacing choices.

The design descriptions additionally name `space-2xs=2px`, `space-xs=4px`, `space-sm=8px`, `space-md=12px`, `space-base=16px`, `space-lg=20px`, `space-xl=24px`, `space-2xl=32px`, and `space-3xl=48px`. Treat these as named design-description tokens only; note that their names and roles do not consistently match the HTML's Tailwind scale. Prefer the actual screen's dimensions when matching a template.

### Key layout and control dimensions

These are component/layout dimensions, not interchangeable spacing tokens:

| Element | Source dimensions |
|---|---|
| Dashboard / overview left navigation | 256px (`w-64`) |
| App top bar | 56px (`h-14`); add-repository header is 64px (`h-16`) |
| Dashboard repository cards | 24px internal padding (`p-6`) |
| Add Repository main content | 32px padding, 40px at large breakpoint (`p-8 lg:p-10`) |
| Add Repository columns | 7:5 split, 32px gap (`gap-8`) |
| Overview main content | max width 1440px, 32px horizontal/vertical padding |
| Dashboard main content | max width 1580px, 32px horizontal/vertical padding |
| Authentication card | max width 512px (`max-w-lg`), 32px/44px padding |
| Landing hero headline | max width 680px |
| Landing workbench | max width 1024px (`max-w-5xl`) |
| Standard form control | 44px (`h-11`); repository URL input 48px |
| Standard app action | typically 40px (`h-10`); some actions 38px minimum or 44px |
| Icon actions | 32px or 36px square on app bars; source-specific |
| Sidebar navigation row | 32px vertical padding plus label line height; use source layout rather than a fixed invented row height |
| Landing workbench | minimum 460px on desktop |
| Authentication grid | 36px by 36px |
| Repository-symbol navigation offset | 256px |

Responsive breakpoints follow the Tailwind defaults used by the templates: `sm=640px`, `md=768px`, `lg=1024px`, `xl=1280px`. Preserve the exact source breakpoint modifiers and grid transitions.

## Border radius, borders, and elevation

### Radius values

| Token | Value | Typical source use |
|---|---:|---|
| `radius-sm` | 2px (`0.125rem`) | Tight labels in the design-token descriptions |
| `radius-default` | 4px (`0.25rem`) | Small badges, tags, and source SVG nodes |
| `radius-md` | 6px (`0.375rem`) | Compact controls and inline elements |
| `radius-lg` | 8px (`0.5rem`) | Buttons, inputs, and nested panels |
| `radius-xl` | 12px (`0.75rem`) | App cards, drawers, and auth subpanels |
| `radius-2xl` | 16px (`1rem`) | Main cards and auth modal |
| `radius-full` | 9999px | Landing navigation/status pills, circular dots, and progress tracks |

Radius must follow each template's actual class. A nominal token name in a design markdown file may not correspond to the radius emitted by the HTML's Tailwind class (for example, Tailwind `rounded-sm` is 2px).

### Border and depth rules

- Most app shell borders are 1px; app dividers commonly use white with alpha `0.04`-`0.08`.
- Primary and semantic state borders use the matching role color with the source alpha.
- Inputs gain a primary-colored border/ring on focus without changing layout.
- Cards use low-contrast borders; hover primarily brightens the border/surface instead of moving the component.
- Use shadows only where the template uses them (menus, active controls, CTA, modal, and floating overlays). Do not apply a generic shadow to every surface.
- Preserve `backdrop-blur` on the app header and landing navigation; authentication uses a blurred, gridded ambient backdrop.

The design reference files contain alternative elevation palettes and shadow recipes. They are not additional app-wide defaults when the rendered screen does not use them.

## Screen composition and interaction contract

Tokens do not replace the screen's information architecture or micro-interactions. Reproduce the following screen-specific behavior from the provided templates:

### Landing

- Floating translucent pill navigation remains at the top; CTA is white with black text.
- Hero status pill, centered 36px/60px headline, proof copy, CTA, then a framed two-pane code/evidence workbench.
- Citation hover highlights its corresponding code line. Citation click highlights and scrolls the target line into view, then clears the glow after about 2.4s.
- Terminal prompt types/deletes rotating examples with a blinking cursor; reduced-motion users do not receive the typing animation.
- Sections reveal on scroll; tagline words progressively brighten as the user scrolls.
- FAQ answers expand/collapse with a 350ms eased height/chevron transition. Respect `prefers-reduced-motion`.
- Cards use subtle border/shadow/translation changes, generally 200-500ms depending on the specific section.

### Authentication

- Centered enclave card over a 36px technical grid and subtle indigo/emerald ambient glows.
- Sign In/Register segmented control changes heading, explanatory text, submit label, organization field visibility, and forgot-password visibility.
- Password visibility toggle changes the password field type and eye icon.
- Form submit has a verification/loading state, then a success state, then restores the original button. Use the existing 150ms transition timing.
- Preserve visible focus rings, required email/password inputs, checkable remember-device control, and provider buttons.

### Dashboard

- Fixed top context bar and 256px left navigation; registry content and telemetry rail form an 8:4 column split at `xl`.
- Filter tabs select All/Ready/Partially Ready/Attention; search filters repository cards. Escape clears and blurs an active search.
- Repo cards visually distinguish Ready, Partial, and Attention without changing their common card structure.
- Resource quota and storage bars, latency sparkline, activity list, and AST tip are aligned in the right rail.
- Hover changes card surface/border; primary actions retain their selected-color contrast.

### Add Repository

- Fixed 64px header and 256px navigation; wizard heading/progress tracker above a 7:5 input/analysis layout.
- Repository URL field has clear action and validity badge; ecosystem search suggestions can be hidden with Escape.
- Checkbox rows are full-row targets; toggling should retain a clear selected indicator.
- Diagnostics open as a right-side drawer with backdrop; close button, footer button, and backdrop dismiss it. The panel slides over 300ms.
- Confirm & Start Indexing changes to dispatch/loading, then success; disable interaction while dispatching.
- Preserve live metric tiles, language bar, preflight checks, pinned commit, and diagnostic log visual hierarchy.

### Repository Overview

- Fixed 56px top bar and 256px sidebar. Header metadata/actions precede four KPI cards and a three-tab content selector.
- Tab selection swaps the visible Architecture/Pipeline/Symbol content and updates the active segment styling.
- Symbol search and type filters work together; changing one does not clear the other.
- Remove action opens a destructive confirmation modal; cancel/close dismisses it, confirmation displays a toast.
- Refresh action animates its icon and reports progress/result through a toast; chat/export actions also report via toast.
- Keep the dependency graph and density meter inside the right rail; use the screen-specific surface overrides above.

Animations and transitions must honor reduced-motion preferences when the source provides that behavior. Preserve hover, active, focus, loading, selected, success, warning, error, and dismissed states; static screenshots alone are not the complete specification.

## Known source conflicts and non-negotiable handling

The supplied UI is not fully tokenized or globally uniform:

1. `UI/DESIGN.md`, `precision_engineering/DESIGN.md`, and `technical_precision/DESIGN.md` describe different neutral/brand palettes and sometimes different spacing, typography, and corner radii.
2. The Landing, Authentication, and app-shell HTML use visibly distinct surface palettes.
3. Repository Overview's neutral surfaces differ from the Dashboard/Add Repository shell even though the same app-level theme colors are present.
4. The design documents describe values such as `#090D16`, `#6366F1`, and `#10B981` as broad system defaults, while the actual shell HTML uses `#10131a`, `#c0c1ff`, and `#4edea3` for the corresponding base/primary/success roles.
5. Several pages use Tailwind palette utilities or inline literal colors in addition to the theme extension.

Until the source screens themselves are revised, do **not** replace a screen's colors with a competing palette from a generic design file. The semantic tables above preserve both the shared app palette and intentional page/screen variants so the rendered references remain reproducible. If the product requires one newly unified palette instead, update the source UI HTML/screens and this document together; do not silently make that design decision in an implementation.

## Agent rules

- Read this document and the relevant screen HTML/screenshot before UI implementation.
- Prefer semantic CSS variables/design-system tokens over literal color values or one-off values.
- Keep values page-scoped only where the source reference explicitly differs.
- Do not add new palette values or introduce one-off spacing/radius/type sizes without source evidence and updating this document.
- Never approximate responsive breakpoints, typography, spacing, borders, radii, or transitions when the reference provides exact values.
- Treat the matching screenshot as a visual check at its reference viewport; also verify the responsive behavior in HTML.
- If the app's final token implementation differs from these references, update the UI source and this document in the same change.
