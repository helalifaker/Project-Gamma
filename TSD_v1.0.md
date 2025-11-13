# 🏗️ School Relocation Planner — Technical Specification Document (TSD v1.0)

```yaml
meta:
  document: Technical Specification Document
  version: 1.0
  date: 2025-01-XX
  related_prd: PRD_v1.4.md
  tech_stack:
    frontend: Next.js 16 (App Router)
    ui_framework: React 19
    styling: Tailwind CSS v4
    components: shadcn/ui
    animations: Framer Motion
    database: Supabase (PostgreSQL)
    auth: Supabase Auth
    orm: Supabase Client (direct) or Prisma (optional)
    types: TypeScript 5.x
    testing: Vitest + React Testing Library
    linting: ESLint + Prettier
  performance_target: ≤50ms response time
```

---

## 1. Architecture Overview

### 1.1 System Architecture

```
┌─────────────────────────────────────────────────────────┐
│                    Client (Browser)                     │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐ │
│  │   Next.js    │  │   React 19   │  │  Tailwind v4 │ │
│  │  App Router  │  │   Components │  │   + shadcn   │ │
│  └──────────────┘  └──────────────┘  └──────────────┘ │
└───────────────────────┬─────────────────────────────────┘
                        │ HTTPS
┌───────────────────────▼─────────────────────────────────┐
│              Next.js API Routes / Edge Functions        │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐ │
│  │   Server     │  │   Edge       │  │   Middleware │ │
│  │   Actions    │  │   Functions  │  │   (Auth)     │ │
│  └──────────────┘  └──────────────┘  └──────────────┘ │
└───────────────────────┬─────────────────────────────────┘
                        │
┌───────────────────────▼─────────────────────────────────┐
│                    Supabase Backend                    │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐ │
│  │  PostgreSQL  │  │     Auth    │  │   Storage   │ │
│  │   Database   │  │   (RLS)     │  │   (Files)   │ │
│  └──────────────┘  └──────────────┘  └──────────────┘ │
└─────────────────────────────────────────────────────────┘
```

### 1.2 Design Patterns

- **Server Components First:** Use React Server Components for data fetching
- **Client Components:** Only for interactivity (forms, animations, real-time updates)
- **Server Actions:** For mutations (create, update, delete)
- **Optimistic Updates:** For instant UI feedback
- **Virtualization:** For large data tables (react-window or @tanstack/react-virtual)
- **Memoization:** React.useMemo, useCallback for expensive calculations
- **Delta Updates:** Only send changed fields, not entire objects

---

## 2. Technology Stack

### 2.1 Core Dependencies

```json
{
  "dependencies": {
    "next": "^16.0.0",
    "react": "^19.0.0",
    "react-dom": "^19.0.0",
    "@supabase/supabase-js": "^2.39.0",
    "@supabase/ssr": "^0.1.0",
    "tailwindcss": "^4.0.0",
    "framer-motion": "^11.0.0",
    "zod": "^3.22.0",
    "date-fns": "^3.0.0",
    "recharts": "^2.10.0",
    "jspdf": "^2.5.0",
    "xlsx": "^0.18.0"
  },
  "devDependencies": {
    "typescript": "^5.3.0",
    "@types/node": "^20.0.0",
    "@types/react": "^18.0.0",
    "eslint": "^8.0.0",
    "prettier": "^3.0.0",
    "vitest": "^1.0.0",
    "@testing-library/react": "^14.0.0"
  }
}
```

### 2.2 shadcn/ui Components

Install core components:
- Button, Input, Select, Slider
- Table, Dialog, Sheet, Tabs
- Card, Badge, Label
- Form (react-hook-form + zod)
- Toast (sonner)

---

## 3. Project Structure

