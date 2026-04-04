# Rural Land Lease & Settlement Management Portal

## Quick Start

```bash
docker compose up --build -d
./run_tests.sh
```

## Services

| Service | Port | Description |
|---------|------|-------------|
| API     | 8000 | ThinkPHP REST API |
| MySQL   | 3307 | MySQL 8.0 (external debug port) |

## URLs

- Health: http://localhost:8000/health
- Frontend: http://localhost:8000/static/index.html
- API Docs: http://localhost:8000/api/docs
- Login: http://localhost:8000/static/login.html
- Register: http://localhost:8000/static/register.html

## Test Credentials

Database (inline in docker-compose.yml):
- Host: `db` (internal) / `localhost:3307` (external)
- Database: `rural_lease`
- User: `app` / Password: `app`

## Testing

```bash
# Full test suite (unit + API, executed in container)
./run_tests.sh
```

**151 tests** (64 unit + 87 API), **407 assertions**, 100% pass rate.

### Coverage Model

Coverage is measured at the **route level** (backend) and **module/page level** (frontend),
not by line-instrumentation. This is because the mandated architecture uses Docker-first
HTTP integration tests: PHPUnit runs inside the container and calls the live API via curl
against a real MySQL database. The test process and the server process are separate PHP
processes, so Xdebug/PCOV line-coverage cannot observe the server-side code.

- **Backend:** 42/42 API routes tested (100%). Each route has at least one happy-path
  and one negative-path (401/403/400/409) assertion where applicable.
- **Frontend:** 7/7 JS modules, 4/4 HTML pages, 13/13 page sections tested (100%).
  Each module is verified for correct serving, key function definitions, API path
  references, and UI element presence.

## Stack

- **Backend:** ThinkPHP 6 (PHP 8.2)
- **Frontend:** Layui 2.9
- **Database:** MySQL 8.0
- **Runtime:** Docker Compose

## Architecture

- REST API with JSON responses
- Bearer token authentication
- RBAC + geographic scope enforcement
- X-Trace-Id on all responses
- Structured error envelope: `{status, code, message, trace_id}`
- AES-256 encryption for sensitive fields
- Append-only audit log
- Idempotent payment posting (10-minute window)

## Modules

Auth, Profiles, Verification, Contracts, Invoices, Payments, Refunds,
Messaging, Risk Controls, Audit, Admin Config, Background Jobs, Exports.
