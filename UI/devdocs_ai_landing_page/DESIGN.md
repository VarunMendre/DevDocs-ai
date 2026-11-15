---
name: Technical Precision
colors:
  surface: '#0f131c'
  surface-dim: '#0f131c'
  surface-bright: '#353943'
  surface-container-lowest: '#0a0e17'
  surface-container-low: '#181b25'
  surface-container: '#1c1f29'
  surface-container-high: '#262a34'
  surface-container-highest: '#31353f'
  on-surface: '#dfe2ef'
  on-surface-variant: '#c7c4d7'
  inverse-surface: '#dfe2ef'
  inverse-on-surface: '#2c303a'
  outline: '#908fa0'
  outline-variant: '#464554'
  surface-tint: '#c0c1ff'
  primary: '#c0c1ff'
  on-primary: '#1000a9'
  primary-container: '#8083ff'
  on-primary-container: '#0d0096'
  inverse-primary: '#494bd6'
  secondary: '#4edea3'
  on-secondary: '#003824'
  secondary-container: '#00a572'
  on-secondary-container: '#00311f'
  tertiary: '#ffb95f'
  on-tertiary: '#472a00'
  tertiary-container: '#ca8100'
  on-tertiary-container: '#3e2400'
  error: '#ffb4ab'
  on-error: '#690005'
  error-container: '#93000a'
  on-error-container: '#ffdad6'
  primary-fixed: '#e1e0ff'
  primary-fixed-dim: '#c0c1ff'
  on-primary-fixed: '#07006c'
  on-primary-fixed-variant: '#2f2ebe'
  secondary-fixed: '#6ffbbe'
  secondary-fixed-dim: '#4edea3'
  on-secondary-fixed: '#002113'
  on-secondary-fixed-variant: '#005236'
  tertiary-fixed: '#ffddb8'
  tertiary-fixed-dim: '#ffb95f'
  on-tertiary-fixed: '#2a1700'
  on-tertiary-fixed-variant: '#653e00'
  background: '#0f131c'
  on-background: '#dfe2ef'
  surface-variant: '#31353f'
typography:
  display:
    fontFamily: Geist
    fontSize: 3.5rem
    fontWeight: '600'
    lineHeight: '1.1'
    letterSpacing: -0.035em
  display-mobile:
    fontFamily: Geist
    fontSize: 2.25rem
    fontWeight: '600'
    lineHeight: '1.2'
    letterSpacing: -0.025em
  headline-xl:
    fontFamily: Geist
    fontSize: 2.25rem
    fontWeight: '600'
    lineHeight: '1.2'
    letterSpacing: -0.025em
  headline-lg:
    fontFamily: Geist
    fontSize: 1.75rem
    fontWeight: '600'
    lineHeight: '1.25'
    letterSpacing: -0.02em
  headline-md:
    fontFamily: Geist
    fontSize: 1.25rem
    fontWeight: '500'
    lineHeight: '1.3'
    letterSpacing: -0.015em
  headline-sm:
    fontFamily: Geist
    fontSize: 1rem
    fontWeight: '500'
    lineHeight: '1.4'
    letterSpacing: -0.01em
  body-lg:
    fontFamily: Geist
    fontSize: 1rem
    fontWeight: '400'
    lineHeight: '1.5'
    letterSpacing: -0.01em
  body-md:
    fontFamily: Geist
    fontSize: 0.875rem
    fontWeight: '400'
    lineHeight: '1.5'
    letterSpacing: -0.005em
  body-sm:
    fontFamily: Geist
    fontSize: 0.75rem
    fontWeight: '400'
    lineHeight: '1.5'
    letterSpacing: 0em
  code-inline:
    fontFamily: JetBrains Mono
    fontSize: 0.8125rem
    fontWeight: '400'
    lineHeight: '1.4'
    letterSpacing: -0.01em
  code-block:
    fontFamily: JetBrains Mono
    fontSize: 0.8125rem
    fontWeight: '400'
    lineHeight: '1.6'
    letterSpacing: 0em
  label-caps:
    fontFamily: JetBrains Mono
    fontSize: 0.6875rem
    fontWeight: '500'
    lineHeight: '1'
    letterSpacing: 0.06em
  label-md:
    fontFamily: Geist
    fontSize: 0.8125rem
    fontWeight: '500'
    lineHeight: '1.2'
    letterSpacing: 0em
  label-sm:
    fontFamily: JetBrains Mono
    fontSize: 0.75rem
    fontWeight: '500'
    lineHeight: '1.2'
    letterSpacing: 0.02em
