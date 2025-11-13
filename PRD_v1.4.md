# 🧾 School Relocation Planner — Product Requirements Document (PRD v1.4)

```yaml
meta:
  product: School Relocation Planner
  owner: Faker Helali (CAO)
  version: 1.4
  date: 2025-11-12
  timezone: Asia/Riyadh
  planning_horizon: [2023,2052]
  relocation_year: 2028
  historical_years: [2023,2024]
  transition_years: [2025,2026,2027]
  new_campus_start: 2028
  curricula: [French, IB]
  curriculum_codes: [FR, IB]  # Database codes for curriculum_type
  rent_models: [FixedEscalation, RevenueShare, PartnerModel]
  capex_classes: [Building, FF&E, IT, Other]
  performance_target_ms: 50
  ui_framework: Next.js 16 + Tailwind v4 + shadcn/ui + Framer Motion
  design_language: "Neo-Minimal, Data-Centric, Accessible, Adaptive"
```

## Glossary

- **2024A**: Actual data for year 2024 (locked historical record)
- **FR**: French curriculum code
- **IB**: International Baccalaureate curriculum code
- **FixedEscalation**: Rent model with fixed annual escalation rate
- **RevenueShare**: Rent model based on percentage of revenue
- **PartnerModel**: Rent model based on partner yield calculation
- **FF&E**: Furniture, Fixtures & Equipment
- **BUA**: Built-Up Area (total constructed floor area in square meters)
- **CPI**: Consumer Price Index (inflation measure)
- **EBITDA**: Earnings Before Interest, Taxes, Depreciation, and Amortization
- **NPV**: Net Present Value

---

## 0. Design Philosophy

> *An elegant, visual, and data-intelligent workspace built for strategic decision-making.*

### Core Model Principle
**Rent-Driven Planning:** The financial model is driven by rent assumptions. Rent changes (via different models or parameters) directly impact EBITDA and cash flow. Tuition adjustments are simulated to maintain target financial performance under varying rent scenarios.

### Core Principles
1. **Visual Hierarchy:** emphasize years-as-columns clarity; typography-led data visualization.  
2. **Zero Clutter:** minimalist grid design, no heavy cards or gradients.  
3. **Fluid Performance:** every edit feels instantaneous (<50 ms response).  
4. **Governed Simplicity:** admin vs. planner roles visually distinct (color accents + badges).  
5. **Frictionless Navigation:** breadcrumbs, keyboard shortcuts, semantic focus rings.  

### Visual Style
- **Font System:** Inter (Text) + JetBrains Mono (Figures).  
- **Theme:** Neutral background with subtle glass overlays (5–8% transparency).  
- **Color System:** Semantic tokens (`success`, `warning`, `critical`, `neutral`).  
- **Animations:** 120 ms micro-transitions via Framer Motion.  
- **Accessibility:** WCAG 2.1 AA compliant, high-contrast text on all charts/tables.  

---

## 1. App Layout

```yaml
layout:
  top_nav: [Overview, Versions, Tuition Simulator, Compare, Reports, Admin]
  breadcrumbs: enabled
  theme: light/dark + system auto
  loading_state: shimmer skeleton + Lottie micro-loader
  transitions: 120ms fade-slide
```

### Overview Page (`/`)
- Hero banner: blurred background, logo watermark, quick actions.  
- Status cards: total versions, ready, draft, validation issues.  
- Mini Compare: sparkline charts (Revenue, Rent %, EBITDA, Cash).  
- Quick access: "Create New Version", "Tuition Simulator", "Compare Versions", "View Latest Report".

### Versions Page (`/versions`)
- List view: all versions with status badges, last modified, owner.  
- Filters: status (Draft/Ready/Locked), curriculum, date range.  
- Actions: create, clone, archive, export.  
- Click version → navigate to `/versions/[id]` (version detail/edit page).

**Version Cloning:**
- "Clone Version" creates a new Draft version with all data copied.
- Cloned version includes: curriculum plans, rent plans, capex plans, opex plans.
- Cloned version excludes: audit logs, approval history, tuition simulations.
- User can modify cloned version independently.
- Use case: Create scenarios from baseline version.

