# 7. Documentation Standards

**Prerequisites:** 
- [Section 0: Prerequisites](00-Prerequisites.md) - Complete decision points
- [Section 2: Source Code Layout](02-Source-Code-Layout.md) - Complete source structure before documenting it

## P0 — Required Actions

**Action:** Create essential documentation
- [ ] Write `README.md` that answers in under 1 minute:
  - What the repo is
  - How to install/run it
  - Where the code lives (reference [Section 2](02-Source-Code-Layout.md) structure)
  - How to contribute and get help
- [ ] Add `LICENSE` file (mandatory for public repos, optional for private)

See [Templates](Templates.md) for README template.

## P1 — Recommended Actions

**Action:** Expand documentation
- [ ] Ensure `docs/` folder exists (should be created in [Section 1 P1](01-Root-Directory-Structure.md))
- [ ] Populate `docs/` folder with structure:
  - `docs/architecture.md` - System design
  - `docs/setup.md` - Detailed setup instructions
  - `docs/operations.md` - Operational procedures
  - `docs/onboarding.md` - Developer onboarding guide (see below)
- [ ] Add sub-READMEs in major directories (`src/README.md`, `infra/README.md`)
- [ ] Include diagrams (Mermaid or images) for complex systems
- [ ] For APIs: maintain OpenAPI/Swagger specs
- [ ] Ensure accessibility: alt text for images, screen reader compatibility

**Action:** Create developer onboarding documentation
- [ ] Create `docs/onboarding.md` with:
  - Prerequisites: Required tools, versions, accounts, access requests
  - Local development setup: Step-by-step instructions to get running
  - First contribution walkthrough: End-to-end example (fork, branch, change, PR)
  - Common issues and solutions: Troubleshooting guide for frequent problems
  - Team communication: Channels, meeting times, key contacts
  - Code style and conventions: Quick reference to standards
  - Where to get help: Links to docs, Slack channels, etc.
- [ ] Link from README.md "Getting Started" or "Contributing" section
- [ ] Keep onboarding guide updated with codebase and process changes
- [ ] Consider adding a "Day 1" checklist for new developers

**Action:** Document API versioning strategy (for API projects)
- [ ] Choose versioning scheme:
  - URL versioning: `/api/v1/`, `/api/v2/` (recommended for REST APIs)
  - Header versioning: `Accept: application/vnd.api+json;version=1` (for content negotiation)
- [ ] Document deprecation policy:
  - Deprecation notice period (typically 6-12 months before removal)
  - Breaking change communication process (changelog, release notes, announcements)
  - Migration guides for major versions
- [ ] Maintain version compatibility matrix in `docs/api.md` or `docs/architecture.md`
- [ ] Link to OpenAPI/Swagger specs (already in P1 above)
- [ ] Document versioning decisions in ADRs if significant (see P2 below)

**Action:** Implement FAIR Principles (Findable, Accessible, Interoperable, Reusable)
- [ ] **Findable:**
  - [ ] Clear metadata in README (description, keywords, tags)
  - [ ] Standardized naming conventions
  - [ ] Repository description and topics configured
  - [ ] Persistent identifiers (DOI) if applicable
- [ ] **Accessible:**
  - [ ] Open licensing (LICENSE file)
  - [ ] Clear access procedures documented
  - [ ] Authentication/authorization documented
  - [ ] API access documented
- [ ] **Interoperable:**
  - [ ] Standard formats (JSON, YAML, OpenAPI)
  - [ ] API specifications (OpenAPI/Swagger)
  - [ ] Workflow descriptors (CWL, WDL, Nextflow)
  - [ ] Container specifications (Dockerfile)
- [ ] **Reusable:**
  - [ ] Clear licensing terms
  - [ ] Comprehensive documentation
  - [ ] Reproducibility guides
  - [ ] Usage examples
- [ ] Create FAIR metadata file:
  - [ ] ro-crate.json (RO-Crate format)
  - [ ] schema.org metadata
  - [ ] JSON-LD structured data
- [ ] Document FAIR compliance in `docs/fair-compliance.md`
- [ ] **Note:** Especially important for research/data repositories

