# 13. Quality & Health Checks

**Prerequisites:** 
- [Section 0: Prerequisites](00-Prerequisites.md) - Complete decision points
- All P0 sections (1-12) should be completed first

**Effort Estimate:**
- **Basic quality checks (P0):** 1-2 hours
- **Automated health scoring (P1):** 4-8 hours
- **Compliance validation (P1):** 2-4 hours additional
- **DORA metrics tracking (P2):** 4-8 hours additional

## Questions to Answer

Before proceeding, answer these questions:

1. **Do you have automated health checks?**
   - [ ] Yes (needs alignment with ALIGNMENT standards)
   - [ ] No (needs setup - P1)

2. **What quality metrics do you track?**
   - [ ] Test coverage
   - [ ] Code complexity
   - [ ] Dependency health
   - [ ] Documentation coverage
   - [ ] None (needs setup)

3. **Do you have compliance validation automated?**
   - [ ] Yes (needs alignment)
   - [ ] No (needs setup - P1)

**Document your answers** in `alignment-progress.json` before proceeding.

## P0 — Required Actions

**Action:** Ensure basic quality
- [ ] Verify repo builds from scratch using documented instructions
- [ ] Ensure all tests pass on CI (tests from [Section 6](06-Testing-Structure.md), CI from [Section 5](05-CI-CD-Structure.md))
- [ ] Check for broken links (use link checker)
- [ ] Verify no missing directories referenced in docs
- [ ] Test fresh clone and build process

## P1 — Recommended Actions

**Action:** Implement automated repository health scoring
- [ ] Create health scoring system:
  - [ ] Define scoring criteria (0-100 scale):
    - P0 requirements: 40 points (all required)
    - P1 requirements: 30 points (recommended)
    - P2 requirements: 20 points (advanced)
    - Documentation quality: 10 points
  - [ ] Weight scores by priority (P0 > P1 > P2)
- [ ] Implement automated health checker:
  - [ ] Script or tool that validates standards compliance
  - [ ] Check for required files (README, LICENSE, SECURITY.md, etc.)
  - [ ] Validate structure (directories exist, naming conventions)
  - [ ] Check documentation completeness
  - [ ] Verify CI/CD configuration
  - [ ] Check test coverage (if applicable)
- [ ] Generate health report:
  - [ ] Calculate overall health score
  - [ ] List missing requirements
  - [ ] Provide remediation suggestions
  - [ ] Generate visual badge (shields.io, custom)
- [ ] Integrate with CI/CD (Section 5):
  - [ ] Run health check in CI pipeline
  - [ ] Block merges if health score drops below threshold (e.g., <70)
  - [ ] Post health score as PR comment
- [ ] Track health over time:
  - [ ] Store health scores historically
  - [ ] Create health trends dashboard
  - [ ] Alert on health degradation
- [ ] Document in `docs/repository-health.md`:
  - [ ] Scoring methodology
  - [ ] Health check configuration
  - [ ] Remediation guide

**Action:** Set up automated compliance validation
- [ ] Create compliance validation CI job:
  - [ ] Validate required files exist (README, LICENSE, SECURITY.md, etc.)
  - [ ] Check file structure matches standards
  - [ ] Verify naming conventions
  - [ ] Validate documentation completeness
  - [ ] Check CI/CD configuration correctness
  - [ ] Verify security scanning configured
- [ ] Configure validation rules:
  - [ ] P0 requirements: Blocking (must pass)
  - [ ] P1 requirements: Warning (should pass)
  - [ ] P2 requirements: Info (nice to have)
- [ ] Generate compliance report:
  - [ ] List passed checks
  - [ ] List failed checks with remediation steps
  - [ ] Provide compliance score
- [ ] Integrate with PR process:
  - [ ] Run validation on PR creation/update
  - [ ] Post compliance report as PR comment
  - [ ] Block merge if P0 requirements fail
- [ ] Document validation process in `docs/compliance-validation.md`

See [Templates](Templates.md) for Repository Health Check Script template.

## P2 — Advanced Actions

**Action:** Track DORA Metrics (Developer Productivity)
- [ ] **Deployment Frequency:**
  - [ ] Track deployments from CI/CD (Section 5)
  - [ ] Calculate frequency (daily, weekly, monthly)
  - [ ] Set targets (elite: multiple per day)
- [ ] **Lead Time for Changes:**
  - [ ] Measure commit to production time
  - [ ] Track PR creation to merge time
  - [ ] Set targets (elite: <1 hour)
- [ ] **Mean Time to Recovery (MTTR):**
  - [ ] Track incident resolution time (Section 11)
  - [ ] Calculate average recovery time
  - [ ] Set targets (elite: <1 hour)
- [ ] **Change Failure Rate:**
  - [ ] Track failed deployments
  - [ ] Calculate failure percentage
  - [ ] Set targets (elite: <15%)
- [ ] Create DORA metrics dashboard
- [ ] Document DORA metrics tracking in `docs/dora-metrics.md`