### Version Detail Page (`/versions/[id]`)
- Tab navigation: [Overview, Curriculum, Costs Analysis, Capex, Opex, Tuition Sim, Reports]
- **Overview Tab**: Summary metrics, status banner, approval workflow.
- **Curriculum Tab**: Dual-curriculum split view (Section 2).
- **Costs Analysis Tab**: Rent Lens (Section 3), rent model selector, cost breakdown.
- **Capex Tab**: Auto-reinvestment timeline (Section 4).
- **Opex Tab**: Opex structure editor (Section 4).
- **Tuition Sim Tab**: Tuition simulation tool (Section 3).
- **Reports Tab**: Export options (Section 10).

### Compare Page (`/compare`)
- Side-by-side comparison of 2–4 versions.  
- Year-by-year diff view: Revenue, Costs, EBITDA, Cash Flow.  
- Highlight deltas with color coding (green=higher, red=lower).  
- Export comparison report (PDF/XLSX).

### Tuition Simulator Page (`/tuition-simulator`)
**Purpose:** Dedicated page for rent-driven tuition simulation. This is the primary tool for evaluating how tuition increases are needed to maintain target EBITDA under different rent scenarios.

**Workflow:**
1. Select a base version (or create new scenario)
2. Configure rent model and parameters (drives the financial impact)
3. Set target EBITDA (margin % or absolute amount)
4. Simulate tuition increases per curriculum (FR, IB)
5. View impact: Revenue, EBITDA, Cash Flow, Rent Load %
6. Save as new version/scenario

**Key Features:**
- **Rent Model Selector:** Choose FixedEscalation, RevenueShare, or PartnerModel
- **Rent Parameters:** Configure all rent model inputs (see Section 3)
- **Tuition Controls:** Per-curriculum sliders for tuition adjustment (-20% to +50%)
- **Target EBITDA:** Set margin % or absolute target
- **Real-time Impact:** Instant recalculation (<50ms) showing:
  - Year-by-year tuition, revenue, rent, EBITDA
  - Rent Load % (rent as % of revenue)
  - Sensitivity curves: Tuition vs. Rent Load %
- **Scenario Creation:** "Create Scenario" button clones version with adjusted tuition

**Visual Layout:**
- Left Panel: Rent model configuration, target EBITDA input
- Center: Interactive charts (Tuition vs. Rent Load %, Year-by-year impact)
- Right Panel: Tuition adjustment sliders per curriculum, summary metrics
- Bottom: Year-by-year detailed table with all financial metrics

**Use Cases:**
- Evaluate rent model impact on required tuition increases
- Find optimal tuition adjustment to maintain target EBITDA
- Compare different rent scenarios side-by-side
- Create multiple scenarios with different tuition strategies

### Reports Page (`/reports`)
- Report library: all generated reports with filters.  
- Generate new report: select version(s), template, format.  
- Scheduled reports: auto-generate on version approval.  
- Download history with checksums.

### Admin Page (`/admin`)
- Global settings: CPI rates, interest rates, discount rates, inflation indices.  
- Validation rules configuration.  
- User management: roles, permissions.  
- System audit log.  

---

## 2. Dual-Curriculum Model

Each curriculum (French & IB) has independent data for:
- capacity
- tuition
- CPI frequency
- staffing ratios (teacher_ratio, non_teacher_ratio)

**Aggregation Logic:**
- Total Revenue = (French Revenue) + (IB Revenue)
- Total Students = (French Students) + (IB Students)
- Total Capacity = (French Capacity) + (IB Capacity)
- Staff Costs = Sum of (Students × teacher_ratio × teacher_salary_with_CPI) + (Students × non_teacher_ratio × non_teacher_salary_with_CPI) per curriculum
  - Teacher and non-teacher salaries are different and subject to annual CPI adjustments
  - Salaries increase annually: `salary(t) = base_salary × (1 + CPI_rate)^(t - base_year)`
- All financial metrics (EBITDA, Cash Flow, etc.) aggregate across both curricula.

UI pattern: **Split Tabs + Aggregate Totals**

```yaml
component: CurriculumSplitView
layout:
  left_tab: French (FR)
  right_tab: IB
  bottom_section: Aggregated Financial Summary
```

**Visual Cues**
- Flag icons and distinct accent colors.  
- Sticky first column; smooth horizontal scroll.  
- Aggregate totals row highlighted with subtle background.  