```
project-gamma/
├── app/                          # Next.js App Router
│   ├── (auth)/                   # Auth routes group
│   │   ├── login/
│   │   └── callback/
│   ├── (dashboard)/              # Protected routes group
│   │   ├── layout.tsx            # Dashboard layout
│   │   ├── page.tsx              # Overview page
│   │   ├── versions/
│   │   │   ├── page.tsx          # Versions list
│   │   │   └── [id]/
│   │   │       ├── page.tsx      # Version detail
│   │   │       └── tuition-sim/
│   │   ├── tuition-simulator/
│   │   ├── compare/
│   │   ├── reports/
│   │   └── admin/
│   ├── api/                      # API routes
│   │   ├── versions/
│   │   ├── calculations/
│   │   └── exports/
│   ├── layout.tsx                 # Root layout
│   └── globals.css
├── components/                   # React components
│   ├── ui/                       # shadcn/ui components
│   ├── layout/
│   │   ├── TopNav.tsx
│   │   ├── Breadcrumbs.tsx
│   │   └── Sidebar.tsx
│   ├── versions/
│   │   ├── VersionList.tsx
│   │   ├── VersionCard.tsx
│   │   └── VersionDetail.tsx
│   ├── curriculum/
│   │   ├── CurriculumSplitView.tsx
│   │   └── CurriculumTable.tsx
│   ├── rent/
│   │   ├── RentLens.tsx
│   │   ├── RentModelSelector.tsx
│   │   └── RentModelForms.tsx
│   ├── tuition-simulator/
│   │   ├── TuitionSimulator.tsx
│   │   ├── RentConfigPanel.tsx
│   │   ├── TuitionControls.tsx
│   │   └── ImpactCharts.tsx
│   ├── calculations/
│   │   ├── CalculationEngine.ts
│   │   └── RentCalculators.ts
│   └── charts/
│       ├── RevenueChart.tsx
│       ├── RentLoadChart.tsx
│       └── SensitivityCurve.tsx
├── lib/                          # Utilities & helpers
│   ├── supabase/
│   │   ├── client.ts             # Browser client
│   │   ├── server.ts             # Server client
│   │   └── middleware.ts          # Auth middleware
│   ├── utils/
│   │   ├── calculations.ts       # Core calculation functions
│   │   ├── validations.ts        # Zod schemas
│   │   ├── formatting.ts         # Number/date formatting
│   │   └── constants.ts         # App constants
│   └── hooks/
│       ├── useVersion.ts
│       ├── useCalculations.ts
│       └── useRentModel.ts
├── types/                        # TypeScript types
│   ├── database.ts               # Generated from Supabase
│   ├── version.ts
│   ├── curriculum.ts
│   └── rent.ts
├── supabase/                     # Supabase config
│   ├── migrations/
│   ├── seed.sql
│   └── functions/                # Edge Functions (if needed)
├── public/                       # Static assets
├── .env.local                    # Environment variables
├── .env.example
├── next.config.js
├── tailwind.config.ts
├── tsconfig.json
└── package.json
```

---

## 4. Database Design

### 4.1 Supabase Setup

```typescript
// lib/supabase/client.ts
import { createClient } from '@supabase/supabase-js'

export const supabase = createClient(
  process.env.NEXT_PUBLIC_SUPABASE_URL!,
  process.env.NEXT_PUBLIC_SUPABASE_ANON_KEY!
)
```

### 4.2 Database Schema (PostgreSQL)

