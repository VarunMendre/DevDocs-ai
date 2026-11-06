---
name: Precision Engineering
colors:
  surface: '#10131a'
  surface-dim: '#10131a'
  surface-bright: '#363940'
  surface-container-lowest: '#0b0e14'
  surface-container-low: '#191c22'
  surface-container: '#1d2026'
  surface-container-high: '#272a31'
  surface-container-highest: '#32353c'
  on-surface: '#e1e2eb'
  on-surface-variant: '#c7c4d7'
  inverse-surface: '#e1e2eb'
  inverse-on-surface: '#2e3037'
  outline: '#908fa0'
  outline-variant: '#464554'
  surface-tint: '#c0c1ff'
  primary: '#c0c1ff'
  on-primary: '#1000a9'
  primary-container: '#8083ff'
  on-primary-container: '#0d0096'
  inverse-primary: '#494bd6'
  secondary: '#93ccff'
  on-secondary: '#003351'
  secondary-container: '#3198dc'
  on-secondary-container: '#002c47'
  tertiary: '#4edea3'
  on-tertiary: '#003824'
  tertiary-container: '#00885d'
  on-tertiary-container: '#000703'
  error: '#ffb4ab'
  on-error: '#690005'
  error-container: '#93000a'
  on-error-container: '#ffdad6'
  primary-fixed: '#e1e0ff'
  primary-fixed-dim: '#c0c1ff'
  on-primary-fixed: '#07006c'
  on-primary-fixed-variant: '#2f2ebe'
  secondary-fixed: '#cce5ff'
  secondary-fixed-dim: '#93ccff'
  on-secondary-fixed: '#001d31'
  on-secondary-fixed-variant: '#004b73'
  tertiary-fixed: '#6ffbbe'
  tertiary-fixed-dim: '#4edea3'
  on-tertiary-fixed: '#002113'
  on-tertiary-fixed-variant: '#005236'
  background: '#10131a'
  on-background: '#e1e2eb'
  surface-variant: '#32353c'
typography:
  headline-xl:
    fontFamily: Geist
    fontSize: 36px
    fontWeight: '600'
    lineHeight: 44px
    letterSpacing: -0.025em
  headline-xl-mobile:
    fontFamily: Geist
    fontSize: 28px
    fontWeight: '600'
    lineHeight: 36px
    letterSpacing: -0.02em
  headline-lg:
    fontFamily: Geist
    fontSize: 30px
    fontWeight: '600'
    lineHeight: 38px
    letterSpacing: -0.02em
  headline-lg-mobile:
    fontFamily: Geist
    fontSize: 24px
    fontWeight: '600'
    lineHeight: 32px
    letterSpacing: -0.015em
  headline-md:
    fontFamily: Geist
    fontSize: 24px
    fontWeight: '600'
    lineHeight: 32px
    letterSpacing: -0.015em
  title-section:
    fontFamily: Geist
    fontSize: 16px
    fontWeight: '500'
    lineHeight: 24px
    letterSpacing: 0.05em
  title-card:
    fontFamily: Geist
    fontSize: 16px
    fontWeight: '600'
    lineHeight: 24px
    letterSpacing: -0.01em
  body-default:
    fontFamily: Geist
    fontSize: 14px
    fontWeight: '400'
    lineHeight: 22px
    letterSpacing: 0em
  body-strong:
    fontFamily: Geist
    fontSize: 14px
    fontWeight: '500'
    lineHeight: 22px
    letterSpacing: 0em
  metadata:
    fontFamily: Geist
    fontSize: 12px
    fontWeight: '400'
    lineHeight: 16px
    letterSpacing: 0.01em
  code-default:
    fontFamily: JetBrains Mono
    fontSize: 13px
    fontWeight: '400'
    lineHeight: 20px
    letterSpacing: -0.01em
  code-inline:
    fontFamily: JetBrains Mono
    fontSize: 12px
    fontWeight: '500'
    lineHeight: 16px
    letterSpacing: 0em
rounded:
  sm: 0.125rem
  DEFAULT: 0.25rem
  md: 0.375rem
  lg: 0.5rem
  xl: 0.75rem
  full: 9999px
spacing:
  gutter: 1.5rem
  gutter-lg: 2rem
  margin: 1.5rem
  margin-lg: 2.5rem
  space-xs: 0.25rem
  space-sm: 0.5rem
  space-md: 1rem
  space-lg: 1.5rem
  space-xl: 2rem
---

## Brand & Style

This design system embodies the ethos of precision engineering: deterministic, unyielding clarity paired with refined digital craftsmanship. Tailored for engineers, software architects, and technical leaders interacting with dense context graphs, ASTs, and automated documentation intelligence, the interface avoids decorative excess in favor of functional density and low cognitive friction.

