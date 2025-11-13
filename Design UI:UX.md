# 🎨 School Relocation Planner — **Design‑Only Specification** (5⭐ Aesthetic)

> Scope: **pure design guidance**. No folders, models, code, or implementation details. This document defines the visual language, interaction patterns, motion, states, and page blueprints to deliver the intended product experience.

---

## 1) Design Philosophy
**Neo‑Minimal · Data‑Centric · Accessible · Adaptive**
- **Years‑as‑columns clarity:** All analytical tables show years as columns; first column (metrics) is sticky.
- **Typography‑led:** Numbers are the hero; use a calm, confident hierarchy.
- **Zero clutter:** Flat surfaces, restrained lines, minimal chrome.
- **Fluid performance:** Interactions feel instant; micro‑motions ≤120ms.
- **Governed simplicity:** Roles and statuses are obvious without noise.

---

## 2) Visual Identity

### 2.1 Accent & Brand
- **Primary Accent:** `#0099FF`
- **Signature Gradient:** `linear-gradient(135deg, #0099FF 0%, #00E0FF 100%)`
- **Use:** Primary CTAs, display headings, hero accents.

### 2.2 Color Tokens
- **Semantic:**
  - Success `#10B981` · Warning `#F59E0B` · Critical `#EF4444` · Info `#3B82F6` · Neutral `#6B7280`
- **Light Surfaces:**
  - Base `#FFFFFF` · Mid `#F5F6F8` · Raised `#FFFFFF` · Border `rgba(0,0,0,0.05)` · Text `#111827`
- **Dark Surfaces:**
  - Base `#0E1012` · Mid `#13161A` · Raised `#181B1F` · Border `#1F2428` · Text `#E5E7EB`

### 2.3 Typography
- **Text:** Inter (headings/body/labels)
  - Headings: weight **600**, letter‑spacing **0.2px**, line‑height **1.25**
  - Display headings: weight **700**, **gradient text** (clip text to signature gradient)
- **Numeric:** JetBrains Mono (figures/tables)
  - Weight **500**, letter‑spacing **‑0.15px** for tight numerics

### 2.4 Spacing & Radius (key points)
- Spacing scale uses 4px multiples; common blocks: 16/24/32px.
- Radius: **12px** (xl) for cards; **16px** (2xl) optional for hero/overlays.

### 2.5 Elevation & Shadows
- Elevation 1: subtle separators `0 1px 2px rgba(0,0,0,0.05)`
- Elevation 2: standard panels `0 4px 6px rgba(0,0,0,0.08)`
- Elevation 3: overlays `0 10px 15px rgba(0,0,0,0.12)`
- **Dark mode:** amplify strength (~1.3×) for readability.

### 2.6 Glass Morphism (sparingly)
- Only for top app bar or empty‑state callouts.
- 5–8% transparency, soft backdrop blur.

---

## 3) Interaction & Motion

### 3.1 Timing & Easing
- **Default transitions:** 120ms, cubic‑bezier **(0.4, 0, 0.2, 1)**
- **Spring interactions:** stiffness **250**, damping **15** (buttons, small cards)

### 3.2 Micro‑Delights
- **Button tap:** scale **0.97**
- **Button release:** scale **1.02** with spring
- **Save pulse (on success):** scale **1 → 1.05 → 1**
- **Ready glow (status=Ready):** subtle pulsing shadow (emerald) at **2s** loop
- **Confetti:** reserved for milestone events only (optional)

### 3.3 Reduced Motion
- Respect `prefers-reduced-motion`: disable non‑essential animations; retain instant state changes.