---

## 3. Rent Lens & Tuition Simulator

### Rent Models Configuration

**FixedEscalation Model:**
```yaml
inputs:
  - base_rent: initial annual rent amount
  - escalation_rate: annual percentage increase (e.g., 3%)
  - escalation_frequency: years between escalations (default: 1)
calculation:
  - year1: base_rent
  - subsequent: rent(t) = base_rent × (1 + escalation_rate)^(number_of_escalations)
```

**RevenueShare Model:**
```yaml
inputs:
  - revenue_share_pct: percentage of revenue (e.g., 15%)
  - minimum_rent: optional floor amount
  - maximum_rent: optional cap amount
calculation:
  - rent(t) = Revenue(t) × revenue_share_pct
  - Apply min/max constraints if configured
```

**PartnerModel:**
```yaml
inputs:
  - land_size: land area in square meters (sqm)
  - land_price_per_sqm: price per square meter of land
  - bua_size: Built-Up Area (BUA) in square meters (sqm)
  - bua_price_per_sqm: price per square meter of BUA
  - yield_base: initial yield percentage for first year (e.g., 8%)
  - yield_growth_rate: annual yield growth percentage (e.g., 0.5%)
  - growth_frequency: years between yield increases (1, 2, or 3 years)
calculation:
  - capex_base = (land_size × land_price_per_sqm) + (bua_size × bua_price_per_sqm)
  - year1: rent = capex_base × yield_base
  - subsequent: yield(t) = yield_base × (1 + yield_growth_rate)^(floor((t-1)/growth_frequency))
  - rent(t) = capex_base × yield(t)
```

### Rent Lens
**Location:** Inline expandable card in **Costs Analysis** tab (`/versions/[id]` → Costs Analysis tab).

**Functionality:**
- Displays currently selected rent model with all configured inputs.
- Shows NPV calculation (using Admin discount rate).
- Mini sensitivity chart: rent impact on EBITDA across rent model options.
- Quick switch between models with smooth animation.
- "Apply Model" button to save rent model selection to version.

**Visual:**
- Collapsed: summary card showing model name, annual rent range, NPV.
- Expanded: full inputs editor, sensitivity chart, detailed breakdown.

### Tuition Simulation (Tab in Version Detail)
**Location:** Available as a tab in `/versions/[id]` → Tuition Sim Tab, or as standalone page `/tuition-simulator`.

**Purpose:** Evaluate tuition adjustment needed to sustain EBITDA under rent stress. This tool is **rent-driven**—rent model and parameters are configured first, then tuition is adjusted to meet target financial performance.

**Model Flow:**
1. **Rent Drives Impact:** Rent model selection and parameters determine rent costs
2. **Financial Impact:** Rent affects EBITDA and cash flow
3. **Tuition Adjustment:** Tuition is adjusted (per curriculum) to maintain target EBITDA
4. **Scenario Creation:** Save adjusted tuition as new version

```yaml
inputs:
  - base_version: select existing version or start fresh
  - rent_model: [FixedEscalation, RevenueShare, PartnerModel]
  - rent_parameters: model-specific inputs (see Section 3)
  - rent_projection: year→amount (auto-calculated from model)
  - tuition_base: per curriculum (FR, IB) - from base version
  - tuition_adjustment_fr: Δtuition% for French curriculum (slider: -20% to +50%)
  - tuition_adjustment_ib: Δtuition% for IB curriculum (slider: -20% to +50%)
  - target_ebitda: target EBITDA margin % or absolute amount
process:
  - Calculate rent projection from selected model and parameters
  - Calculate current EBITDA with base tuition
  - Adjust tuition per curriculum to meet target EBITDA
  - Render Tuition vs Rent Load% curve
  - Show year-by-year impact: Revenue, Rent, EBITDA, Cash Flow
  - Enable "Save as Scenario" → clone new version with adjusted tuition
```

**UI Components:**
- **Rent Configuration Panel (Left):**
  - Rent model selector (FixedEscalation, RevenueShare, PartnerModel)
  - Model-specific parameter inputs
  - Rent projection preview (year-by-year)
  - NPV calculation display
  