Drawing aesthetic principles from modern developer-first environments (Linear, Vercel, Raycast), the style combines:
- **Structural Minimalism**: Deliberate negative space, sharp content hierarchies, and elimination of non-functional ornament.
- **Architectural Framing**: Subtle, translucent hairline borders (`1px`) defining spatial containers over pure atmospheric blur.
- **Micro-tactility**: Interactive states rely on subtle luminous depth—fine inner top highlights on interactive elements, precise outline states, and snappy transition timings (100ms–150ms cubic bezier).

The interface communicates stability, high throughput, and executive technical polish.

## Colors

The color architecture is built natively for low-light developer workflows, strictly preventing washed-out muddy tones by rooting the base palette in deep obsidian-slate neutrals:

### Background Surfaces
- **Canvas Base (`surface-base`)**: `#0B0E14` (absolute viewport substrate)
- **Sub-canvas / Grouping (`surface-subtle`)**: `#101520` (panels, sidebars, tool drawers)
- **Elevated Container (`surface-card`)**: `#161D2B` (cards, floating sheets, inspector modals)
- **Interactive Elevated (`surface-overlay`)**: `#1E283D` (dropdown menus, popovers, hovering rows)

### Structural Borders & Rims
- **Border Subtle**: `rgba(255, 255, 255, 0.06)` (divider lines, passive card borders)
- **Border Default**: `rgba(255, 255, 255, 0.08)` (interactive inputs, container separations)
- **Border Hover**: `rgba(255, 255, 255, 0.16)` (hovered controls and cards)
- **Focus Ring**: `rgba(99, 102, 241, 0.40)` with `1px solid #6366F1` inner edge

### Brand & Interactive Accents
- **Primary Indigo Accent**: `#6366F1` (interactive CTAs, active tab indicators, primary toggles)
- **Primary Deep / Hover**: `#4F46E5`
- **Secondary Active Sky**: `#0284C7` (active real-time threads, indexing streams, AI reasoning loops)

### Semantic & Feedback Tokens
- **Success / Verified**: `#10B981` (emerald border/accent), `#059669` (hover), `rgba(16, 185, 129, 0.12)` (fill wash)
- **Warning / Degraded**: `#F59E0B` (amber border/accent), `#D97706` (hover), `rgba(245, 158, 11, 0.12)` (fill wash)
- **Critical / Error**: `#EF4444` (crimson border/accent), `#DC2626` (hover), `rgba(239, 68, 68, 0.12)` (fill wash)

### Typography Color Tokens
- **Text Heading**: `#F8FAFC` (slate-50 equivalent, maximum legible contrast)
- **Text Body**: `#94A3B8` (slate-400 equivalent, comfortable low-fatigue reading)
- **Text Metadata / Muted**: `#64748B` (slate-500, timestamps, non-interactive annotations)
- **Code & Syntax Base**: `#E2E8F0` (slate-200, high readability against terminal/editor blocks)

## Typography

The type system pairs **Geist** for crisp proportional hierarchy with **JetBrains Mono** for deterministic layout of technical tokens, paths, git SHAs, and schema definitions.

### Structural Typographic Rules
- **Section Headers (`title-section`)**: Rendered exclusively in uppercase with tracking expanded (`0.05em`) and colored `#64748B` to act as clean architectural dividers without visual shouting.
- **Card Titles (`title-card`)**: Paired with `#F8FAFC` to anchor contextual groupings immediately.
- **Code Tokens**: Monospace is reserved strictly for functional technical values: filepaths (`src/kernel/indexer.ts`), commit hashes (`8a4f91c`), HTTP verbs, and AST node names. In-line snippets employ a background wash of `rgba(255, 255, 255, 0.05)` with `px-1.5 py-0.5` rounding to clearly isolate logic from prose.
- **Leading Calibration**: Long-form documentation prose enforces `lineHeight: 22px` on `14px` text to maintain optimum optical tracking across multi-column layouts.

## Layout & Spacing

Layout geometry follows an uncrowded grid model prioritizing rapid scanning, data-density balance, and zero overlap.

### Grid & Breakpoints
- **Desktop (≥1280px)**: 12-column layout with standard `gutter-lg` (32px / `2rem`) and `margin-lg` (40px / `2.5rem`). Two-column splits (e.g., Code Spec vs. Output Playground) use a 7:5 ratio; three-column views use an equal 4:4:4 split.
- **Tablet (768px – 1279px)**: 8-column layout with `gutter` (24px / `1.5rem`) and `margin` (24px / `1.5rem`). Technical inspectors collapse into collapsible slide-over sheets.
- **Mobile (<768px)**: 4-column fluid layout with `16px` gutters and `16px` margins. Side-by-side grids reflow vertically into single-column card stacks.

### Sizing and Padding Rules
- **Interactive Form Inputs**: Minimum height fixed at `44px` with horizontal padding of `16px` (`space-md`) and vertical padding of `10px` (`0.625rem`).
- **Interactive Buttons**: Minimum height fixed at `40px` (`h-10`) with `px-4` minimum.
- **Card Containers**: Component interior padding defaults strictly to `24px` (`space-lg`) on primary panels and `16px` (`space-md`) on nested metadata drawers.

## Elevation & Depth

