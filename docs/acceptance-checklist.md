# Rural Lease Portal - Acceptance Checklist

## 1. Runtime and Startup

- [x] `docker compose up --build` succeeds
- [x] API and DB healthchecks are healthy
- [x] `GET /health` returns `{"status":"ok"}`
- [x] `./run_tests.sh` runs from host and executes tests in container

## 2. Security Gates

- [x] Password policy enforced (>=12, upper/lower/digit/symbol)
- [x] Lockout and backoff enforced (5 in rolling 15 min)
- [x] RBAC enforced on protected routes (401/403 verified)
- [x] Geographic scope enforced on all scoped resources
- [x] Sensitive fields encrypted at rest (AES-256-CBC)
- [x] Audit log append-only (no DELETE/UPDATE endpoint)

## 3. Domain Gates

- [x] Verification workflow works with required reject reason
- [x] Duplicate detection and merge flow works
- [x] Contract schedule generation works from real DB state
- [x] Late fee rule and cap are accurate (day 5/6 boundary, $250 cap, integer cents)
- [x] Payment idempotency works in 10-minute window (scoped by method+route+actor+key)
- [x] Messaging recall and risk actions work as specified

## 4. Fullstack Gates

- [x] Every implemented endpoint is wired to Layui UI
- [x] UI handles loading/success/error/policy-blocked states
- [x] No hardcoded mock responses in production paths

## 5. Test Gates

- [x] Unit tests cover password, lockout, fee calculation, idempotency, encryption
- [x] API tests cover happy path + 400/401/403/409 paths
- [x] API tests cover scope isolation and role denial
- [x] End-to-end flow works: register -> verify -> contract -> invoice -> payment -> export

## 6. Final Test Evidence (Release)

- **Total tests:** 151 (64 unit + 87 API)
- **Total assertions:** 407
- **Pass rate:** 151/151 (100%)
- **Backend route coverage:** 42/42 routes tested (100%)
- **Frontend module coverage:** 7/7 JS modules + 4/4 HTML pages + 13/13 page sections (100%)
- **Cold-start validated:** All 8 migrations apply from empty DB, all tests pass
- **Regressions:** None