- **Chart Zone (Center):**
  - Interactive tuition curve: Tuition vs. Rent Load % relationship
  - Year-by-year impact chart: Revenue, Rent, EBITDA, Cash Flow
  - Sensitivity analysis: How different tuition adjustments affect financials
  
- **Tuition Controls (Right):**
  - Target EBITDA input (margin % or absolute)
  - Per-curriculum tuition adjustment sliders (FR, IB)
  - Summary metrics: Current vs. Target EBITDA, Rent Load %, Required Tuition Increase
  - Year-by-year detailed table (scrollable)
  
- **Actions:**
  - "Reset to Base" button
  - "Create Scenario" CTA → clones version with tuition adjustments applied
  - "Export Results" → PDF/Excel of simulation

**Key Metrics Displayed:**
- Rent Load % = (Rent / Revenue) × 100
- Required Tuition Increase % to meet target EBITDA
- Year-by-year: Tuition, Revenue, Rent, Staff Costs, Opex, EBITDA, Cash Flow
- Sensitivity: Minimum tuition needed for each rent scenario

**Performance:** Response time < 50 ms for recalculation on any input change (rent parameters or tuition sliders).  

---

## 4. Capex & Opex Model

### Capex Auto-Reinvestment (Admin)

```yaml
classes:
  - Building: cycle=20y
  - FF&E: cycle=7y
  - IT: cycle=4y
  - Other: configurable
inputs:
  - trigger: cycle_years or utilization_threshold
  - inflation_index: Admin.CPI
```

UI:
- Timeline chart with gradient bars for each reinvestment cycle.  
- Editable via modal drawer.  
- Hover = class, cycle, cost.

### Opex (Planner)

```yaml
FR-8.2: Planner.OpexStructure
purpose: Model operating expenses as % of revenue.
inputs:
  - opex_pct_of_revenue
  - sub_accounts: optional list{name,pct}
behavior:
  - Default single % of total revenue.
  - If sub-accounts, total must = 100%.
```

UI:
- Input sliders per sub-account.  
- Animated pie chart summary.  

---

## 5. Tuition CPI Frequency & Partner Yield Growth

### Tuition CPI Frequency
```yaml
FR-8.4: Tuition.CPIFrequency
purpose: Allow tuition increase every N years (1–3).
process:
  - CPI applied only when (year - base_year) % periodicity == 0.
```
Visual: tuition chart highlights CPI step years; hover shows applied rate.

### Partner Yield Growth
```yaml
FR-8.5: Rent.PartnerModelYieldGrowth
inputs:
  - land_size: land area in square meters (sqm)
  - land_price_per_sqm: price per square meter of land
  - bua_size: Built-Up Area (BUA) in square meters (sqm)
  - bua_price_per_sqm: price per square meter of BUA
  - yield_base: initial yield percentage for first year
  - yield_growth_rate: annual yield growth percentage
  - growth_frequency: years between yield increases (1, 2, or 3 years)
process:
  - capex_base = (land_size × land_price_per_sqm) + (bua_size × bua_price_per_sqm)
  - year1: rent = capex_base × yield_base
  - subsequent: yield = yield_base × (1 + yield_growth_rate)^(floor((t-1)/growth_frequency))
  - rent(t) = capex_base × yield(t)
```
Visual: bar chart of yield % by year with smooth growth animation. Growth applied based on frequency (every 1, 2, or 3 years).  

---

## 6. Timeline Logic

```yaml
periods:
  2023-2024: actuals_locked=true
  2025-2027: transition_mode (rent cloned from 2024A)
  2028+: relocation_mode (new rent model)
```

### Historical Data (2023-2024)
- **Status:** Locked actuals (read-only for all users except Admin).
- **Data Entry:** Admin can import/enter actual financial data via Admin panel.
- **Source:** Import from accounting systems (CSV/Excel) or manual entry.
- **Fields:** Revenue, Costs, Rent, Staff, Capex, Opex, Students, Capacity per curriculum.
- **Validation:** Must reconcile with accounting records; checksum stored for audit.

### Transition Years (2025-2027)
- **Rent Behavior:** Automatically clones rent amount from 2024 actuals (2024A).
- **Other Data:** Planner can modify curriculum, tuition, staffing, opex, capex.
- **Purpose:** Model pre-relocation scenarios with current rent structure.

