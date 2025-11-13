# CURSOR PROJECT RULES — Dependency & Context Guardrails

## 0) Mission
Generate **small, accurate diffs** that compile, typecheck, lint, and test **without inventing dependencies or APIs**. When information is missing, **stop and ask** instead of guessing.

---

## 1) Hard Rules (Non‑Negotiable)
- **No invented deps/APIs:** Use only packages listed in `package.json` (or language‑equivalent) and only symbols present in the repo.  
- **Respect versions/config:** Assume exactly the versions and configs declared in the project (e.g., Node/Java/Go versions, compiler/tsconfig, framework config).  
- **Change surface control:** Touch only files explicitly listed in the request. Do **not** modify build/config/env files unless explicitly permitted.  
- **Citations required:** Near any non‑obvious API/type, add a one‑line code comment citing the artifact (file + export) or external spec (e.g., OpenAPI path).  
- **Uncertainty protocol:** If any API/type/route/schema is missing or ambiguous, **STOP** and output a short **Question List** (file + symbol needed). No placeholders.

---

## 2) Minimum Context Pack (Load Before Coding)
Always load these if present in the repo (or their language equivalents):

- **Runtime & toolchain:** `package.json` (+ lockfile), compiler config (e.g., `tsconfig.json`, `pyproject.toml`, `pom.xml`, `go.mod`), linter/formatter config.  
- **External interfaces:** API schemas (`openapi.yaml`/GraphQL), DB schema (`schema.prisma`, migrations, SQL DDL), message contracts (Avro/Protobuf).  
- **Local interfaces & invariants:** public types/interfaces, shared error model, config/constants, and any domain barrel files (index exports).  
- **Architecture decisions:** short blueprint/ADR summaries + CONTRIBUTING/DoD.

If some aren’t present, proceed with what exists but apply the **Uncertainty protocol** when needed.

---

## 3) Standard Output Contract
When generating code, **return only**:
1) **Unified diffs** per file (paths included).  
2) **Tests** for new/changed logic.  
3) **Very short note** (≤ 10 lines) listing any assumed artifacts and links to their citations in code.

No long prose, no speculative changes, no unrelated refactors.

---

## 4) Task Templates (Reusable Prompts)

### 4.1 API / Backend Slice
**Goal:** Implement or modify `{METHOD} {PATH}` (or function `{name}`).  
**Touch only:** `{explicit file paths}`.  
**Use:** Existing clients/utilities; do not add dependencies.  
**Contract:**  
- Follow project error shape and config conventions.  
- Validate inputs/outputs (runtime validation if project uses it).  
- Return **diffs + tests** (`__tests__/` or project standard).  
**Pre‑flight:**  
- List exact files/symbols you will call (with paths).  
- Predict 3 compile/typecheck errors you might hit.  
**If unknown:** output **Question List** and stop.

### 4.2 UI / Component Slice
**Goal:** Implement `{Component}` with given props/behavior.  
**Touch only:** `{explicit file paths}`.  
**Constraints:** No new UI libs; follow accessibility/i18n rules if present.  
**Contract:** diffs + snapshot/golden tests + minimal docs.  
**Pre‑flight & Uncertainty:** same as API slice.

---

## 5) Missing‑Context Tripwires (Add These to Each Ask)
- “List the **exact files and symbols** you will use (with paths). If any are unknown, stop and produce a **Question List** instead of guessing.”  
- “Predict **3 likely compile/type errors** based on config and planned imports.”  
- “Add **citation comments** beside non‑obvious calls (file:export or OpenAPI path).”

---

## 6) Review Checklist (Definition of Done)
- **Build safety:** compiles, lints, typechecks cleanly.  
- **Tests:** fail first then pass; meaningful assertions; no trivial snapshots.  
- **API accuracy:** called functions/routes/models **exist**; parameter & return types match.  
- **No new deps** (unless explicitly allowed) and versions respected.  
- **Security & safety:** input validation, safe HTML/escaping, no secrets in code, least‑privilege client usage.  
- **Performance:** avoid N+1 / heavy synchronous loops; streaming or pagination where standard.  
- **Accessibility/i18n (UI):** semantic structure; no hard‑coded text if i18n enforced.  
- **Citations present** for non‑obvious APIs/types.  
- **Change surface adhered to** (only allowed files modified).

---

## 7) Risk Categories → Two‑Step Flow Required
For migrations, security/auth, payments, data deletion, and governance/policy logic:  
1) **Plan diff** (schema/flow changes + rollback).  
2) After approval, **apply diff** with tests and data safeguards.

---

## 8) Context Budget Policy (When Tokens Are Tight)
1) **Always include:** runtime/toolchain config, API/DB schema, shared types/errors/config, and the target module.  
2) **Then include** direct neighbors (imports of the touched file).  
3) **Then include** small, high‑signal docs (ADR summaries/blueprint).  
4) **Exclude** low‑signal/big files (lockfiles, large snapshots) unless explicitly relevant.

---

## 9) House Style & Hygiene
- Follow project formatter/linter rules; no stylistic churn.  
- Avoid loose typing where strong typing is available.  
- Reuse established utilities; do not duplicate helpers or create parallel patterns.  
- Keep diffs small and purposeful; one concern per change.

---

## 10) Failure Handling (How to Recover)
- If build/test errors occur, request the **exact logs** and return **corrected diffs only**.  
- If a cited artifact doesn’t exist, acknowledge the mistake, revert the change, and produce a **Question List**.  
- If requirements conflict with established invariants, call it out and propose a minimal, explicit exception.

---

## 11) Example Citation Comments (One‑liners near the usage)
```ts
// uses POST /v1/users (openapi.yaml: paths./v1/users.post)
```

```ts
// uses model User (schema.prisma: model User { ... })
```

```ts
// uses fetchTenant from src/lib/clients/auth.ts
```

---

## 12) Quick “Ask” Skeleton (Copy with Each Request)
```
Goal: {one sentence}
Touch only: {paths}
Must reuse: {clients/utils}
Keep invariants: {list}

Pre-flight:
- Files & symbols to call (with paths):
- 3 likely compile/type errors:

Output:
- Unified diffs per file
- Tests for new logic
- Short note listing assumptions + where cited in code

If any symbol/route/schema is unknown → return a Question List and stop.
```
