# Pull Request

## What
<!-- Summary of change -->

## Why (Linked Decisions)
<!-- ADR/Decision log links -->
- Decision: [link to decision log or ADR]
- Related: [link to related decisions]

## Scope
<!-- Files touched -->
- Modified: 
- Added:
- Deleted:

## Tests
<!-- Unit, contract, integration, E2E results -->

### Unit Tests
- [ ] Coverage: ≥80% line, ≥90% critical branch
- [ ] All tests passing
- [ ] Report: `/artifacts/<ticket-id>/unit-report.json`

### Contract Tests
- [ ] Matches frozen interface spec
- [ ] Report: `/artifacts/<ticket-id>/contract-results.txt`

### Integration Tests
- [ ] Passes with real DB/containers
- [ ] Report: `/artifacts/<ticket-id>/integration-report.txt`

### E2E Tests
- [ ] Happy path + 1 negative path
- [ ] Report: `/artifacts/<ticket-id>/e2e-report.html`

### Non-Functional Tests
- [ ] Performance: <10% regression
- [ ] Accessibility: WCAG 2.1 AA
- [ ] Security scan: clean
- [ ] Report: `/artifacts/<ticket-id>/perf-smoke.csv`

## Validation
<!-- Local run output -->
- Lint: ✅ / ❌
- Type check: ✅ / ❌
- Unit tests: ✅ / ❌
- **Confidence ≥90%:** ✅ / ❌

**Confidence Assessment:**
<!-- Explain confidence level and any concerns -->

## Documentation
<!-- Updated README/API/Changelog links -->
- [ ] README updated
- [ ] API reference updated
- [ ] CHANGELOG.md updated
- [ ] PRD/TSD/UI docs updated (if applicable)

## Risk & Rollback
<!-- Notes + flag/rollback plan -->
- **Risks:**
  - 
  
- **Rollback Plan:**
  - 
  
- **Feature Flags:**
  - 

## QA Readiness
<!-- Link to QA readiness report -->
- Report: `/artifacts/<ticket-id>/qa-readiness.md`
- Decision: Go / No-Go
- QA Owner: @username

## Evidence Bundle
<!-- Attach to /artifacts/<ticket-id>/ -->
- [ ] unit-report.json
- [ ] contract-results.txt
- [ ] integration-report.txt
- [ ] e2e-report.html (or screenshots)
- [ ] perf-smoke.csv
- [ ] qa-readiness.md

---

## Checklist

- [ ] All pre-code greenlight boxes checked
- [ ] Design Freeze Hash recorded
- [ ] Interface specs versioned
- [ ] All dependencies ≥ DRL-2
- [ ] Tests written and passing
- [ ] Documentation updated
- [ ] Confidence ≥90%
- [ ] Evidence bundle attached
- [ ] QA sign-off obtained