```sql
-- Enable UUID extension
CREATE EXTENSION IF NOT EXISTS "uuid-ossp";

-- Users table (extends Supabase auth.users)
CREATE TABLE public.users (
  id UUID PRIMARY KEY REFERENCES auth.users(id) ON DELETE CASCADE,
  email TEXT UNIQUE NOT NULL,
  name TEXT,
  role TEXT NOT NULL CHECK (role IN ('ADMIN', 'PLANNER', 'VIEWER')),
  created_at TIMESTAMPTZ DEFAULT NOW(),
  updated_at TIMESTAMPTZ DEFAULT NOW()
);

-- Versions table
CREATE TABLE public.versions (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  name TEXT NOT NULL,
  status TEXT NOT NULL CHECK (status IN ('Draft', 'Ready', 'Locked', 'Archived')),
  created_by UUID NOT NULL REFERENCES public.users(id),
  created_at TIMESTAMPTZ DEFAULT NOW(),
  updated_at TIMESTAMPTZ DEFAULT NOW(),
  approved_by UUID REFERENCES public.users(id),
  approved_at TIMESTAMPTZ,
  checksum TEXT,
  CONSTRAINT unique_name_per_user UNIQUE (name, created_by)
);

CREATE INDEX idx_versions_status ON public.versions(status);
CREATE INDEX idx_versions_created_by ON public.versions(created_by);
CREATE INDEX idx_versions_created_at ON public.versions(created_at DESC);

-- Curriculum plan table
CREATE TABLE public.curriculum_plan (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  version_id UUID NOT NULL REFERENCES public.versions(id) ON DELETE CASCADE,
  curriculum_type TEXT NOT NULL CHECK (curriculum_type IN ('FR', 'IB')),
  year INTEGER NOT NULL CHECK (year >= 2023 AND year <= 2052),
  capacity INTEGER NOT NULL CHECK (capacity > 0),
  students INTEGER NOT NULL CHECK (students >= 0 AND students <= capacity),
  tuition DECIMAL(12, 2) NOT NULL CHECK (tuition > 0),
  teacher_ratio DECIMAL(5, 4) NOT NULL CHECK (teacher_ratio > 0 AND teacher_ratio < 1),
  non_teacher_ratio DECIMAL(5, 4) NOT NULL CHECK (non_teacher_ratio > 0 AND non_teacher_ratio < 1),
  cpi_frequency INTEGER NOT NULL CHECK (cpi_frequency IN (1, 2, 3)),
  cpi_base_year INTEGER NOT NULL,
  created_at TIMESTAMPTZ DEFAULT NOW(),
  updated_at TIMESTAMPTZ DEFAULT NOW(),
  CONSTRAINT unique_curriculum_year UNIQUE (version_id, curriculum_type, year)
);

CREATE INDEX idx_curriculum_plan_version ON public.curriculum_plan(version_id);
CREATE INDEX idx_curriculum_plan_type_year ON public.curriculum_plan(curriculum_type, year);

-- Rent plan table
CREATE TABLE public.rent_plan (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  version_id UUID NOT NULL REFERENCES public.versions(id) ON DELETE CASCADE,
  year INTEGER NOT NULL CHECK (year >= 2023 AND year <= 2052),
  model_type TEXT CHECK (model_type IN ('FixedEscalation', 'RevenueShare', 'PartnerModel')),
  amount DECIMAL(12, 2) NOT NULL CHECK (amount >= 0),
  model_config JSONB,
  created_at TIMESTAMPTZ DEFAULT NOW(),
  updated_at TIMESTAMPTZ DEFAULT NOW(),
  CONSTRAINT unique_rent_year UNIQUE (version_id, year)
);

CREATE INDEX idx_rent_plan_version ON public.rent_plan(version_id);
CREATE INDEX idx_rent_plan_year ON public.rent_plan(year);

-- Capex rule table (Admin managed)
CREATE TABLE public.capex_rule (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  class TEXT NOT NULL CHECK (class IN ('Building', 'FF&E', 'IT', 'Other')),
  cycle_years INTEGER NOT NULL CHECK (cycle_years > 0),
  inflation_index TEXT NOT NULL,
  base_cost DECIMAL(12, 2) NOT NULL CHECK (base_cost > 0),
  trigger_type TEXT NOT NULL CHECK (trigger_type IN ('cycle', 'utilization', 'both')) DEFAULT 'cycle',
  utilization_threshold DECIMAL(5, 2) CHECK (utilization_threshold >= 0 AND utilization_threshold <= 100),
  created_by UUID NOT NULL REFERENCES public.users(id),
  created_at TIMESTAMPTZ DEFAULT NOW(),
  updated_at TIMESTAMPTZ DEFAULT NOW()
);

CREATE INDEX idx_capex_rule_class ON public.capex_rule(class);

-- Capex plan table
CREATE TABLE public.capex_plan (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  version_id UUID NOT NULL REFERENCES public.versions(id) ON DELETE CASCADE,
  year INTEGER NOT NULL CHECK (year >= 2023 AND year <= 2052),
  class TEXT NOT NULL CHECK (class IN ('Building', 'FF&E', 'IT', 'Other')),
  amount DECIMAL(12, 2) NOT NULL CHECK (amount >= 0),
  rule_id UUID REFERENCES public.capex_rule(id),
  created_at TIMESTAMPTZ DEFAULT NOW(),
  updated_at TIMESTAMPTZ DEFAULT NOW(),
  CONSTRAINT unique_capex_year_class UNIQUE (version_id, year, class)
);

CREATE INDEX idx_capex_plan_version ON public.capex_plan(version_id);
CREATE INDEX idx_capex_plan_year_class ON public.capex_plan(year, class);

-- Opex plan table
CREATE TABLE public.opex_plan (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  version_id UUID NOT NULL REFERENCES public.versions(id) ON DELETE CASCADE,
  sub_account TEXT,
  pct_of_revenue DECIMAL(5, 2) NOT NULL CHECK (pct_of_revenue >= 0 AND pct_of_revenue <= 100),
  amount DECIMAL(12, 2) NOT NULL CHECK (amount >= 0),
  created_at TIMESTAMPTZ DEFAULT NOW(),
  updated_at TIMESTAMPTZ DEFAULT NOW()
);

CREATE INDEX idx_opex_plan_version ON public.opex_plan(version_id);

-- Tuition simulation table
CREATE TABLE public.tuition_simulation (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  version_id UUID NOT NULL REFERENCES public.versions(id) ON DELETE CASCADE,
  rent_model_type TEXT NOT NULL CHECK (rent_model_type IN ('FixedEscalation', 'RevenueShare', 'PartnerModel')),
  adjustment_factor DECIMAL(5, 2) NOT NULL CHECK (adjustment_factor >= -20 AND adjustment_factor <= 50),
  target_margin DECIMAL(5, 2),
  target_ebitda DECIMAL(12, 2),
  results JSONB,
  created_at TIMESTAMPTZ DEFAULT NOW()
);

CREATE INDEX idx_tuition_sim_version ON public.tuition_simulation(version_id);
CREATE INDEX idx_tuition_sim_created ON public.tuition_simulation(created_at DESC);

-- Audit log table
CREATE TABLE public.audit_log (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  user_id UUID NOT NULL REFERENCES public.users(id),
  action TEXT NOT NULL CHECK (action IN ('Create', 'Update', 'Delete', 'Approve', 'Lock', 'Archive')),
  entity_type TEXT NOT NULL,
  entity_id UUID NOT NULL,
  changes JSONB,
  reason TEXT,
  timestamp TIMESTAMPTZ DEFAULT NOW()
);

CREATE INDEX idx_audit_user ON public.audit_log(user_id);
CREATE INDEX idx_audit_entity ON public.audit_log(entity_type, entity_id);
CREATE INDEX idx_audit_timestamp ON public.audit_log(timestamp DESC);

-- Admin settings table
CREATE TABLE public.admin_settings (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  setting_key TEXT UNIQUE NOT NULL,
  setting_value JSONB NOT NULL,
  updated_by UUID NOT NULL REFERENCES public.users(id),
  updated_at TIMESTAMPTZ DEFAULT NOW()
);

CREATE INDEX idx_admin_settings_key ON public.admin_settings(setting_key);

-- Financial statement settings (Admin configurable)
-- Setting keys:
--   - 'dso_days': Days Sales Outstanding (integer)
--   - 'dpo_days': Days Payable Outstanding (integer)
--   - 'deferred_revenue_pct': Deferred Revenue as % of Revenue (decimal)
--   - 'global_cpi_rate': Annual CPI rate (decimal)
--   - 'discount_rate': Discount rate for NPV (decimal)
--   - 'interest_rate': Interest rate (decimal)
--   - 'teacher_salary_fr': Base teacher salary FR (decimal)
--   - 'teacher_salary_ib': Base teacher salary IB (decimal)
--   - 'non_teacher_salary_fr': Base non-teacher salary FR (decimal)
--   - 'non_teacher_salary_ib': Base non-teacher salary IB (decimal)

-- Row Level Security (RLS) Policies
ALTER TABLE public.versions ENABLE ROW LEVEL SECURITY;
ALTER TABLE public.curriculum_plan ENABLE ROW LEVEL SECURITY;
ALTER TABLE public.rent_plan ENABLE ROW LEVEL SECURITY;
ALTER TABLE public.capex_plan ENABLE ROW LEVEL SECURITY;
ALTER TABLE public.opex_plan ENABLE ROW LEVEL SECURITY;
ALTER TABLE public.tuition_simulation ENABLE ROW LEVEL SECURITY;
ALTER TABLE public.audit_log ENABLE ROW LEVEL SECURITY;
ALTER TABLE public.admin_settings ENABLE ROW LEVEL SECURITY;

-- RLS Policies (examples - adjust based on requirements)
-- Versions: Users can read their own versions, Admins can read all
CREATE POLICY "Users can read own versions" ON public.versions
  FOR SELECT USING (auth.uid() = created_by OR EXISTS (
    SELECT 1 FROM public.users WHERE id = auth.uid() AND role = 'ADMIN'
  ));

CREATE POLICY "Planners can create versions" ON public.versions
  FOR INSERT WITH CHECK (auth.uid() = created_by AND EXISTS (
    SELECT 1 FROM public.users WHERE id = auth.uid() AND role IN ('ADMIN', 'PLANNER')
  ));

-- Similar policies for other tables...
```