### 3.4 Focus & Keyboard
- Visible focus ring using **accent focus** (`#66D9FF` feel) on all interactive elements.
- Keyboard shortcuts (discoverable in a help sheet):
  - **Cmd/Ctrl+K** Command palette
  - **Cmd/Ctrl+N** New version
  - **Cmd/Ctrl+S** Save
  - **Cmd/Ctrl+/** Shortcuts help
  - **Esc** Close modal/sheet

---

## 4) Roles & Statuses

### 4.1 Roles (visual distinction)
- **Admin:** blue‑tinted badge accent
- **Planner:** green‑tinted badge accent
- **Viewer:** neutral gray badge accent
- Role badge sits near the user avatar/menu and appears in governance contexts.

### 4.2 Status Indicators
- **Draft:** Gray badge
- **Ready:** Green badge with **pulsing glow** (gentle, not neon)
- **Locked:** Blue badge
- Status badges must be legible at a glance and consistent across lists, headers, and detail views.

---

## 5) Layout System

### 5.1 App Bar
- Persistent across pages; minimal chrome.
- Left: brand mark; Center/Left: global nav; Right: shortcuts hint, role badge, theme toggle.
- May use subtle glass effect; never on dense data surfaces.

### 5.2 Breadcrumbs
- Always visible under the app bar.
- Clickable path; last crumb marked with `aria-current="page"`.

### 5.3 Main Container
- Max width **1400px**, centered.
- Standard page padding: mobile **16px**, desktop **24px**.
- Grid: 12‑column mental model; avoid nested heavy grids.

---

## 6) Core Components (Design Behavior)

### 6.1 Display Heading
- Large, gradient‑filled text for hero sections; do **not** use gradient on regular H2/H3.

### 6.2 Primary Button
- Filled with **signature gradient**; rounded (xl); motion on tap/release; clear focus ring.
- Secondary: flat border; hover surface lift.
- Ghost: minimal hover surface.

### 6.3 Card/Panel (Flat)
- Raised surface on light/dark backgrounds; elevation 2; 16–24px padding.
- Borders are subtle; corners **12px**.

### 6.4 Status Badge
- Color per status; Ready includes soft glow loop.

### 6.5 Tables (Years‑as‑Columns)
- First column sticky; hover highlights row; compact numeric cells in mono.
- Header background subtly tinted (light/dark variant).
- Scroll horizontally for long ranges; vertical density prefers clarity over squeeze.

### 6.6 Sparklines
- Minimal inline charts for trend quick‑looks (no axes); use primary blue stroke; width ~120px, height ~32px.

---

## 7) Page Blueprints (Design‑Only)

### 7.1 Overview
- **Hero**: display heading with gradient; supporting one‑line value proposition.
- **Quick actions**: primary (gradient), secondary, ghost.
- **KPIs**: four small cards (Revenue, EBITDA, Rent Load, Cash). Mono numbers; optional delta line.
- **Recent Versions**: flat table with Owner, Status (badge), EBITDA%, Cash, quick actions.
- **Mini‑Compare**: three blocks each with title, status badge, **sparkline**, and 4 compact mono stats.

### 7.2 Versions List
- **Filter strip**: Status, Curriculum, Date Range, +New control.
- **Table**: Version Name | Status | Modified | Owner | Actions.
- **Loading**: shimmer skeleton rows.
- **Empty state**: upbeat blurb and call‑to‑action.

### 7.3 Version Detail (Header + Tabs)
- **Header**: version title + status badge + governance actions (Request Edit, Lock, Approve).
- **Tabs**: Overview · Curriculum · Costs Analysis · Capex · Opex · Tuition Sim · Reports.

**Overview tab**
- **P&L summary table** with pivot years (2024, 2025, 2028, 2038, 2048, 2052).
- **Validation & Governance card**: Critical=0, Warnings count, Override flag; links to matrix/report.

**Curriculum tab**
- FR and IB lines grouped; rows for Students and Tuition (SAR); years as columns.

**Costs Analysis tab**
- Rows for Rent Load %, Staff % Revenue, Opex % Revenue; link to “Rent Lens”.

**Capex tab**
- Timeline cue (no heavy visuals): note cycles (e.g., Building 20y, FF&E 7y, IT 4y); control to open configuration.

**Opex tab**
- Percentage allocations with validation rule: **must sum to 100%**.

**Tuition Sim tab**
- **3‑panel layout**: Rent Config | Impact Charts | Tuition Controls.
- Real‑time feedback feel; charts animate in ≤120ms.

**Reports tab**
- Export cue; reiterate **pivot‑years tables** + **full‑period charts**.

### 7.4 Compare
- Select 2–3 versions and show **side‑by‑side** metrics: Revenue, Rent %, EBITDA %.
- Δ vs baseline column with color‑coded signals (green positive, red negative, amber mixed).

### 7.5 Admin (Design surface)
- **Financial Settings**: DSO, DPO, Deferred %, Discount rate.
- **User Management**: Lean table (Name, Email, Role); avoid dense chrome.

---

## 8) States & Feedback

### 8.1 Loading
- **Shimmer skeletons** that mimic the final structure (tables/cards). Fade in/out ≤120ms.

### 8.2 Empty States
- Friendly title, one‑line explanation, and a single primary action.

### 8.3 Errors
- Inline red message near the source; toast may summarize; include a visible retry affordance.

### 8.4 Success
- Subtle toast with check icon; auto‑dismiss ~3s; optional **save pulse** on the source control.

---

## 9) Dark Mode
- Detect system preference; allow manual toggle; persist choice.
- Apply **dark surfaces** and stronger shadows for elevation.
- Ensure all contrasts meet **WCAG AA**.
- Charts: adjust stroke/fill to remain legible on dark surfaces.

---

## 10) Accessibility (WCAG 2.1 AA)
- **Text contrast:** ≥4.5:1; large text ≥3:1; interactive ≥3:1.
- **Focus:** clear rings on all interactive elements.
- **Semantic structure:** headings, landmark roles, descriptive labels.
- **Screen readers:** ARIA for status badges, toasts, validation errors.
- **Reduced motion:** honor preference across the system.

---

## 11) Data Visualization Guidance
- **Sparklines (Overview):** quick trend; no axes; consistent stroke width.
- **Trend lines:** smooth animation ≤120ms; avoid overshooting.
- **Bars/Areas:** restrained fills; avoid gradients except where used as branded accent in hero contexts.
- **Color semantics:** Blue primary; Green positive; Amber warning; Red error.

---

## 12) Interaction Patterns
- **Inline editing**: click → edit → autosave on blur; show compact validation if invalid.
- **Optimistic updates**: reflect change immediately; reconcile silently.
- **Undo**: lightweight toast‑based undo where safe.
- **Command palette**: task navigation; recent items; keyboard first.

---

## 13) Responsive Rules
- **Navigation:** collapses to icon/hamburger on small screens.
- **Tables:** horizontal scroll with sticky first column and shadow fade at edges.
- **Charts:** touch‑friendly; simplify markers/legends on mobile.
- **Forms:** full‑width inputs on small screens; maintain 16px tap targets.

---

## 14) Do / Don’t Checklist
**Do**
- Use gradient only for **display headings** and **primary CTAs**.
- Keep surfaces flat and calm; let **numbers** stand out.
- Keep micro‑motions short and respectful of reduced‑motion.
- Make roles and statuses visible at a glance.

**Don’t**
- Don’t apply heavy gradients to cards or tables.
- Don’t over‑shadow; elevation should be just enough for separation.
- Don’t hide focus rings.
- Don’t animate critical content constantly; use motion purposefully.

---

## 15) Acceptance Criteria (Design)
- **Hierarchy**: At first glance, display heading + KPIs are the visual anchors.
- **Tables**: Years as columns; sticky metric column; mono numerics; hover row highlight.
- **Status**: Draft/Ready/Locked badges identical across list and detail; Ready shows gentle glow.
- **Motion**: Button tap 0.97; release 1.02 spring; general transitions ≤120ms.
- **Accessibility**: All interactive elements have focus rings and pass contrast checks.
- **Dark Mode**: Parity with light mode; no loss of legibility.
- **Performance feel**: Interactions appear instantaneous; no jank during table scroll.

---

## 16) Glossary (Design)
- **Display Heading**: Large gradient text used only for hero/title contexts.
- **Surface Base/Mid/Raised**: Layering strategy to create depth without clutter.
- **Pivot Years**: The six fixed years for tables: **2024, 2025, 2028, 2038, 2048, 2052**.
- **Ready Glow**: Pulsing shadow around the Ready badge, 2s loop, low intensity.

---

**End of Design‑Only Specification.**

