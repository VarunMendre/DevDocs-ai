# DESIGN.md — DevDocs AI Design System & Screen Specifications

**System Name:** Technical Precision / Precision Engineering  
**Version:** 2.0.0  
**Target Platform:** Desktop Web Application (1440px baseline grid)  
**Primary Design Philosophy:** *Evidence Before Assertion* — Technical clarity, dense yet breathable information architecture, high contrast, deterministic visual hierarchies, and tactile developer-centric surfaces.

---

## 1. Global Visual Foundations & Token Registry

### 1.1 Color Palette & Semantics

The palette uses deep obsidian surfaces layered with slate elevation containers and high-voltage functional accents.

```yaml
colors:
  # Backgrounds & Surface Elevations
  surface-canvas: "#070A0F"          # Deepest background / application canvas
  surface-base: "#0B0E14"            # Default container base
  surface-card: "#101520"            # Elevated cards, panes, and sidebars
  surface-card-hover: "#151B2B"      # Interactive hover state for cards
  surface-inset: "#080B11"           # Inset input fields, code terminals, code blocks
  surface-overlay: "rgba(11, 14, 20, 0.85)" # Modals, backdrops with backdrop-filter

  # Borders & Dividers
  border-subtle: "#1B2234"           # Structural panel dividers
  border-card: "#212B40"             # Card and section boundaries
  border-interactive: "#334155"      # Interactive elements, active borders
  border-focus: "#6366F1"            # Keyboard focus ring boundary

  # Brand & Primary Accents
  primary-500: "#6366F1"             # Electric Indigo (Primary CTA, key brand moments)
  primary-400: "#818CF8"             # Indigo highlight, active states
  primary-600: "#4F46E5"             # Indigo pressed / active state
  primary-glow: "rgba(99, 102, 241, 0.25)" # Radial glows and focus halos

  # Functional / Status Semantics
  status-ready: "#10B981"            # Emerald (100% Tree-sitter verified, Synced, Healthy)
  status-ready-bg: "rgba(16, 185, 129, 0.10)"
  status-ready-border: "rgba(16, 185, 129, 0.25)"

  status-warning: "#F59E0B"          # Amber (Partially ready, large blobs skipped)
  status-warning-bg: "rgba(245, 158, 11, 0.10)"
  status-warning-border: "rgba(245, 158, 11, 0.25)"

  status-error: "#EF4444"            # Crimson / Coral (Rate limit, parsing failed)
  status-error-bg: "rgba(239, 68, 68, 0.10)"
  status-error-border: "rgba(239, 68, 68, 0.25)"

  status-info: "#38BDF8"             # Cyan / Sky (Telemetry, commit SHAs, AST parsing)
  status-info-bg: "rgba(56, 189, 248, 0.10)"

  # Text & Content Hierarchy
  text-primary: "#F8FAFC"            # Headings, active values, high-contrast labels
  text-secondary: "#94A3B8"          # Descriptive copy, explanations, secondary metadata
  text-tertiary: "#64748B"           # Disabled states, minor timestamps, inactive icons
  text-code: "#CBD5E1"               # Monospace source code and syntax highlights
```

---

### 1.2 Typography & Scale

| Token | Family | Size | Line Height | Weight | Tracking | Usage |
|---|---|---|---|---|---|---|
| `display-lg` | Geist / Inter | 56px | 64px | 700 Bold | -0.03em | Landing page hero header |
| `display-md` | Geist / Inter | 40px | 48px | 600 SemiBold | -0.025em | Section headlines |
| `heading-xl` | Geist / Inter | 32px | 40px | 600 SemiBold | -0.02em | Page title (`Developer Registry`, `Connect Repository Target`) |
| `heading-lg` | Geist / Inter | 24px | 32px | 600 SemiBold | -0.015em | Repo names, primary card headers |
| `heading-md` | Geist / Inter | 18px | 26px | 600 SemiBold | -0.01em | Sub-module headers, architectural features |
| `body-md` | Geist / Inter | 14px | 22px | 400 Regular | 0.0em | Primary content text, card descriptions |
| `body-sm` | Geist / Inter | 13px | 18px | 400 / 500 | +0.005em | Table metadata, helper text, input labels |
| `caption-mono`| JetBrains Mono / Geist Mono | 12px | 16px | 500 Medium | +0.02em | Commit SHAs (`#7b4a2e1`), line ranges, latency (`P99 <142ms`) |
| `kicker` | JetBrains Mono / Geist Mono | 11px | 14px | 600 SemiBold | +0.10em | Uppercase category tags (`// INGESTION WIZARD`, `NODE.JS ENGINE`) |