### 4.3 Type Generation

```bash
# Generate TypeScript types from Supabase schema
npx supabase gen types typescript --project-id <project-id> > types/database.ts
```

---

## 5. API Design

### 5.1 Server Actions Pattern

```typescript
// app/actions/versions.ts
'use server'

import { createClient } from '@/lib/supabase/server'
import { revalidatePath } from 'next/cache'
import { z } from 'zod'

const createVersionSchema = z.object({
  name: z.string().min(1),
  // ... other fields
})

export async function createVersion(formData: FormData) {
  const supabase = await createClient()
  
  // Validate
  const validated = createVersionSchema.parse({
    name: formData.get('name'),
    // ...
  })
  
  // Insert
  const { data, error } = await supabase
    .from('versions')
    .insert({
      ...validated,
      created_by: (await supabase.auth.getUser()).data.user?.id,
      status: 'Draft'
    })
    .select()
    .single()
  
  if (error) throw error
  
  revalidatePath('/versions')
  return data
}
```

### 5.2 API Routes (for complex operations)

```typescript
// app/api/calculations/route.ts
import { NextRequest, NextResponse } from 'next/server'
import { calculateRent } from '@/lib/utils/calculations'

export async function POST(request: NextRequest) {
  const body = await request.json()
  const result = calculateRent(body.model, body.params)
  return NextResponse.json(result)
}
```