### Relocation Mode (2028+)
- **Rent Behavior:** New rent model applies (FixedEscalation, RevenueShare, or PartnerModel).
- **Full Planning:** All inputs editable; new campus assumptions apply.
- **Validation:** Must select rent model; cannot use 2024A rent.

---

## 7. Roles & Permissions

```yaml
roles:
  ADMIN:
    - Manage global CPI, interest, discount, inflation, validation rules
    - Approve, lock, or archive versions
  PLANNER:
    - Create and clone versions
    - Input curriculum, tuition, staffing, capex, opex
    - Run validation and generate reports
  VIEWER:
    - Read-only access
```

---

## 8. Performance Targets

```yaml
performance:
  total_roundtrip_ms: ≤50
  techniques:
    - Virtualized table rendering
    - Delta computation on edit
    - Memoized rent/staff/opex calculations
    - Edge caching via Supabase functions
  benchmark:
    - 100k rows render <40 ms
```

---

## 9. Governance & Audit UI

- Approval banner colors (Draft=gray, Ready=green, Locked=blue).  
- Reason dialog for transitions.  
- Audit table with timestamps and action tags.  

---

## 10. Reports & Exports

- **PDF (A4 landscape):** minimalist layout, logo watermark.  
- **XLSX:** full data with year columns.  
- **Footer:** version ID + checksum.  
- Slide-in export panel (PDF / Excel / CSV).  

---

## 11. Accessibility & Dark Mode

```yaml
accessibility:
  contrast_ratio: ≥4.5:1
  keyboard_focus: visible_ring
  reduced_motion: supported
  screen_reader_labels: full
dark_mode:
  background: #0E1012
  surface: #181B1F
  accent: #0099FF
  text: light_gray
```

---

## 12. Acceptance Highlights

- [ ] Dual curricula fully modeled and aggregated.  
- [ ] Transition years reuse 2024 rent by default.  
- [ ] 3 rent models available post-2028.  
- [ ] Capex reinvestment per class.  
- [ ] Opex as % of revenue with sub-accounts.  
- [ ] Tuition simulation < 50 ms response.  
- [ ] CPI frequency + partner yield growth applied correctly.  
- [ ] UI WCAG 2.1 AA compliant.  

---

## 13. Data Model

### Core Tables