Visual separation relies on luminance stepping and hairline borders rather than heavy atmospheric dropshadows.

### Surface Tiers
- **Tier 0 (Base Canvas)**: `#0B0E14` (completely un-elevated background).
- **Tier 1 (Panels & Sidebar)**: `#101520` bordered by `1px solid rgba(255, 255, 255, 0.06)`.
- **Tier 2 (Cards & Active Modules)**: `#161D2B` bordered by `1px solid rgba(255, 255, 255, 0.08)`. Shadow: `0 1px 2px 0 rgba(0, 0, 0, 0.35)`.
- **Tier 3 (Floating Menus & Command Palette)**: `#1E283D` bordered by `1px solid rgba(255, 255, 255, 0.12)`. Shadow: `0 10px 25px -5px rgba(0, 0, 0, 0.6), 0 8px 10px -6px rgba(0, 0, 0, 0.5)`.

### Border Highlights (The Linear/Raycast Bevel)
Interactive cards and primary buttons utilize an internal pseudo-border or inset shadow:
`box-shadow: inset 0 1px 0 0 rgba(255, 255, 255, 0.12)`
This creates a crisp tactile ridge on the top edge, catching imaginary light and instantly separating primary actions from passive dark canvas.

## Shapes

The design system enforces a disciplined **Soft** (`1`) geometric radius scale to project precision and structural discipline.

- **Base Radius (`0.25rem` / `4px`)**: Badges, status tags, inline code chips, checkboxes, scrollbars.
- **Medium Radius (`0.375rem` / `6px`)**: Buttons, text inputs, segmented control segments, dropdown items.
- **Large Radius (`0.5rem` / `8px`)**: Container cards, code block containers, command menus (`rounded-lg`).
- **Extra Large Radius (`0.75rem` / `12px`)**: Application-level modals and overlay sheets (`rounded-xl`).

Circular pill shapes are strictly prohibited except for binary status dot indicators (`w-2 h-2 rounded-full`).

## Components

### Buttons
- **Primary**: Solid `#4F46E5` background, high-contrast `#FFFFFF` text, `h-10 px-4`, `rounded-[6px]`, font weight `500`. Features an inset top highlight `box-shadow: inset 0 1px 0 0 rgba(255, 255, 255, 0.25)`. Hover transition shifts to `#6366F1`.
- **Secondary / Outline**: `#161D2B` background, border `1px solid rgba(255, 255, 255, 0.08)`, text `#F8FAFC`. On hover: border transitions to `rgba(255, 255, 255, 0.20)`, background shifts to `#1E283D`.
- **Ghost / Tertiary**: Transparent background, text `#94A3B8`. Hover states apply `rgba(255, 255, 255, 0.04)` fill and `#F8FAFC` text color.

### Form Inputs
- Built with a strict `44px` height (`h-11`), `px-4 py-2.5`, surface `#101520`, border `1px solid rgba(255, 255, 255, 0.08)`, text `#F8FAFC`, placeholder `#64748B`.
- Focused state: Border transitions to `#6366F1` with an exterior focus ring `0 0 0 2px rgba(99, 102, 241, 0.40)`. No layout shift.

### Chips & Semantic Badges
- Compact height (`24px`), `px-2 py-0.5`, `rounded-[4px]`, typography `label_font` (`12px` font weight `500`).
- **Verified / Ready**: Text `#10B981`, background `rgba(16, 185, 129, 0.10)`, border `1px solid rgba(16, 185, 129, 0.25)`.
- **Warning / Stale**: Text `#F59E0B`, background `rgba(245, 158, 11, 0.10)`, border `1px solid rgba(245, 158, 11, 0.25)`.
- **Error / Failed**: Text `#EF4444`, background `rgba(239, 68, 68, 0.10)`, border `1px solid rgba(239, 68, 68, 0.25)`.

### Cards & Container Panels
- Surface `#161D2B`, `1px solid rgba(255, 255, 255, 0.08)`, `rounded-lg` (`8px`), padding `24px`.
- Headers within cards separate body content with an internal hairline divider (`border-b border-white/[0.06] pb-4 mb-4`).
- Interactive card variants gain a subtle hover border `rgba(255, 255, 255, 0.16)` and a cursor pointer.

### Checkboxes & Radios
- Size `16px x 16px`, surface `#101520`, border `1px solid rgba(255, 255, 255, 0.15)`, `rounded-[4px]`.
- Checked state: Background `#6366F1`, border `#6366F1`, displaying a crisp white micro-check icon.

### Developer-Specific: Code Blocks & AST Token Trays
- Surface `#0B0E14`, inset within `#161D2B` cards.
- Border: `1px solid rgba(255, 255, 255, 0.06)`, `rounded-md`.
- Filepath and action bar at top (`h-9 px-3 bg-white/[0.02] border-b border-white/[0.06] text-xs font-mono text-slate-400 flex items-center justify-between`).
- Syntax highlighting uses calibrated contrast: keywords in `#818CF8`, strings in `#34D399`, types in `#38BDF8`.