---

## 6. Component Architecture

### 6.1 Server Components (Default)

```typescript
// app/(dashboard)/versions/page.tsx
import { createClient } from '@/lib/supabase/server'
import { VersionList } from '@/components/versions/VersionList'

export default async function VersionsPage() {
  const supabase = await createClient()
  const { data: versions } = await supabase
    .from('versions')
    .select('*')
    .order('created_at', { ascending: false })
  
  return <VersionList versions={versions} />
}
```

### 6.2 Client Components (Interactivity)

```typescript
// components/tuition-simulator/TuitionSimulator.tsx
'use client'

import { useState, useMemo } from 'react'
import { useCalculations } from '@/lib/hooks/useCalculations'

export function TuitionSimulator({ baseVersion }) {
  const [rentModel, setRentModel] = useState('FixedEscalation')
  const [tuitionAdjustment, setTuitionAdjustment] = useState({ fr: 0, ib: 0 })
  
  const results = useMemo(() => {
    return calculateImpact(baseVersion, rentModel, tuitionAdjustment)
  }, [baseVersion, rentModel, tuitionAdjustment])
  
  return (
    <div className="grid grid-cols-3 gap-4">
      <RentConfigPanel model={rentModel} onChange={setRentModel} />
      <ImpactCharts data={results} />
      <TuitionControls 
        adjustment={tuitionAdjustment} 
        onChange={setTuitionAdjustment} 
      />
    </div>
  )
}
```

---

## 7. State Management

### 7.1 Strategy

- **Server State:** Supabase queries (use React Server Components)
- **Form State:** react-hook-form + zod
- **UI State:** React useState/useReducer (local component state)
- **Global State:** React Context (theme, user preferences) - minimal use
- **URL State:** Next.js searchParams for filters/pagination

### 7.2 Custom Hooks

```typescript
// lib/hooks/useCalculations.ts
import { useMemo } from 'react'
import { calculateRent, calculateRevenue, calculateEBITDA } from '@/lib/utils/calculations'

export function useCalculations(version, rentModel, tuitionAdjustment) {
  return useMemo(() => {
    const rent = calculateRent(rentModel, version.rentParams)
    const revenue = calculateRevenue(version.curriculum, tuitionAdjustment)
    const ebitda = calculateEBITDA(revenue, rent, version.opex, version.staff)
    
    return {
      rent,
      revenue,
      ebitda,
      rentLoadPercent: (rent.total / revenue.total) * 100
    }
  }, [version, rentModel, tuitionAdjustment])
}
```

---

## 8. Performance Optimization

### 8.1 Techniques

1. **Virtualization:** Use `@tanstack/react-virtual` for large tables
2. **Memoization:** React.useMemo for expensive calculations
3. **Delta Updates:** Only update changed fields
4. **Debouncing:** For slider inputs (use `use-debounce`)
5. **Streaming:** Use React Suspense for progressive loading
6. **Edge Caching:** Cache calculation results in Edge Functions

### 8.2 Calculation Engine

```typescript
// lib/utils/calculations.ts
export function calculateRent(model: RentModel, params: RentParams): RentResult {
  switch (model) {
    case 'FixedEscalation':
      return calculateFixedEscalation(params)
    case 'RevenueShare':
      return calculateRevenueShare(params)
    case 'PartnerModel':
      return calculatePartnerModel(params)
  }
}

// Memoize expensive calculations
const memoizedCalculate = memoize(calculateRent, {
  maxSize: 100,
  ttl: 60000 // 1 minute cache
})
```

### 8.3 Code Splitting

```typescript
// Lazy load heavy components
const TuitionSimulator = dynamic(() => import('@/components/tuition-simulator/TuitionSimulator'), {
  loading: () => <Skeleton />
})
```

---

## 9. Security & Authentication

### 9.1 Supabase Auth Setup

```typescript
// lib/supabase/middleware.ts
import { createServerClient } from '@supabase/ssr'
import { NextResponse, type NextRequest } from 'next/server'

export async function updateSession(request: NextRequest) {
  const supabase = createServerClient(
    process.env.NEXT_PUBLIC_SUPABASE_URL!,
    process.env.NEXT_PUBLIC_SUPABASE_ANON_KEY!,
    {
      cookies: {
        get(name: string) {
          return request.cookies.get(name)?.value
        },
        set(name: string, value: string, options: any) {
          request.cookies.set({ name, value, ...options })
        },
        remove(name: string, options: any) {
          request.cookies.set({ name, value: '', ...options })
        },
      },
    }
  )

  const { data: { user } } = await supabase.auth.getUser()

  if (!user && !request.nextUrl.pathname.startsWith('/login')) {
    return NextResponse.redirect(new URL('/login', request.url))
  }

  return NextResponse.next()
}
```

