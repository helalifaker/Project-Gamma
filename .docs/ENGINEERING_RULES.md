# Engineering Rules & Collaboration Framework

This document contains the complete Engineering Rules & Collaboration Framework for the School Relocation Planner project.

**See `.cursorrules` for the Cursor-specific implementation of these rules.**

---

## Quick Reference

- **Confidence Gate:** ≥90% required before any code change
- **Dependency Readiness:** All dependencies must be ≥ DRL-2 before coding
- **Testing:** ≥80% line coverage, ≥90% critical branch coverage
- **Documentation:** Update docs in same PR as code changes

---

## Full Documentation

For complete details, see:
- `.cursorrules` - Cursor-specific rules and behavior
- `.github/ISSUE_TEMPLATE/feature.yml` - Feature ticket template
- `.github/PULL_REQUEST_TEMPLATE.md` - PR template

---

## Project-Specific Guidelines

### Financial Calculations
- Always use Decimal.js (see `lib/utils/calculations.ts`)
- Use `roundCurrency()` for all monetary values
- Never mix `number` with `Decimal`

### Performance
- Target <50ms response time (p95)
- Use Edge Functions for heavy calculations
- Implement tag-based cache invalidation

### Database
- Use covering indexes for read paths
- Use `version_hash` for cache invalidation
- Precompute statements in `version_cache` table

---

**Last Updated:** 2025-01-XX  
**Version:** 1.0