**Action:** Integration Testing Across ALIGNMENT Sections
- [ ] **Test Section Integration:**
  - [ ] Verify CI/CD (Section 5) runs tests from Section 6
  - [ ] Verify security scanning (Section 10) works with CI/CD (Section 5)
  - [ ] Verify pre-commit hooks (Section 3) enforce commit conventions (Section 12)
  - [ ] Verify documentation (Section 7) references correct structure (Section 1-2)
  - [ ] Verify health checks (Section 13) validate all previous sections
- [ ] **End-to-End Validation:**
  - [ ] Create test repository following all P0 sections
  - [ ] Verify fresh clone → build → test → deploy works
  - [ ] Verify all validation scripts pass
  - [ ] Verify all documentation links work
- [ ] **Cross-Section Dependencies:**
  - [ ] Document which sections depend on others
  - [ ] Verify prerequisite sections are complete before proceeding
  - [ ] Test that skipping optional sections doesn't break required ones
- [ ] **Automated Integration Tests:**
  - [ ] Create CI job that validates ALIGNMENT compliance end-to-end
  - [ ] Run validation scripts for all implemented sections
  - [ ] Check that all required files exist and are correct
  - [ ] Verify no circular dependencies between sections
- [ ] **Document Integration Points:**
  - [ ] Create `docs/integration-testing.md` describing how sections work together
  - [ ] Document known integration issues and workarounds
  - [ ] Provide integration test checklist

## Core Principles

Apply these principles to every decision:

- ✅ **Intentional**: Everything must have a clear purpose. No guesswork.
- ✅ **Discoverable**: Everything must be findable in under 1 minute.
- ✅ **Documented**: Everything must be documented at the appropriate level for the repo.
- ✅ **Consistent**: Everything must follow consistent patterns across the project.

## Decision: What quality checks do you need?

**Assess your requirements:**

**Basic Quality (All Projects):**
- [ ] Repository builds from scratch
- [ ] All tests pass
- [ ] No broken links in documentation
- [ ] Fresh clone and build works

**Automated Health Scoring (Recommended):**
- [ ] Health scoring system (0-100 scale)
- [ ] Automated health checker script
- [ ] Health report generation
- [ ] CI/CD integration

**Compliance Validation (Production):**
- [ ] Compliance validation CI job
- [ ] PR compliance checks
- [ ] Compliance reporting

**DORA Metrics (Advanced):**
- [ ] Deployment frequency tracking
- [ ] Lead time measurement
- [ ] MTTR tracking
- [ ] Change failure rate tracking

**Action:** Implement quality checks appropriate to your project
- [ ] Start with basic quality (P0)
- [ ] Add automated health scoring (P1)
- [ ] Consider compliance validation for production (P1)
- [ ] Add DORA metrics for advanced teams (P2)

## Verification

**Manual Check:**
- [ ] Test fresh clone: `git clone <repo> && cd <repo> && <build-command>`
- [ ] Verify all tests pass: Run test suite locally
- [ ] Check for broken links: Use link checker (markdown-link-check, etc.)
- [ ] Verify no missing directories referenced in docs
- [ ] If P1 completed: Run health check script and verify score
- [ ] If P1 completed: Check compliance validation runs in CI

**Automated Check (Future):**
```bash
./scripts/validate-section-13.sh
```

**Done Criteria:**
- ✅ Repository builds from scratch successfully
- ✅ All tests pass locally and on CI
- ✅ No broken links in documentation
- ✅ Health check script runs successfully (if P1 completed)
- ✅ Compliance validation configured (if P1 completed)

## Common Issues & Solutions

**Issue:** "Repository doesn't build from scratch"  
- **Solution:** 
  - Test: Clone to fresh directory and try to build
  - Check README instructions are complete and accurate
  - Verify all dependencies are documented
  - Fix any missing steps or dependencies
  - Document the complete setup process

**Issue:** "Tests pass locally but fail in CI"  
- **Solution:** 
  - Check Node.js/Python/Go version matches CI
  - Verify environment variables are set in CI
  - Check for platform-specific issues (Windows vs Linux)
  - Ensure all dependencies are in lockfile
  - Test in Docker container to match CI environment

**Issue:** "I have broken links in documentation"  
- **Solution:** 
  - Use link checker: `markdown-link-check README.md`
  - Fix broken links or remove them
  - Add link checking to CI/CD (prevent future breaks)
  - Check external links periodically (they can break over time)

**Issue:** "Health scoring seems complicated"  
- **Solution:** 
  - Start with simple checks: Required files exist, structure is correct
  - Use the template script from [Templates.md](Templates.md)
  - Add more checks incrementally
  - Focus on P0 requirements first (40 points), then P1 (30 points)

**Issue:** "I don't know what DORA metrics are"  
- **Solution:** 
  - DORA = DevOps Research and Assessment
  - Four metrics: Deployment frequency, lead time, MTTR, change failure rate
  - Used to measure team performance
  - This is P2 (advanced), skip if not needed
  - Focus on basic quality first, add metrics later