```yaml
tables:
  - versions:
      columns:
        - id (uuid, PK)
        - name (string)
        - status (enum: Draft, Ready, Locked, Archived)
        - created_by (uuid, FK → users)
        - created_at (timestamp)
        - updated_at (timestamp)
        - approved_by (uuid, FK → users, nullable)
        - approved_at (timestamp, nullable)
        - checksum (string, for validation)
      indexes: [status, created_by, created_at]
      
  - curriculum_plan:
      columns:
        - id (uuid, PK)
        - version_id (uuid, FK → versions, CASCADE DELETE)
        - curriculum_type (enum: FR, IB)
        - year (integer, 2023-2052)
        - capacity (integer)
        - students (integer, ≤ capacity)
        - tuition (decimal)
        - teacher_ratio (decimal, e.g., 0.15)
        - non_teacher_ratio (decimal, e.g., 0.08)
        - cpi_frequency (integer, 1-3)
        - cpi_base_year (integer)
      indexes: [version_id, curriculum_type, year]
      constraints: [students ≤ capacity, year in range]
      
  - rent_plan:
      columns:
        - id (uuid, PK)
        - version_id (uuid, FK → versions, CASCADE DELETE)
        - year (integer)
        - model_type (enum: FixedEscalation, RevenueShare, PartnerModel)
        - amount (decimal)
        - model_config (jsonb)  # Model-specific parameters
          # FixedEscalation: {base_rent, escalation_rate, escalation_frequency}
          # RevenueShare: {revenue_share_pct, minimum_rent, maximum_rent}
          # PartnerModel: {land_size, land_price_per_sqm, bua_size, bua_price_per_sqm, yield_base, yield_growth_rate, growth_frequency}
      indexes: [version_id, year]
      
  - capex_rule:
      columns:
        - id (uuid, PK)
        - class (enum: Building, FF&E, IT, Other)
        - cycle_years (integer)
        - inflation_index (string, FK → admin_settings.cpi_name)
        - base_cost (decimal)
        - created_by (uuid, FK → users)
        - created_at (timestamp)
      indexes: [class]
      
  - capex_plan:
      columns:
        - id (uuid, PK)
        - version_id (uuid, FK → versions, CASCADE DELETE)
        - year (integer)
        - class (enum: Building, FF&E, IT, Other)
        - amount (decimal)
        - rule_id (uuid, FK → capex_rule, nullable)
      indexes: [version_id, year, class]
      
  - opex_plan:
      columns:
        - id (uuid, PK)
        - version_id (uuid, FK → versions, CASCADE DELETE)
        - sub_account (string, nullable)
        - pct_of_revenue (decimal, 0-100)
        - amount (decimal, calculated)
      indexes: [version_id]
      constraints: [sum(pct_of_revenue) = 100 if sub_accounts exist]
      
  - tuition_simulation:
      columns:
        - id (uuid, PK)
        - version_id (uuid, FK → versions, CASCADE DELETE)
        - rent_model_type (enum: FixedEscalation, RevenueShare, PartnerModel)
        - adjustment_factor (decimal, -20 to 50)
        - target_margin (decimal, nullable)
        - target_ebitda (decimal, nullable)
        - results (jsonb)  # Year-by-year simulation results
        - created_at (timestamp)
      indexes: [version_id, created_at]
      
  - audit_log:
      columns:
        - id (uuid, PK)
        - user_id (uuid, FK → users)
        - action (enum: Create, Update, Delete, Approve, Lock, Archive)
        - entity_type (string)
        - entity_id (uuid)
        - changes (jsonb, nullable)
        - reason (text, nullable)
        - timestamp (timestamp)
      indexes: [user_id, entity_type, entity_id, timestamp]
      
  - admin_settings:
      columns:
        - id (uuid, PK)
        - setting_key (string, unique)
        - setting_value (jsonb)
        - updated_by (uuid, FK → users)
        - updated_at (timestamp)
      indexes: [setting_key]
      # Examples: 
      #   global_cpi_rate: decimal (annual CPI rate for salary adjustments)
      #   discount_rate: decimal
      #   interest_rate: decimal
      #   teacher_salary_fr: decimal (base salary for French curriculum teachers)
      #   teacher_salary_ib: decimal (base salary for IB curriculum teachers)
      #   non_teacher_salary_fr: decimal (base salary for French curriculum non-teachers)
      #   non_teacher_salary_ib: decimal (base salary for IB curriculum non-teachers)
      #   validation_rules: jsonb
      # Note: Salaries are subject to annual CPI adjustments
      
  - users:
      columns:
        - id (uuid, PK)
        - email (string, unique)
        - name (string)
        - role (enum: ADMIN, PLANNER, VIEWER)
        - created_at (timestamp)
      indexes: [email, role]
```

### Relationships
- `versions` → `curriculum_plan` (1:N)
- `versions` → `rent_plan` (1:N)
- `versions` → `capex_plan` (1:N)
- `versions` → `opex_plan` (1:N)
- `versions` → `tuition_simulation` (1:N)
- `capex_rule` → `capex_plan` (1:N, nullable)
- `users` → `versions` (1:N, created_by)
- `users` → `audit_log` (1:N)

---

## 14. Validation Rules

### Data Validation

```yaml
curriculum_plan:
  - students ≤ capacity (per year, per curriculum)
  - tuition > 0
  - teacher_ratio > 0 and < 1
  - non_teacher_ratio > 0 and < 1
  - cpi_frequency in [1, 2, 3]
  - year in [2023, 2052]
  
rent_plan:
  - amount ≥ 0
  - year in [2023, 2052]
  - For transition years (2025-2027): model_type must be null (uses 2024A)
  - For relocation years (2028+): model_type must be set
  
opex_plan:
  - pct_of_revenue in [0, 100]
  - If sub_accounts exist: sum(pct_of_revenue) = 100
  - amount = Revenue × pct_of_revenue (auto-calculated)
  
capex_plan:
  - amount ≥ 0
  - year in [2023, 2052]
  
versions:
  - name must be unique per user (or globally, TBD)
  - Cannot delete Locked versions (only Archive)
  - Cannot edit Locked versions (only Admin can unlock)
```