**Action:** Set up automated documentation generation
- [ ] Configure API documentation generation:
  - [ ] Generate from OpenAPI/Swagger specs
  - [ ] Use tools: Swagger UI, Redoc, or custom generator
  - [ ] Host generated docs (GitHub Pages, Netlify, etc.)
  - [ ] Auto-update on spec changes
- [ ] Configure code documentation generation:
  - [ ] JSDoc/TSDoc for JavaScript/TypeScript
  - [ ] GoDoc for Go
  - [ ] Sphinx for Python
  - [ ] Rustdoc for Rust
- [ ] Auto-update README:
  - [ ] Badge updates (build status, coverage, etc.)
  - [ ] Dependency version badges
  - [ ] Metrics and statistics
- [ ] Generate architecture diagrams:
  - [ ] From code structure
  - [ ] From dependency graphs
  - [ ] From API specifications
- [ ] CI job for documentation:
  - [ ] Validate documentation freshness
  - [ ] Rebuild docs on changes
  - [ ] Deploy updated docs
- [ ] Document process in `docs/automated-docs.md`

## P2 — Advanced Actions

**Action:** Advanced documentation
- [ ] Create ADRs in `docs/adr/` following format: `####-decision-name.md`
- [ ] Add runbooks for operations and incident response
- [ ] Set up versioned documentation system (Docusaurus, MkDocs, etc.)
- [ ] Follow language-specific comment conventions (JSDoc, docstrings, etc.)

## Decision: What documentation do you need?

**From Section 0, determine your project type:**

**Library/Package:**
- [ ] README with installation and usage
- [ ] API documentation (JSDoc, GoDoc, etc.)
- [ ] Examples in `examples/` directory

**Application:**
- [ ] README with setup and run instructions
- [ ] Architecture documentation (`docs/architecture.md`)
- [ ] Developer onboarding guide (`docs/onboarding.md`)

**API/Service:**
- [ ] README with API overview
- [ ] OpenAPI/Swagger specification
- [ ] API versioning documentation

**Infrastructure:**
- [ ] README with deployment instructions
- [ ] Infrastructure documentation (`infra/README.md`)
- [ ] Environment setup guide

## Verification

**Manual Check:**
- [ ] Verify `README.md` exists and contains all required sections (what, install, run, contribute)
- [ ] Check `LICENSE` file exists (if public repo)
- [ ] If `docs/` folder exists: verify it has clear structure
- [ ] Test README instructions: Can someone clone and run the project?
- [ ] Check for broken links: Use link checker tool
- [ ] Verify documentation is up-to-date (no outdated instructions)

**Automated Check (Future):**
```bash
./scripts/validate-section-7.sh
```

**Done Criteria:**
- ✅ README.md exists and is complete
- ✅ LICENSE file exists (if public)
- ✅ Documentation is accurate and up-to-date
- ✅ No broken links in documentation
- ✅ Developer onboarding guide exists (if P1 completed)

## Common Issues & Solutions

**Issue:** "My README is too long"  
- **Solution:** 
  - Keep README concise (under 1 minute read)
  - Move detailed docs to `docs/` folder
  - Use README as entry point with links to detailed docs

**Issue:** "I don't know what to put in README"  
- **Solution:** Use the template from [Templates.md](Templates.md)
- Must have: What it is, how to install, how to run, where code lives, how to contribute
- Should have: Badges, examples, screenshots (if applicable)

**Issue:** "docs/ folder doesn't exist"  
- **Solution:** 
  - Create it: `mkdir docs` (or `mkdir docs\` on Windows)
  - This is a P1 action, so it's okay to skip initially
  - Add it when you have more than README to document

**Issue:** "I don't know if I need API documentation"  
- **Solution:** 
  - If you have an API (REST, GraphQL, etc.) → Yes, document it
  - Use OpenAPI/Swagger for REST APIs
  - Use GraphQL schema documentation for GraphQL
  - If no API → Skip API-specific documentation

**Issue:** "My documentation is outdated"  
- **Solution:** 
  - Set up automated doc generation (P1 action)
  - Add documentation review to PR checklist
  - Update docs when code changes
  - Use "last updated" dates in docs

**Issue:** "I don't know what FAIR principles are"  
- **Solution:** 
  - FAIR = Findable, Accessible, Interoperable, Reusable
  - Important for research/data repositories
  - For most software projects, focus on basic documentation first
  - FAIR is P1/P2, so skip if not applicable