---

### 1.3 Elevation, Radius & Spacing Grid

- **Base Unit:** 4px grid system (`4px`, `8px`, `12px`, `16px`, `24px`, `32px`, `48px`, `64px`).
- **Corner Radii:**
  - `rounded-xs`: `4px` (Tags, status pills, inline badges).
  - `rounded-sm`: `6px` (Buttons, inputs, dropdown items).
  - `rounded-md`: `10px` (Feature cards, code preview blocks).
  - `rounded-lg`: `14px` (Major modules, auth modal cards, interactive diagrams).
- **Shadows & Ambient Occlusion:**
  - `shadow-subtle`: `0 1px 3px rgba(0, 0, 0, 0.4), 0 0 0 1px rgba(33, 43, 64, 0.5)`
  - `shadow-glow`: `0 0 20px -3px rgba(99, 102, 241, 0.35)`
  - `shadow-inset`: `inset 0 1px 2px rgba(0, 0, 0, 0.6)`

---

## 2. Global Component Standards

### 2.1 Buttons & Interactive Controls
- **Primary Action:**
  - Height: `42px` (desktop standard), `px-5`, `rounded-md`.
  - Fill: Linear gradient from `#6366F1` to `#4F46E5`.
  - Text: `14px`, 600 SemiBold, `#FFFFFF`.
  - Micro-interaction: `hover:brightness-110 active:scale-[0.98] transition-all duration-150`.
- **Secondary / Outline Action:**
  - Height: `42px`, `px-4`, `rounded-md`.
  - Background: `#101520`, border: `1px solid #212B40`.
  - Text: `#94A3B8`, `hover:text-white hover:border-slate-600 hover:bg-[#151B2B]`.
- **Destructive Action:**
  - Background: `rgba(239, 68, 68, 0.08)`, border: `1px solid rgba(239, 68, 68, 0.3)`.
  - Text: `#F87171`, `hover:bg-red-500/20`.

### 2.2 Status Badges & Verifiability Tags
- **Ready / Verified:** Emerald dot with `100% Tree-sitter verified` or `READY (FULL COVERAGE)`.
- **Partially Ready:** Amber indicator with precise coverage ratio (`92.6% parsed • 38 files skipped (>2MB)`).
- **Attention / Rate Limit:** Red badge with countdown reset indicator (`GitHub rate limit • 14m remaining`).

---

## 3. Screen-by-Screen Specifications

### 3.1 High-Converting Landing Page (`/`)
*Canvas Ref:* `DevDocs AI - High-Converting Landing Page`

- **Visual Architecture:** Single-column narrative progression built on high-trust outcomes.
- **Section Breakdown:**
  1. **Sticky Header:** DevDocs AI Logo, minimal nav (`Repositories`, `Documentation`, `API Status`), and GitHub SSO avatar trigger.
  2. **Hero Stage:**
     - Kicker pill: `HYBRID AST v3.4 LIVE • Deterministic Commit Pinning`.
     - Value proposition: *"Explore Any Codebase With Zero Hallucinations."*
     - CTA Cluster: Primary button `Start Free with GitHub` (prominent glow) + Demo pill `https://github.com/fastify/fastify [DEMO]`.
     - Social Proof Ribbon: Dev headshots + `18,400+ developers at teams like Vercel, Supabase, Cloudflare, Linear • 4.9/5 (642 verified reviews)`.
  3. **Interactive Code & Telemetry Terminal:**
     - Live mock terminal with real commit SHA `#7b4a2e1` and query flow (`"Trace how Fastify dispatches incoming requests"`).
     - Split viewport: Evidence Grounded Answer with line citations (`lib/handleRequest.js:14`) alongside syntax-highlighted AST inspector showing lines 12–21.
  4. **Value Proposition Grid (Outcome-Driven Cards):**
     - 4-card grid: `Onboard in Hours, Not Weeks`, `Citations You Can Trust With Your Job`, `Zero-Setup Knowledge Extraction`, `Isolated Enclaves & Zero Data Training`.
  5. **"The Reality Gap" Direct Comparison:**
     - Side-by-side contrast card: *Generic LLMs & Copilot Chat* (red warnings, phantom APIs) vs *DevDocs AI Intelligence* (emerald checkmarks, verified AST, SHA pinning).
  6. **Customer Social Proof:** Authentic testimonial quotes with staff titles and company categories.
  7. **Bottom Re-engagement & Footer:** High-contrast signup block with zero-friction commitment cues (`No credit card required • 10 free public repos • SOC2 Type II`) and security badges.