### 9.2 Role-Based Access Control

```typescript
// lib/utils/permissions.ts
export function hasPermission(userRole: string, action: string, resource: string): boolean {
  const permissions = {
    ADMIN: ['*'],
    PLANNER: ['create:version', 'update:version', 'read:version', 'export:report'],
    VIEWER: ['read:version', 'read:report']
  }
  
  return permissions[userRole]?.includes(action) || permissions[userRole]?.includes('*')
}
```

---

## 10. Development Workflow

### 10.1 Environment Setup

```bash
# .env.local
NEXT_PUBLIC_SUPABASE_URL=https://xxx.supabase.co
NEXT_PUBLIC_SUPABASE_ANON_KEY=xxx
SUPABASE_SERVICE_ROLE_KEY=xxx  # Server-side only
DATABASE_URL=postgresql://...  # For migrations
DIRECT_URL=postgresql://...    # Direct connection
```

### 10.2 Git Workflow

- **Main branch:** Production-ready code
- **Develop branch:** Integration branch
- **Feature branches:** `feature/version-management`, `feature/tuition-simulator`
- **Commit convention:** Conventional Commits

### 10.3 Development Commands

```json
{
  "scripts": {
    "dev": "next dev",
    "build": "next build",
    "start": "next start",
    "lint": "next lint",
    "type-check": "tsc --noEmit",
    "test": "vitest",
    "test:ui": "vitest --ui",
    "db:generate": "supabase gen types typescript",
    "db:migrate": "supabase db push",
    "db:seed": "supabase db seed"
  }
}
```

---

## 11. Testing Strategy

### 11.1 Unit Tests

```typescript
// lib/utils/__tests__/calculations.test.ts
import { describe, it, expect } from 'vitest'
import { calculateFixedEscalation } from '../calculations'

describe('calculateFixedEscalation', () => {
  it('calculates rent with escalation', () => {
    const result = calculateFixedEscalation({
      baseRent: 100000,
      escalationRate: 0.03,
      years: 5
    })
    expect(result[5]).toBeCloseTo(115927.41, 2)
  })
})
```

### 11.2 Integration Tests

```typescript
// app/api/__tests__/versions.test.ts
import { describe, it, expect } from 'vitest'
import { createVersion } from '@/app/actions/versions'

describe('Version API', () => {
  it('creates a new version', async () => {
    const version = await createVersion({
      name: 'Test Version',
      // ...
    })
    expect(version.id).toBeDefined()
  })
})
```

### 11.3 E2E Tests (Future)

- Use Playwright for critical user flows
- Test tuition simulator workflow
- Test version creation and cloning

---

## 12. Deployment

### 12.1 Vercel Deployment

```yaml
# vercel.json
{
  "buildCommand": "npm run build",
  "devCommand": "npm run dev",
  "installCommand": "npm install",
  "framework": "nextjs",
  "env": {
    "NEXT_PUBLIC_SUPABASE_URL": "@supabase-url",
    "NEXT_PUBLIC_SUPABASE_ANON_KEY": "@supabase-anon-key"
  }
}
```

### 12.2 Environment Variables

- Production: Set in Vercel dashboard
- Staging: Separate Supabase project
- Development: `.env.local`

---

## 13. Monitoring & Observability

### 13.1 Performance Monitoring

- Vercel Analytics
- Web Vitals tracking
- Custom performance marks for calculations

### 13.2 Error Tracking

- Sentry integration for error tracking
- Logging via Supabase Edge Functions

---

## 14. Admin Page Features

### 14.1 Admin Page Structure

The Admin page (`/admin`) provides configuration interfaces for system-wide settings that affect all versions and calculations.

**Navigation Tabs:**
- Global Settings (CPI, Interest, Discount Rates)
- Capex Configuration (Reinvestment Rules)
- Financial Statement Settings (DSO, DPO, Deferred Revenue %)
- User Management
- Validation Rules
- System Audit Log

### 14.2 Capex Configuration

**Location:** `/admin` → Capex Configuration tab

**Purpose:** Configure reinvestment rules for each Capex category (Building, FF&E, IT, Other).

**UI Components:**

```typescript
// components/admin/CapexConfiguration.tsx
interface CapexRule {
  id: string
  class: 'Building' | 'FF&E' | 'IT' | 'Other'
  cycle_years: number        // Reinvestment cycle in years
  inflation_index: string    // CPI index to use for inflation
  base_cost: number          // Base cost for reinvestment
  trigger_type: 'cycle' | 'utilization' | 'both'
  utilization_threshold?: number  // Optional: utilization % threshold
}
```

