# Completeness Checklist Reference

## 1. Feature Completeness Matrix

Compare every feature specified in the design against the implementation:

```markdown
## Feature Completeness Matrix

| # | Feature / Requirement | Design Spec Section | Expected Files | Found | Status |
|---|----------------------|--------------------|-----------|----|--------|
| 1 | User registration | SRS FR-001 | src/user/routes.ts, src/user/service.ts | Yes | Complete |
| 2 | Email verification | SRS FR-002 | src/auth/verify.ts | Yes | Complete |
| 3 | Password reset | SRS FR-003 | src/auth/reset.ts | No | MISSING |
| 4 | Order creation | SRS FR-010 | src/order/routes.ts | Partial | STUB |
| 5 | Payment processing | SRS FR-011 | src/payment/service.ts | Yes | Complete |
```

### Status Definitions

| Status | Meaning | Severity |
|--------|---------|----------|
| **Complete** | Feature fully implemented with all code paths | None |
| **Partial** | Feature exists but has incomplete logic or missing code paths | BLOCKER |
| **STUB** | Feature file exists but contains placeholder or minimal implementation | BLOCKER |
| **MISSING** | Feature not found in the codebase at all | BLOCKER |
| **Deferred** | Feature intentionally excluded with documented justification | None (if approved) |

---

## 2. Module Completeness Checklist

For each domain module in the implementation:

| Check | Required Files | Verification |
|-------|---------------|-------------|
| Domain model | `[module].model.ts` | Contains type definitions and validation schemas |
| Business logic | `[module].service.ts` | Contains orchestration logic, not just pass-through |
| Data access | `[module].repository.ts` | Contains database operations, not empty stubs |
| API endpoints | `[module].routes.ts` | Contains route handlers with real logic |
| Unit tests | `[module].model.test.ts`, `[module].service.test.ts` | Contains meaningful test functions |

### Module Inspection Procedure

For each module:
1. **Verify file existence**: All expected files present
2. **Verify file substance**: Files contain actual logic (not just imports or empty exports)
3. **Verify function completeness**: All functions have implementations (not `pass`, `{}`, or `throw "not implemented"`)
4. **Verify data flow**: Functions connect properly (service calls repository, routes call service)

---

## 3. API Endpoint Completeness

Compare every endpoint defined in the API contract against the implementation:

```markdown
## API Endpoint Completeness

| Method | Path | Handler | Auth | Validation | Response | Status |
|--------|------|---------|------|-----------|----------|--------|
| POST | /api/v1/users | createUser | Yes ✓ | Zod schema ✓ | 201 + user ✓ | Complete |
| GET | /api/v1/users/:id | getUser | Yes ✓ | Path param ✓ | 200 + user ✓ | Complete |
| PUT | /api/v1/users/:id | updateUser | Yes ✓ | Zod schema ✓ | 200 + user ✓ | Complete |
| DELETE | /api/v1/users/:id | deleteUser | Yes ✓ | Path param ✓ | 204 ✓ | Complete |
| GET | /api/v1/orders | listOrders | Yes ✓ | Query params | — | STUB — returns [] |
| POST | /api/v1/orders | createOrder | No ✗ | None | — | INCOMPLETE |
```

### Per-Endpoint Checks

For each endpoint verify:
1. **Route exists**: Handler function is registered for the correct method and path
2. **Authentication**: Auth middleware is applied (if required by spec)
3. **Validation**: Request body/params/query validated against schema
4. **Business logic**: Handler calls service layer (not just returning static data)
5. **Error responses**: Handler returns appropriate error codes for failure cases
6. **Response format**: Response matches the documented schema

---

## 4. Database Migration Completeness

Verify all specified database entities have corresponding migrations:

| Entity | Migration File | Columns | Indexes | Constraints | Status |
|--------|---------------|---------|---------|-------------|--------|
| users | 001_create_users.sql | All specified ✓ | email (unique) ✓ | FK to org ✓ | Complete |
| orders | 002_create_orders.sql | Missing `discount` column | — | — | INCOMPLETE |
| payments | — | — | — | — | MISSING |

### Migration Checks

1. **Entity coverage**: Every entity in the data model has a create migration
2. **Column coverage**: Every column specified in the model exists in the migration
3. **Index coverage**: Indexed columns from the design have corresponding CREATE INDEX
4. **Constraint coverage**: Foreign keys, unique constraints, NOT NULL as specified
5. **Seed data**: If the design specifies default data (roles, categories), verify seed migrations exist

---

