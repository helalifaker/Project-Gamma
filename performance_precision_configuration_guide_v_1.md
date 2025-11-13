# ⚡ Performance & Precision Configuration Guide (v1.0)

> Scope: Ensures the School Relocation Planner delivers **ultra-fast autosaving**, **professional decimal precision**, and **consistent financial accuracy** with a premium user experience.

---

## 1. 🔄 Autosave System — Fast, Reliable, and Audit-Proof

### 1.1 Objectives
- Save user inputs automatically without interrupting workflow.
- Reflect saved state instantly (optimistic UI).
- Prevent server overload through debouncing.
- Maintain auditability and rollback safety.

### 1.2 Key Behaviors
- **Trigger:** Every input change.
- **Delay:** 400 ms debounce after last edit.
- **Feedback:** Toast (✓ Saved) + timestamp badge.
- **Offline:** Queue unsaved edits in local storage until network restored.

### 1.3 Implementation Pattern (Conceptual)
```typescript
useEffect(() => {
  const handler = setTimeout(async () => {
    if (isDirty) await saveDraft(formValues) // server action call
  }, 400)
  return () => clearTimeout(handler)
}, [formValues])
```

### 1.4 User Feedback UX
| Event | Visual Response |
|--------|-----------------|
| Save Success | Toast ✓ “Saved at 18:42” + subtle pulse on Save icon |
| Save Error | Toast ⚠️ “Connection lost – autosave queued” |
| Offline Queue | Local badge “Offline edits pending” |
| Reconnect | Auto-sync queued edits + success toast |

### 1.5 Audit Integrity
- Each autosave writes a **hash checksum** of payload.
- Logged to `audit_log` with timestamp, user, delta fields.
- Enables rollback and validation consistency.

---

## 2. 🧮 Decimal Precision & Rounding Discipline

### 2.1 Core Principle
> All internal computations must preserve **full floating-point precision**, while display formatting applies **consistent rounding rules**.

### 2.2 Decimal Library Configuration
```typescript
import Decimal from 'decimal.js-light'
Decimal.set({ precision: 20, rounding: Decimal.ROUND_HALF_UP })
```

### 2.3 Financial Rounding Table
| Context | Precision | Rounding | Display Format |
|----------|------------|-----------|----------------|
| Tuition, Rent, Opex | 2 decimals | Half-up | 1 234 567.89 |
| Ratios, Percentages | 1–2 decimals | Half-even | 45.6 % |
| Large Cash, NPV | 0–2 decimals | Floor (conservative) | SAR 1 234 567 |
| Internal Calculations | Full double precision | Never rounded | Machine precision |

### 2.4 Utility Functions (Design Specification)
```typescript
function toCurrency(value, locale = 'en-SA', decimals = 2) {
  return new Intl.NumberFormat(locale, {
    style: 'currency',
    currency: 'SAR',
    minimumFractionDigits: decimals,
    maximumFractionDigits: decimals
  }).format(value)
}

function toPercent(value, decimals = 1) {
  return `${value.toFixed(decimals)}%`
}

function roundFinancial(value, mode = 'half-even', decimals = 2) {
  const decimal = new Decimal(value)
  return decimal.toDecimalPlaces(decimals, mode === 'half-even' ? Decimal.ROUND_HALF_EVEN : Decimal.ROUND_HALF_UP)
}
```

### 2.5 Display Rules
- Align decimals using `tabular-nums` typography.
- Do not truncate; always round.
- Ensure totals and subtotals match displayed precision.

---

## 3. 🧠 Real-Time Validation Flow

### 3.1 Objectives
- Prevent invalid data entry before save.
- Validate locally in < 50 ms using lightweight schemas.

### 3.2 Structure
```typescript
const schema = z.object({
  tuition: z.number().min(1000).max(200000),
  rentLoad: z.number().max(0.5),
  opexPct: z.number().min(0).max(1)
})
```

### 3.3 User Feedback
- Inline error: red outline + tooltip.
- Toast summary: “Validation failed (2 fields)”.
- No blocking modals.

### 3.4 Autosave Integration
- Validation executes **before debounce delay ends**.
- If invalid, autosave is postponed until resolved.

---

## 4. ⚙️ Performance Benchmarks

| Layer | Metric | Target |
|--------|---------|--------|
| Autosave latency | Perceived | ≤ 200 ms |
| Calculation refresh | End-to-end | ≤ 40 ms |
| Validation cycle | Execution | ≤ 50 ms |
| Table scroll | FPS | 60 stable |
| Total UI idle cost | Memory | < 30 MB |

**Perceived performance** is achieved through optimistic rendering and micro-animation feedback, not raw compute speed alone.

---

## 5. 🎨 Layout & UX Consistency

- Maintain 12-column, 1400 px max width grid.
- Sticky headers & first columns for year-based tables.
- Align numbers at decimal separator; label columns with pivot years.
- Keep all micro-interactions ≤ 120 ms.
- Preserve clarity: no abrupt opacity transitions; prefer fade-slide 0.12 s.

---

## 6. 🔐 Reliability & Safety Net

- Offline queue resilience via local storage or IndexedDB.
- Checksum verification on each autosave commit.
- Conflict resolution (server vs local): use **last updated timestamp** and prompt user if drift > 10 min.
- Periodic background sync every 60 s to ensure state freshness.

---

## 7. ✅ Quality Assurance Checklist

| Category | Goal | Validation Method |
|-----------|------|-------------------|
| Autosave | < 200 ms perceived save | Profile network trace |
| Decimal rounding | Consistent with finance rules | Compare totals vs Excel reference |
| Layout | Numbers aligned; grid consistent | Visual QA in both modes |
| Validation | < 50 ms execution | Log schema latency |
| Accessibility | Focus visible; contrast ≥ 4.5:1 | Lighthouse + manual review |
| Dark mode parity | Identical precision, contrast | Snapshot diff check |

---

## 8. 📦 Summary — The 4 Pillars of Performance

1. **Optimistic Autosave:** Always feel instantaneous.
2. **Precision Engine:** Decimal.js ensures consistent financial math.
3. **Smart Validation:** Zod keeps data clean without friction.
4. **UI Integrity:** Smooth, accurate, accessible, and trustworthy.

---

**End of Performance & Precision Configuration Guide v1.0**