rounded:
  sm: 0.125rem
  DEFAULT: 0.25rem
  md: 0.375rem
  lg: 0.5rem
  xl: 0.75rem
  full: 9999px
spacing:
  gutter: 1rem
  gutter-desktop: 1.5rem
  margin: 1rem
  margin-desktop: 2rem
  space-2xs: 0.125rem
  space-xs: 0.25rem
  space-sm: 0.5rem
  space-md: 0.75rem
  space-base: 1rem
  space-lg: 1.25rem
  space-xl: 1.5rem
  space-2xl: 2rem
  space-3xl: 3rem
---

## Brand & Style

This design system delivers an exacting, high-performance developer workspace designed for deep codebase intelligence, dependency tracing, and architectural navigation. It speaks directly to software engineers, systems architects, and engineering leadership who require high information density without cognitive clutter.

The aesthetic fuses **Modern Technical Minimalism** with **Developer Instrument Utility**, drawing immediate cues from high-precision toolsets like Linear, Raycast, and GitHub Copilot:
- **Tone:** Authoritative, razor-sharp, frictionless, and computational.
- **Visual Weight:** Deliberate and structured. The interface behaves like a performance cockpit: dense telemetry, crystalline structural borders, razor-sharp typography, and subtle micro-illuminations highlighting semantic state changes.
- **Emotional Response:** Inspires deep focus, surgical command over complex systems, and instantaneous responsiveness.

## Colors

The system is constructed with a dark-first foundation rooted in deep, light-absorbing slates that prioritize syntax contrast and reduce long-session eye strain, paired with an equally disciplined high-contrast light mode.

### Role Tokens & Logic
- **Canvas Base (`#090D16`):** The primary root viewport. Deep obsidian slate providing an infinite-depth backdrop for code blocks and graph nodes.
- **Surface Elevation (`#0F172A` to `#1E293B`):** Structural layers, inspector drawers, and floating command palettes.
- **Primary Accent (`#6366F1`):** Electric Indigo. Represents runtime operations, active query filters, terminal prompts, and focused interactive primitives.
- **Verification Emerald (`#10B981`):** Represents verified AST indexing, cryptographic hash confirmations, clean test runs, and authoritative semantic citations.
- **Warning Amber (`#F59E0B`):** Signifies partial index coverage, uncommitted git diffs, deprecated API usage, and performance bottlenecks.
- **Border Spectrum (`#1E293B` / `#334155` at variable alpha):** Monolithic 1px boundary lines separating panes and tree views without heavy shadow reliance.

## Typography

The typographic architecture balances human-speed prose parsing with machine-level code rendering:
- **Primary Text (`Geist`):** Delivers clean geometry, compact counters, and neutral legibility optimized for data-dense software interfaces.
- **Monospace Elements (`JetBrains Mono`):** Applied to terminal streams, symbols, function tokens, AST paths, keyboard shortcuts, and file diffs. Ensures vertical and horizontal scanning alignment across column boundaries.
- **Tabular Numerics:** All monospace and label instances must employ `font-variant-numeric: tabular-nums` to guarantee jitter-free rendering during rapid indexing updates and metrics counts.

## Layout & Spacing

The layout model adapts between a rigid, dock-oriented multi-pane workstation on desktop and a vertical stacked canvas on mobile:
- **Desktop Architecture (Multi-Pane Workbench):** A three-column shell consisting of a collapsible file/symbol tree (240px–320px), an expansive active exploration/editor center (flexible), and a context/inspector drawer (360px–420px). Pane dividers utilize `gutter` tokens with sub-pixel drag handles.
- **Micro-Scale Spacing Rhythm:** Built upon a compact 4px base increment (`space-2xs` to `space-3xl`). Core components (command pallets, badge chips, node tree items) maintain high-density padding to ensure large volumes of codebase structure remain visible without excessive scrolling.
- **Responsive Adaptations:** Below 1024px, secondary panes fold into floating drawers. Below 768px, layout transitions into a unified single-column stream with top navigation anchors and modal sheets for dependency graphs.

## Elevation & Depth

This system avoids aggressive skeuomorphic drops, relying on **Structural Tonal Layers** complemented by razor-sharp **Ghost Outlines** and focused back-glows:

1. **Layer 0 (Base Canvas):** `#090D16` – Pure matte background.
2. **Layer 1 (Panels & Grouping Containers):** `#0F172A` with a 1px solid border of `rgba(255, 255, 255, 0.07)`.
3. **Layer 2 (Interactive Cards & Code Blocks):** `#141C2E` with a 1px border of `rgba(255, 255, 255, 0.1)`. Hover states raise the border stroke to `rgba(99, 102, 241, 0.4)`.
4. **Layer 3 (Overlays, Flyouts & Command Menus):** `#1E293B` backed by a 12px backdrop blur (`backdrop-filter: blur(12px)`) and a subtle ambient drop shadow: `0 16px 36px -8px rgba(0, 0, 0, 0.65), 0 0 0 1px rgba(255, 255, 255, 0.12)`.
5. **State Illumination:** Active focus is represented by an inner border shift paired with a concentrated 0 0 16px glow using `rgba(99, 102, 241, 0.25)`, directing visual focus without displacing surrounding elements.

## Shapes

The interface embraces a low-radius, architectural profile (`roundedness: 1`):
- **Base Components (Inputs, Buttons, Badges):** 4px (`0.25rem`) border radius. This compact curve preserves visual firmness and maximizes screen efficiency in high-density lists.
- **Cards, Code Panels, Modals:** 8px (`0.5rem`) border radius. Provides soft containment without breaking the predominantly orthogonal, technical grid.
- **Pill Exception:** Pure circular pills (`rounded-full`) are reserved solely for live connection indicators, avatar status rings, and numeric citation tags.

## Components

### Buttons
- **Primary:** Background `#6366F1`, label `#FFFFFF`, 1px solid `rgba(255, 255, 255, 0.15)`. On hover, surface brightens to `#4F46E5`. On active/pressed, scale transitions to `0.98`. Focus ring: 2px offset ring with `#6366F1`.
- **Secondary / Ghost:** Transparent background, 1px solid `rgba(255, 255, 255, 0.1)`, label `#94A3B8`. On hover, background shifts to `rgba(255, 255, 255, 0.05)` and label illuminates to `#F8FAFC`.
- **Icon / Action:** 28x28px or 32x32px square buttons with centered technical icons, 4px corner radius.

### Chips & Status Badges
- **AST / Verified Citation:** Height 20px, font `JetBrains Mono` at `0.6875rem`. Background `rgba(16, 185, 129, 0.1)`, text `#10B981`, border 1px solid `rgba(16, 185, 129, 0.3)`. Left-hand accent includes a 4px solid emerald status dot.
- **Index Warning:** Height 20px, background `rgba(245, 158, 11, 0.1)`, text `#F59E0B`, border 1px solid `rgba(245, 158, 11, 0.3)`.
- **Command / Shortcut Token:** Height 18px, padding `0 4px`, background `#1E293B`, text `#94A3B8`, border 1px solid `rgba(255, 255, 255, 0.08)`.

### Code Blocks & Architecture Cards
- **Code Block:** Surface `#0B0F19`, 1px border `rgba(255, 255, 255, 0.08)`, header bar containing active file path (`JetBrains Mono`, `#94A3B8`) and action cluster (Copy, Open in IDE, Trace AST). Line numbers formatted in tabular monospace with low-contrast slate `#475569`.
- **Surface Cards:** Background `#0F172A`, 1px border `rgba(255, 255, 255, 0.07)`, subtle 8px radius. Content padded using `space-base`. Hovering triggers a border shift to `rgba(99, 102, 241, 0.3)`.

### Inputs & Command Palette
- **Text & Search Fields:** Background `#0B0F19`, border 1px solid `rgba(255, 255, 255, 0.12)`, text `#F8FAFC`, placeholder `#475569`. Focused state draws a direct 1px stroke of `#6366F1` with an outer soft glow.
- **Command Palette (`Cmd+K`):** Suspended modal anchored at 15% from viewport top. Width 640px. Input row has zero outer border with an integrated search prompt icon, leading directly into an auto-suggest list grouped by "Symbols", "Files", "Dependencies", and "Docstrings".

### Tree Views & Lists
- **Symbol & File List:** Compact row height (28px). Hover state applies `rgba(255, 255, 255, 0.03)`. Selected item receives a solid 2px left border accent in `#6366F1` and background `rgba(99, 102, 241, 0.08)`. Indentation strictly spaced at 12px steps with 1px dotted visual guide lines.

### Checkboxes & Radios
- **Selection Controls:** 14x14px bounds with 3px border radius. Inactive state: 1px border `rgba(255, 255, 255, 0.2)`. Active state: `#6366F1` background with white centered check mark.