### Business Logic Validation

```yaml
version_status_transitions:
  - Draft → Ready: requires all mandatory fields filled, validation passes
  - Ready → Locked: Admin approval required, reason mandatory
  - Locked → Draft: Admin only, reason mandatory
  - Any → Archived: Admin only
  
rent_model_validation:
  - FixedEscalation: base_rent > 0, escalation_rate ≥ 0
  - RevenueShare: revenue_share_pct in [0, 100], min_rent ≤ max_rent if both set
  - PartnerModel: land_size > 0, land_price_per_sqm > 0, bua_size > 0, bua_price_per_sqm > 0, yield_base > 0, yield_growth_rate ≥ 0, growth_frequency in [1, 2, 3]
  
aggregation_validation:
  - Total students across curricula ≤ total capacity across curricula
  - Revenue calculations must reconcile: (Students × Tuition) per curriculum
```

### Performance Validation
- All calculations must complete in < 50 ms for single-version operations.
- Report generation must complete in < 5 seconds for 30-year horizon.
- UI interactions must respond in < 50 ms (excluding network latency).

---

## 15. Calculations

| Logic | Formula |
|-------|----------|
| **Revenue (per curriculum)** | `Students × Tuition` |
| **Total Revenue** | `Revenue(FR) + Revenue(IB)` |
| **Teacher Salary (with CPI)** | `teacher_salary_base × (1 + CPI_rate)^(year - base_year)` |
| **Non-Teacher Salary (with CPI)** | `non_teacher_salary_base × (1 + CPI_rate)^(year - base_year)` |
| **Staff Costs (per curriculum)** | `(Students × teacher_ratio × teacher_salary_with_CPI) + (Students × non_teacher_ratio × non_teacher_salary_with_CPI)` |
| **Total Staff Costs** | `Staff_Costs(FR) + Staff_Costs(IB)` |
| **Opex Total** | `Revenue × opex_pct` (or sum of sub-accounts if configured) |
| **Opex Sub-Account** | `Revenue × sub_account_pct` |
| **Tuition with CPI** | `tuition_base × (1 + CPI_rate)^(number_of_applications)` where CPI applied when `(year - base_year) % periodicity == 0` |
| **FixedEscalation Rent** | `base_rent × (1 + escalation_rate)^(number_of_escalations)` |
| **RevenueShare Rent** | `Revenue × revenue_share_pct` (with min/max constraints if set) |
| **PartnerModel Capex Base** | `(land_size × land_price_per_sqm) + (bua_size × bua_price_per_sqm)` |
| **PartnerModel Yield** | `yield(t) = yield_base × (1 + yield_growth_rate)^(floor((t-1)/growth_frequency))` where growth_frequency ∈ [1,2,3] |
| **PartnerModel Rent** | `capex_base × yield(t)` |
| **Capex Replacement** | Auto-triggered per `cycle_years`, cost = `base_cost × (1 + inflation_index)^(years_since_base)` |
| **EBITDA** | `Revenue - Staff_Costs - Rent - Opex - Other_Costs` |
| **Cash Flow** | `EBITDA - Capex - Interest - Taxes` |
| **NPV (Rent)** | `Σ(rent(t) / (1 + discount_rate)^t)` for t = 1 to n |
| **Tuition Sensitivity** | Iterative: Adjust tuition until `EBITDA ≥ EBITDA_target` |

---

## 16. Risks & Mitigations

| Risk | Mitigation |
|------|-------------|
| High rent sensitivity | Tuition simulation + Rent NPV optimizer |
| Mapping errors | Validation and checksum traceability |
| Calculation drift | Shared rounding util across modules |
| Performance regressions | CI test benchmark threshold 50 ms |
| Data integrity issues | Database constraints + application-level validation |
| User permission errors | Role-based access control (RBAC) with audit logging |
| Version cloning errors | Deep copy with relationship preservation + validation |

---

## 17. Definition of Done

```yaml
- All functional requirements implemented.
- All automated tests passed.
- Rent + tuition simulation validated by Admin.
- Capex/Opex logic confirmed.
- Reports exported successfully.
- UI passes design QA + accessibility audit.
- PRD_v1.4.md stored in /docs and linked in README.
```