---

### 3.2 Developer Dashboard (`/dashboard`)
*Canvas Ref:* `DevDocs AI - Dashboard`

- **Layout Structure:** Two-tier layout with persistent Left Navigation Sidebar (`240px`), sticky Top Context Bar, Main Repository Feed (`68%` width), and Telemetry & Observability Rail (`32%` width).
- **Core Components:**
  1. **Registry Header:** Cluster status (`ACTIVE NODE: US-EAST-1`), total count (`4 Active Repositories`), and quota gauge (`4 / 10 Repos • 40%`).
  2. **Filter & Search Toolbar:** Multi-filter pill toggle (`All 4`, `Ready 2`, `Partially Ready 1`, `Attention 1`) + search bar with `ESC` clear keyboard shortcut.
  3. **Repository Inventory Cards:**
     - **Card A (Ready - fastify/fastify):** Shows indexed AST chunks (`1,420`), conversation count (`18`), commit badge `#7b4a2e1`, and direct actions `View Overview` & `Open Chat`.
     - **Card B (Partially Ready - facebook/react):** Shows warning callout banner with skipped file metrics (38 large blobs skipped >2MB) and action `Review Skipped Files`.
     - **Card C (Attention - tailwindlabs/tailwindcss):** Highlights rate limit pause with countdown timer (`14m remaining`), HTTP 403 API quota notice, and `Retry Indexing` action button.
  4. **Right Observability Rail:**
     - Resource Budget Gauge (Token quota progress bar, AST vector storage count `16,710 / 50k`).
     - Vector Retrieval Latency Chart (Sparkline visualizing P99 <142ms latency).
     - Recent Activity Audit Stream with clickable citation verification badges.
     - AST Engine Tip card.

---

### 3.3 Add Repository Wizard (`/repositories/new`)
*Canvas Ref:* `DevDocs AI - Add Repository`

- **Layout Structure:** 3-step progressive onboarding workflow.
- **Workflow Steps:**
  1. **Step Indicator:** `1. TARGET (Validated)` → `2. LIMITS & SAFETY (Passed)` → `3. CONFIRMATION (Awaiting Trigger)`.
  2. **Input Section (Left Column):**
     - Target Repository Remote URL field with protocol validation and sync badge.
     - Fast Search Ecosystem Index (instant autocomplete for public repos with stars and tags).
     - Security Sandboxing & Parser Policies (Max file threshold 2.0MB, AST isolation boundaries, dual vector + BM25 pipeline).
     - Checkbox controls for Git hook triggers, tree blame depth (100 commits), and package lock analysis.
  3. **Pre-Flight Ingestion Analysis (Right Column):**
     - Live preview card showing Stars (`31.4k`), Source Size (`18.4 MB`), and Language Composition bar (TypeScript 68% / JavaScript 32%).
     - Sanitization check tiles: AST Extraction Candidates (`342 files`), Excluded Binaries (`14 files`), Secret Detection Pre-check (`0 Detected • CLEAN`).
     - Estimated Vector Token Density curve graph (~184k tokens).
  4. **Confirmation Footer:**
     - Pinned commit confirmation badge (`7b4a2e1f`).
     - Primary button: `Confirm & Start Indexing` + secondary `Cancel`.

---

### 3.4 Repository Overview (`/repositories/:repositoryId`)
*Canvas Ref:* `DevDocs AI - Repository Overview`