**Features:**
- **Category-based Rules:** Each category (Building, FF&E, IT, Other) has independent reinvestment rules
- **Reinvestment Cycle:** Configure cycle years per category (e.g., Building: 20y, FF&E: 7y, IT: 4y)
- **Inflation Index:** Select which CPI index to apply for cost escalation
- **Base Cost:** Set initial/base cost for each category
- **Trigger Options:**
  - Cycle-based: Automatic reinvestment every N years
  - Utilization-based: Trigger when utilization threshold is met (optional)
  - Both: Use either trigger condition
- **Timeline Visualization:** Visual timeline showing reinvestment cycles across all categories
- **Edit/Delete:** Modify or remove rules (with confirmation)

**Data Flow:**
1. Admin creates/updates capex rules in Admin page
2. Rules stored in `capex_rule` table
3. When Planner creates version, system auto-calculates reinvestment years based on rules
4. Planner can override auto-calculated capex in version detail page

**Validation:**
- Cycle years must be > 0
- Base cost must be > 0
- Inflation index must exist in admin_settings
- Cannot delete rule if referenced by existing versions

### 14.3 Financial Statement Settings

**Location:** `/admin` → Financial Statement Settings tab

**Configurable Parameters:**

1. **DSO (Days Sales Outstanding):** Integer (e.g., 30 days)
   - Used to calculate Accounts Receivable: `AR = (Revenue × DSO) / 365`

2. **DPO (Days Payable Outstanding):** Integer (e.g., 45 days)
   - Used to calculate Accounts Payable
   - **COGS Calculation:** `COGS = Staff Costs + Rent + Other Operating Expenses`
   - `AP = (COGS × DPO) / 365`

3. **Deferred Revenue %:** Decimal (e.g., 0.35 = 35%)
   - Percentage of revenue that is deferred
   - `Deferred Revenue = Revenue × deferred_revenue_pct`

**UI:**
- Input fields for each parameter with validation
- Save button with confirmation
- Audit log entry on save

---

## 15. Financial Statement Structures

### 15.1 Profit & Loss (P&L) Statement

**Structure (as per requirements - detailed format):**

```typescript
interface ProfitAndLoss {
  year: number
  
  // Revenues
  totalRevenues: number
  
  // Operating Expenses
  salariesAndRelatedCosts: number
  schoolRent: number
  otherExpenses: number
  totalOperatingExpenses: number
  
  // Depreciation & Amortization
  depreciationAndAmortization: number
  
  // Interest
  interestIncome: number
  interestExpenses: number
  
  // Net Result
  netResult: number
}
```

**Calculation Flow:**
1. Total Revenues = Sum of (Students × Tuition) per curriculum
2. Salaries and Related Costs = Staff Costs (teachers + non-teachers) with CPI
3. School Rent = From rent_plan (based on selected model)
4. Other Expenses = Opex (from opex_plan)
5. Total Operating Expenses = Salaries + Rent + Other Expenses
6. Depreciation and Amortization = From capex depreciation schedule
7. Interest Income = From admin_settings (if applicable)
8. Interest Expenses = Calculated from debt (if applicable)
9. Net Result = Total Revenues - Total Operating Expenses - Depreciation - Interest Expenses + Interest Income

### 15.2 Balance Sheet (Simplified)

**Structure:**

```typescript
interface BalanceSheet {
  year: number
  
  // Assets
  currentAssets: {
    cashOnHandAndBank: number
    accountsReceivable: number
    totalCurrentAssets: number
  }
  
  nonCurrentAssets: {
    tangibleIntangibleAssetsGross: number
    accumulatedDepreciationAmortization: number
    nonCurrentAssetsNet: number
  }
  
  totalAssets: number
  
  // Liabilities
  currentLiabilities: {
    accountsPayable: number
    deferredIncome: number
    totalCurrentLiabilities: number
  }
  
  nonCurrentLiabilities: {
    provisions: number
  }
  
  totalLiabilities: number
  
  // Equity
  equity: {
    retainedEarnings: number
    netResult: number
    totalEquity: number
  }
}
```

**Calculations:**
- **Cash on Hand and Bank:** From cash flow statement (ending cash)
- **Accounts Receivable:** `(Revenue × DSO) / 365` (using admin DSO setting)
- **Accounts Payable:** `(COGS × DPO) / 365` where `COGS = Staff Costs + Rent + Other Operating Expenses`
- **Deferred Income:** `Revenue × deferred_revenue_pct` (using admin setting)
- **Tangible & Intangible Assets Gross:** Cumulative capex investments
- **Accumulated Depreciation:** Sum of depreciation from all years
- **Provisions:** Calculated based on business rules (if applicable)
- **Retained Earnings:** Cumulative net results
- **Net Result:** From P&L statement

