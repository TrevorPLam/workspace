# 6. Testing Structure

**Prerequisites:** 
- [Section 2: Source Code Layout](02-Source-Code-Layout.md)
- [Section 5: CI/CD Structure](05-CI-CD-Structure.md) - Phase 1 (bootstrap CI/CD) should be complete

## Questions to Answer

Before proceeding, answer these questions:

1. **What testing framework will you use?**
   - [ ] Jest (JavaScript/TypeScript)
   - [ ] pytest (Python)
   - [ ] Go testing (built-in)
   - [ ] Cargo test (Rust)
   - [ ] JUnit (Java)
   - [ ] Other: _______________

2. **What types of tests do you need?**
   - [ ] Unit tests (individual functions/components)
   - [ ] Integration tests (component interactions)
   - [ ] End-to-end tests (full user workflows)
   - [ ] Performance tests (load, stress, benchmarks)
   - [ ] Accessibility tests (a11y for web apps)
   - [ ] Other: _______________

**Document your answers** in `alignment-progress.json` before proceeding.

**Effort Estimate:**
- **New test infrastructure:** 1-2 hours
- **Organizing existing tests:** 2-4 hours
- **Setting up test coverage:** 1-2 hours additional
- **Full test suite setup (unit + integration + e2e):** 4-8 hours

## P0 — Required Actions

**Action:** Set up testing infrastructure
- [ ] Choose and document test pattern:
  - Co-located: `Button.tsx` → `Button.test.tsx`
  - Centralized: `tests/` mirroring source structure
- [ ] Install test framework (Jest, pytest, Go testing, etc.)
- [ ] Create first test file to verify setup
- [ ] Ensure all critical paths have tests (aim for 60%+ coverage)
- [ ] Verify tests pass locally
- [ ] **Note:** CI/CD test step will be added in [Section 5 Phase 2](05-CI-CD-Structure.md) after this section is complete

## P1 — Recommended Actions

**Action:** Organize test structure
- [ ] Separate test categories:
  - `tests/unit/` - Unit tests
  - `tests/integration/` - Integration tests
  - `tests/e2e/` - End-to-end tests
- [ ] Create shared test utilities (`tests/fixtures/`, `tests/mocks/`)
- [ ] For web apps: add accessibility testing (a11y checks)
- [ ] Configure coverage thresholds in test config

**Action:** Define performance budgets
- [ ] Set performance budgets for:
  - Bundle size limits (for web apps, e.g., max 500KB initial bundle)
  - Lighthouse scores (Performance ≥90, Accessibility ≥95, Best Practices ≥90, SEO ≥90)
  - API response times (p50 <200ms, p95 <500ms, p99 <1000ms)
  - Database query times (p95 <100ms for simple queries)
  - Page load times (for web apps, e.g., First Contentful Paint <1.5s)
- [ ] Configure CI checks to enforce budgets:
  - Lighthouse CI for web apps (see [Section 5 P1](05-CI-CD-Structure.md))
  - Bundle size checks in build process
  - API performance tests in test suite
- [ ] Document budgets in `docs/performance.md`
- [ ] Set up alerts for budget violations (see [Section 11](11-Observability-Monitoring.md))
- [ ] Review and update budgets quarterly or when architecture changes

## P2 — Advanced Actions

**Action:** Advanced testing
- [ ] Add contract testing (OpenAPI/GraphQL schemas)
- [ ] Create `tests/performance/` directory for load testing
- [ ] Implement flaky test quarantine and retry logic
- [ ] Add mutation testing for critical components
- [ ] Consider property-based testing where applicable

## Decision: Which test pattern should you use?

**Co-located Pattern (Recommended for most projects):**
- Tests next to source: `Button.tsx` → `Button.test.tsx`
- Easier to find and maintain
- Good for: Components, modules, focused units

**Centralized Pattern:**
- Tests in `tests/` directory mirroring source structure
- Separates test code from production code
- Good for: Large projects, strict separation needs

**Action:** Choose and document your pattern
- [ ] Decide on test pattern (co-located or centralized)
- [ ] Document pattern choice in `docs/testing.md` or README
- [ ] Create first test file following chosen pattern

## Verification

**Manual Check:**
- [ ] Verify test framework is installed and configured
- [ ] Run tests locally: `npm test`, `pytest`, `go test`, etc.
- [ ] Check test coverage: Aim for 60%+ on critical paths
- [ ] Verify test pattern is consistent across project
- [ ] If Phase 2 CI/CD complete: Verify tests run in CI pipeline
- [ ] Check test documentation exists (how to run, what's tested)

**Automated Check (Future):**
```bash
./scripts/validate-section-6.sh
```

**Done Criteria:**
- ✅ Test framework installed and configured
- ✅ Tests run successfully locally
- ✅ Test pattern chosen and documented
- ✅ Critical paths have test coverage (60%+)
- ✅ Tests integrated into CI/CD (if Phase 2 complete)

## Common Issues & Solutions

**Issue:** "I don't know which test framework to use"  
- **Solution:** 
  - JavaScript/TypeScript: Jest (most popular), Vitest (faster)
  - Python: pytest (recommended), unittest (built-in)
  - Go: Built-in `testing` package
  - Rust: Built-in `#[test]` attributes
  - Check language-specific best practices

**Issue:** "My tests aren't running in CI"  
- **Solution:** 
  - Verify test step is added to CI/CD config (Section 5 Phase 2)
  - Check test command matches local: `npm test` vs `npm run test`
  - Ensure test framework is installed in CI environment
  - Check CI logs for test execution errors

**Issue:** "I don't know what 'critical paths' means"  
- **Solution:** 
  - Critical paths = core functionality that must work
  - Examples: Authentication, payment processing, data validation
  - Focus tests on: User-facing features, business logic, error handling
  - Don't worry about testing every utility function

**Issue:** "My test coverage is below 60%"  
- **Solution:** 
  - Start with critical paths first (get those to 80%+)
  - Add tests incrementally (don't try to reach 60% all at once)
  - Use coverage tools to identify untested code: `jest --coverage`, `pytest-cov`
  - Focus on quality over quantity (better to have fewer, better tests)

**Issue:** "I don't know if I should use co-located or centralized tests"  
- **Solution:** 
  - Start with co-located (easier to maintain)
  - Switch to centralized if: Tests become hard to find, or team prefers separation
  - Document your choice so team follows the pattern consistently

**Issue:** "Performance budgets seem complicated"  
- **Solution:** 
  - Start simple: Set bundle size limit for web apps
  - Add more budgets as you identify performance issues
  - Use tools: Lighthouse CI, bundle-size-limit, webpack-bundle-analyzer
  - Review budgets quarterly (they're not set in stone)