- **Layout Structure:** Technical diagnostic blueprint for an indexed repository snapshot.
- **Header Metadata:** Repo slug (`fastify/fastify`), commit pin `#7b4a2e1`, branch `main`, license badge, sync timestamp, and action buttons (`Start Codebase Chat`, `Refresh Index`, `Export Spec`, `Remove`).
- **Telemetry Counter Row:** 4 structured KPI blocks:
  - `342 Indexed Files` (100% Tree-sitter verified).
  - `1,420 Semantic Chunks` (pgvector 1536 dim, cosine).
  - `0 Secret Findings` (Clean entropy checks).
  - `4 Filtered Binaries` (.png, .wasm omitted).
- **Synthesized Architecture Profile:**
  - High-level architectural pattern classification (`Modular Monolith with Fast-Path Router`).
  - 4 Structural Pillars:
    - *Core Request Pipeline* (linked to `lib/handleRequest.js:28`).
    - *Plugin Architecture (Avvio)* (linked to `lib/decorate.js:14`).
    - *Schema Validation Engine* (linked to `lib/validation.js:82`).
    - *Lifecycle Hooks Subsystem* (7 Stage Hooks, `lib/hooks.js:114`).
- **Right Technical Panels:**
  - Interactive Module Dependencies DAG tree visualization.
  - Semantic Density Meter (`94.4% docstring + typing ratio`).

---

### 3.5 Repository Chat & Verifiable Citations (`/repositories/:repositoryId/chat`)
*Canvas Ref:* `DevDocs AI - Repository Chat`

- **Layout Structure:** Three-pane split workspace:
  - **Pane 1 (Left, 220px):** Context Base & Conversation Thread History (Active and archived sessions).
  - **Pane 2 (Center, Flex 1):** Evidence-grounded conversational thread with streaming responses, markdown synthesis, and clickable citation pill chips.
  - **Pane 3 (Right, 420px):** Pinned Codebase Source Drawer showing live file contents (`lib/pluginOverride.js`) tied to commit `#7b4a2e1`, line range highlights (lines 24–49), and AST caller graph explorer.
- **Query Input Box:** Model selector (`Claude 3.5 Sonnet / Hybrid RRF`), token context budget counter (`1,240 / 8,000`), quick prompt suggestion chips, and keyboard shortcut `⌘ + Return`.

---

### 3.6 Authentication & Account Enclave (`/login`, `/signup`)
*Canvas Ref:* `DevDocs AI - Authentication`

- **Layout Structure:** Clean centered security enclave container against a subtle dark technical grid.
- **Core Elements:**
  - Header: Security protocol status (`ENCLAVE SECURE • PROTOCOL V4.2`, node `US-EAST-1 / AST-ISOLATED`).
  - Centered Modal: DevDocs AI glyph, title *"Sign in to DevDocs AI"*, and explanatory subheader on tenant isolation.
  - OAuth SSO Providers: Full-width `GitHub` and `Google SSO` buttons with subtle border styling.
  - Email & Password Inputs: Form fields with RFC validation tags, password reveal toggle, and "Remember this device" 256-bit AES badge.
  - Primary Action Button: High-contrast `Authenticate Session →`.
  - Trust Verification Footer: Policy card explicitly stating *"Zero cross-tenant data sharing. Repositories and indexing records remain strictly isolated to your enclave."*

---

## 4. Accessibility & Micro-Interaction Guidelines

1. **Contrast Ratios:**
   - Text on cards: Minimum 7.1:1 contrast for body copy (`#F8FAFC` on `#101520`).
   - Secondary text: Minimum 4.5:1 contrast (`#94A3B8` on `#101520`).
2. **Keyboard Navigation:**
   - Universal search triggered via `⌘ + K`.
   - Clear input via `ESC`.
   - Submit prompts via `⌘ + Enter`.
   - Distinct 2px indigo outline focus ring (`focus:ring-2 focus:ring-indigo-500/40 focus:outline-none`).
3. **Motion & Transitions:**
   - Hover transformations capped at `150ms cubic-bezier(0.16, 1, 0.3, 1)`.
   - Interactive cards use subtle border luminance shifts rather than aggressive displacement.
