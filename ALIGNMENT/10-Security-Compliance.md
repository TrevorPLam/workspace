# 10. Security & Compliance

**Prerequisites:** 
- [Section 0: Prerequisites](00-Prerequisites.md) - Complete decision points
- [Section 3: Configuration & Environment](03-Configuration-Environment.md)
- [Section 4: Dependency Management](04-Dependency-Management.md)
- [Section 5: CI/CD Structure](05-CI-CD-Structure.md) - Phase 1 (bootstrap) should be complete

## Questions to Answer

Before proceeding, answer these questions:

1. **What is your security compliance level?**
   - [ ] Basic (P0 requirements only)
   - [ ] Standard (P0 + P1 requirements)
   - [ ] High (P0 + P1 + P2 requirements)
   - [ ] Compliance-specific (SOC2, HIPAA, PCI-DSS, etc.)

2. **Do you handle sensitive data (PII, secrets, credentials)?**
   - [ ] Yes (requires enhanced security measures)
   - [ ] No (standard security sufficient)

**Note:** If secrets exist in repository (from Section 0 Question 9 or Section 3 Question 1), they must be remediated before proceeding.

**Document your answers** in `alignment-progress.json` before proceeding.

## P0 — Required Actions

**Action:** Implement security basics
- [ ] Create `SECURITY.md` with:
  - Vulnerability disclosure process
  - Security contact information
  - Supported versions
- [ ] Configure automated secret scanning (should already be in CI/CD from [Section 5](05-CI-CD-Structure.md)):
  - [ ] Pre-commit hooks (e.g., git-secrets, truffleHog)
  - [ ] CI pipeline scanning
- [ ] Set up dependency vulnerability scanning with SLAs (should already be in CI/CD from [Section 5](05-CI-CD-Structure.md)):
  - Critical (CVSS 9.0+): 7 days response
  - High (CVSS 7.0-8.9): 30 days response
  - Medium/Low: Tracked in maintenance cycles
- [ ] Configure license compliance checking
- [ ] For projects with releases: generate SBOM (SPDX or CycloneDX format)

See [Templates](Templates.md) for SECURITY.md template.

## P1 — Recommended Actions

**Action:** Enhance security
- [ ] Add security review gates for:
  - Infrastructure changes
  - Sensitive code changes
- [ ] For containerized apps: add container image scanning
- [ ] For web apps: add DAST (Dynamic Application Security Testing)
- [ ] Implement security policy enforcement (OPA policies for infra)
- [ ] Schedule regular security audits and dependency reviews

## P2 — Advanced Actions

**Action:** Advanced security
- [ ] Set up automated compliance scanning (export controls, GDPR, etc.)
- [ ] Create security metrics dashboard
- [ ] Document threat modeling
- [ ] Create security incident response runbooks
- [ ] Integrate penetration testing

**Action:** Achieve SLSA Compliance (Supply Chain Security)
- [ ] Understand SLSA levels (Level 1-4):
  - Level 1: Scripted builds, provenance available
  - Level 2: Version controlled, hosted source, ephemeral environment
  - Level 3: Non-falsifiable provenance, authenticated build
  - Level 4: Two-person review, hardened build
- [ ] Generate build attestations:
  - [ ] Use in-toto attestations format
  - [ ] Include build provenance (source, dependencies, environment)
  - [ ] Sign attestations cryptographically
  - [ ] Store attestations in artifact registry
- [ ] Verify source authenticity:
  - [ ] Require signed commits (GPG signatures)
  - [ ] Verify commit signatures in CI/CD
  - [ ] Document source verification process
- [ ] Track build environment:
  - [ ] Document build environment (OS, tools, versions)
  - [ ] Use reproducible build tools
  - [ ] Lock build dependencies
- [ ] Document SLSA level achieved in `docs/slsa-compliance.md`:
  - [ ] Current SLSA level
  - [ ] Attestation generation process
  - [ ] Verification procedures
  - [ ] Roadmap to higher levels
- [ ] Integrate with CI/CD (Section 5):
  - [ ] Generate attestations in build pipeline
  - [ ] Verify attestations before deployment
  - [ ] Store attestations with artifacts
- [ ] **Note:** SLSA Level 3+ recommended for production systems

See [Templates](Templates.md) for SLSA Compliance Documentation template.

## Decision: What security level do you need?

**Assess your security requirements:**

**Basic Security (All Projects):**
- [ ] SECURITY.md with disclosure process
- [ ] Secret scanning (pre-commit + CI)
- [ ] Dependency vulnerability scanning
- [ ] License compliance checking

**Enhanced Security (Production Apps):**
- [ ] Security review gates
- [ ] Container image scanning (if using containers)
- [ ] DAST for web apps
- [ ] Regular security audits

**Advanced Security (High-Stakes Systems):**
- [ ] SLSA Level 3+ compliance
- [ ] Automated compliance scanning
- [ ] Threat modeling
- [ ] Penetration testing

**Action:** Implement security appropriate to your project
- [ ] Start with basic security (P0)
- [ ] Add enhanced security for production (P1)
- [ ] Consider advanced security for critical systems (P2)

## Verification

**Manual Check:**
- [ ] Verify `SECURITY.md` exists with disclosure process
- [ ] Check secret scanning is configured (pre-commit + CI)
- [ ] Verify dependency scanning runs in CI/CD
- [ ] Check license compliance is validated
- [ ] If releases: Verify SBOM generation (if P0 completed)
- [ ] Test secret scanning: Try committing a test secret (should be blocked)

**Automated Check (Future):**
```bash
./scripts/validate-section-10.sh
```

**Done Criteria:**
- ✅ SECURITY.md exists and is complete
- ✅ Secret scanning configured and working
- ✅ Dependency vulnerability scanning configured
- ✅ License compliance checking configured
- ✅ No secrets committed to repository

## Common Issues & Solutions

**Issue:** "I don't know what SECURITY.md should contain"  
- **Solution:** 
  - Use template from [Templates.md](Templates.md)
  - Must have: Vulnerability disclosure process, security contact, supported versions
  - Add: Response SLAs, reporting instructions
  - Keep it simple for small projects

**Issue:** "Secret scanning isn't finding secrets"  
- **Solution:** 
  - Use multiple tools: git-secrets (pre-commit) + truffleHog (CI)
  - Configure custom patterns for your secret formats
  - Scan git history: `trufflehog git file://. --json`
  - Rotate any found secrets immediately

**Issue:** "I have too many dependency vulnerabilities"  
- **Solution:** 
  - Start with critical vulnerabilities first (CVSS 9.0+)
  - Update dependencies incrementally
  - Use Dependabot/Renovate for automated updates
  - Document why vulnerabilities can't be fixed (if applicable)

**Issue:** "I don't know if I need SLSA compliance"  
- **Solution:** 
  - SLSA = Supply Chain Levels for Software Artifacts
  - Required for: Production systems, published packages, critical infrastructure
  - Start with SLSA Level 1 (scripted builds)
  - Work toward Level 3+ for production
  - This is P2, so skip if not needed yet

**Issue:** "License compliance checking is complicated"  
- **Solution:** 
  - Use automated tools: license-checker (npm), pip-licenses (Python)
  - Focus on incompatible licenses (GPL if your project isn't GPL)
  - Document license decisions in `docs/licenses.md`
  - Most permissive licenses (MIT, Apache) are compatible with everything