## 5. Configuration Completeness

### Environment Variable Audit

```markdown
## Environment Variable Completeness

| Variable | Referenced In | .env.example | Default Value | Status |
|----------|-------------|-------------|---------------|--------|
| DATABASE_URL | src/config/database.ts | Yes ✓ | — | Complete |
| JWT_SECRET | src/config/auth.ts | Yes ✓ | — | Complete |
| STRIPE_KEY | src/payment/service.ts | No ✗ | — | MISSING from .env.example |
| PORT | src/server.ts | Yes ✓ | 3000 | Complete |
| LOG_LEVEL | src/config/logger.ts | No ✗ | "info" | MISSING from .env.example |
```

### Configuration Checks

1. **All env var references documented**: Every `process.env.X` / `os.environ["X"]` appears in `.env.example`
2. **No hardcoded values**: No URLs, ports, or connection strings hardcoded in source files
3. **Sensible defaults**: Non-sensitive variables have development defaults
4. **Secret variables identified**: Sensitive variables documented as requiring secure storage

---

## 6. Documentation Completeness

### Project Documentation Check

| Document | Required | Exists | Complete | Status |
|----------|----------|--------|----------|--------|
| README.md | Yes | Yes | Has empty "Deployment" section | WARNING |
| .env.example | Yes | Yes | Missing 2 variables | BLOCKER |
| API documentation | Recommended | No | — | WARNING |
| Database schema docs | Recommended | No | — | INFO |

### Inline Documentation Check

| Check | Method | Acceptable Threshold |
|-------|--------|---------------------|
| Public API docs | Grep for exported functions without JSDoc/docstring | ≤ 10% undocumented |
| Complex logic explained | Manual review of functions with cyclomatic complexity > 5 | All complex functions documented |
| No template text | Search for `[INSERT`, `[TBD]`, `[TODO]`, `README template` | Zero instances |

---

## 7. CLEAN Verdict Criteria

### Mandatory Requirements (ALL must pass)

| # | Requirement | Measurement |
|---|------------|-------------|
| 1 | Zero BLOCKER findings across all 6 scan categories | Automated + manual scan |
| 2 | Zero WARNING findings (or all justified) | Each warning individually justified |
| 3 | Feature completeness: 100% of specified features implemented | Feature matrix shows all Complete or approved Deferred |
| 4 | API endpoint completeness: 100% implemented | Endpoint matrix shows all Complete |
| 5 | Configuration completeness: all env vars documented | Env var audit shows no gaps |
| 6 | No scaffold code in production paths | Static pattern scan clean |

### Acceptable Exceptions

| Finding Type | Exception Condition |
|-------------|-------------------|
| INFO findings | Always acceptable — document and report |
| WARNING in test files | MOCK/FAKE/DUMMY in test files is expected |
| TODO in documentation | If it refers to future roadmap items, not current build scope |
| Placeholder in .env.example | Placeholder VALUES in .env.example are expected (that is its purpose) |

---

## 8. Re-Scan Protocol

### When to Re-Scan

After bob-the-builder addresses findings:
1. Receive updated codebase from build-management
2. Execute ALL 6 scan steps (not just the previously failed ones)
3. New issues may be introduced during remediation — full scan catches these

### Re-Scan Differences

| Aspect | First Scan | Re-Scan |
|--------|-----------|---------|
| Scope | Full codebase | Full codebase (same scope) |
| Focus | All categories equally | Extra attention to previously found patterns |
| New findings | Report all | Report all (including new regressions) |
| Previous findings | Initial detection | Verify resolution — did the fix work? |

### Maximum Cycles

- **Cycle 1**: Initial scan → findings → remediation
- **Cycle 2**: Re-scan → remaining findings → remediation
- **After Cycle 2**: If BLOCKERs persist → ESCALATE to user through build-management

### Escalation Report

```markdown
## COMPLETENESS ESCALATION

### Persistent Findings
| ID | Category | File | Pattern | Cycle 1 | Cycle 2 |
|----|----------|------|---------|---------|---------|
| CCC-001 | Behavioral | src/order/service.ts:42 | Empty function body | Found | Still present |
| CCC-005 | Structural | src/payment/ | Module missing entirely | Found | Still missing |

### Remediation History
- **Cycle 1**: [N] findings reported, [M] addressed by bob-the-builder
- **Cycle 2**: [N] findings reported (including [K] new), [M] addressed

### Recommendation
[Suggest user intervention: clarify requirements, reduce scope, or provide guidance]
```