### 15.3 Cash Flow Statement (Simplified)

**Structure:**

```typescript
interface CashFlow {
  year: number
  
  // Operating Activities
  netResult: number
  adjustments: {
    depreciation: number
    accountsReceivableChange: number
    accountsPayableChange: number
    deferredIncomeChange: number
    provisionsChange: number
  }
  netCashFromOperating: number
  
  // Investing Activities
  additionsOfFixedAssets: number  // Capex
  netCashFromInvesting: number
  
  // Financing Activities
  financingActivities: number
  netCashFromFinancing: number
  
  // Summary
  netCashFlowForYear: number
  cashAtBeginningOfYear: number
  cashAtEndOfYear: number
}
```

**Calculations:**
- **Net Result:** From P&L
- **Depreciation:** From P&L
- **Accounts Receivable Change:** `AR(year) - AR(year-1)`
- **Accounts Payable Change:** `AP(year) - AP(year-1)`
- **Deferred Income Change:** `Deferred(year) - Deferred(year-1)`
- **Provisions Change:** `Provisions(year) - Provisions(year-1)`
- **Additions of Fixed Assets:** Total capex for the year
- **Net Cash Flow:** Operating + Investing + Financing
- **Cash at End:** `Cash at Beginning + Net Cash Flow`

---

## 16. Calculation Updates

### 16.1 COGS Calculation

**Formula:**
```typescript
COGS = Staff Costs + Rent + Other Operating Expenses
```

Where:
- **Staff Costs:** `(Students × teacher_ratio × teacher_salary_with_CPI) + (Students × non_teacher_ratio × non_teacher_salary_with_CPI)` per curriculum
- **Rent:** From rent_plan (based on selected model)
- **Other Operating Expenses:** From opex_plan (as % of revenue or sub-accounts)

### 16.2 Working Capital Calculations

**Accounts Receivable:**
```typescript
AR = (Revenue × DSO_days) / 365
// DSO_days from admin_settings
```

**Accounts Payable:**
```typescript
COGS = Staff Costs + Rent + Other Operating Expenses
AP = (COGS × DPO_days) / 365
// DPO_days from admin_settings
```

**Deferred Revenue:**
```typescript
Deferred Revenue = Revenue × deferred_revenue_pct
// deferred_revenue_pct from admin_settings
```

---

## 17. Documentation

### 17.1 Code Documentation

- JSDoc comments for public functions
- README.md for setup instructions
- Component Storybook (future)

### 17.2 API Documentation

- OpenAPI/Swagger for API routes (if needed)
- Inline comments for Server Actions

---

## 18. Implementation Phases

### Phase 1: Foundation (Week 1-2)
- [ ] Project setup (Next.js, Tailwind, shadcn/ui)
- [ ] Supabase configuration
- [ ] Database schema creation
- [ ] Authentication setup
- [ ] Basic layout components

### Phase 2: Core Features (Week 3-4)
- [ ] Version management (CRUD)
- [ ] Dual-curriculum model
- [ ] Basic calculations engine
- [ ] Rent model calculations

### Phase 3: Tuition Simulator (Week 5-6)
- [ ] Tuition Simulator page
- [ ] Rent configuration panel
- [ ] Real-time calculations
- [ ] Charts and visualizations

### Phase 4: Advanced Features (Week 7-8)
- [ ] Capex auto-reinvestment
- [ ] Opex structure
- [ ] Reports & exports
- [ ] Compare functionality

### Phase 5: Polish & Testing (Week 9-10)
- [ ] Performance optimization
- [ ] Accessibility improvements
- [ ] Testing coverage
- [ ] Documentation

---

## 19. Key Decisions & Rationale

1. **Next.js App Router:** Modern, supports Server Components, better performance
2. **Supabase:** Integrated auth, database, real-time capabilities
3. **Direct Supabase Client:** Simpler than Prisma for this use case
4. **Server Components First:** Better performance, less client-side JS
5. **Zod for Validation:** Type-safe, works with forms
6. **shadcn/ui:** Accessible, customizable, Tailwind-based
7. **No Redux/Zustand:** React built-in state sufficient for this app

---

## 20. Open Questions

- [ ] Should we use Prisma or direct Supabase client?
- [ ] Do we need real-time subscriptions?
- [ ] What's the expected concurrent user count?
- [ ] Do we need file storage for reports?
- [ ] Should calculations run on Edge Functions for better performance?

---

**Document Status:** Draft v1.0  
**Last Updated:** 2025-01-XX  
**Next Review:** After Phase 1 completion

