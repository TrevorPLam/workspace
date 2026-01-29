# ALIGNMENT Standards - Glossary & Concept Explanations

**Purpose:** Beginner-friendly explanations of technical terms used throughout ALIGNMENT standards  
**Status:** Template - Ready for content population  
**Maintenance:** Update as new concepts are introduced

---

## A

### API (Application Programming Interface)
**Simple:** A set of rules that lets different programs talk to each other and request information.  
**Why it matters:** When your app needs to use another service (like a payment processor), you use its API.  
**Example:** Your weather app uses the weather service's API to get today's forecast. The API says "send me your location, and I'll send you weather data."  
**Related Sections:** 05 (CI/CD - API endpoints), 07 (Documentation - API docs)

### AGPL (Affero General Public License)
**Simple:** A very strict open-source license that requires you to share any changes, even for online services.  
**Why it matters:** If you use AGPL code, your own code might have to be AGPL too - potentially limiting your options.  
**Example:** If you use an AGPL library and someone uses your service online, they can request your source code.  
**Related Sections:** 04 (Dependency Management - license checking), 10 (Security - compliance)

---

## B

### Branch (Git)
**Simple:** A separate "copy" of your code where you can make changes without affecting the main version.  
**Why it matters:** Lets multiple people work on different features at the same time without conflicts.  
**Example:** You create a "feature/login" branch to add login functionality. Meanwhile, someone else works on "feature/payments" branch. Both are safe until they're ready to merge.  
**Related Sections:** 09 (Governance - branching strategy), 12 (Change Management - breaking changes)

### Breaking Change
**Simple:** A change to your code that makes existing user code stop working.  
**Why it matters:** Breaking changes cause pain for users who depend on your code. They need warning and time to adapt.  
**Example:** If you rename a function from `getUser()` to `fetchUser()`, anyone using `getUser()` gets an error. That's breaking.  
**Related Sections:** 12 (Change Management - semantic versioning), 07 (Documentation - migration guides)

### Build (Software)
**Simple:** The process of converting your source code into something runnable (executable program, website, etc.).  
**Why it matters:** Your code doesn't run directly - it must be "built" first (compiled, bundled, processed).  
**Example:** JavaScript code is bundled and minified. Go code is compiled into an executable. Python code is usually interpreted without a separate build step.  
**Related Sections:** 05 (CI/CD - build step), 01 (Root Directory - build artifacts)

---

## C

### CHANGELOG.md
**Simple:** A file listing all changes made to your project, organized by version.  
**Why it matters:** Users and developers need to know what changed between versions and if it affects them.  
**Example:** "Version 2.0.0: Breaking change - removed deprecated getUser() function. Use fetchUser() instead."  
**Related Sections:** 12 (Change Management), 01 (Root Directory - required files)

### CI/CD (Continuous Integration / Continuous Deployment)
**Simple:** Automated systems that run tests, checks, and deployments whenever you push code.  
**Why it matters:** Catches bugs early, ensures quality, and automates repetitive work.  
**Example:** You push code → tests run automatically → if tests pass, code automatically deploys to production.  
**Related Sections:** 05 (CI/CD Structure), 13 (Quality checks - CI integration)

### CODEOWNERS
**Simple:** A file that specifies who must review changes to specific parts of the code.  
**Why it matters:** Ensures the right experts review sensitive areas and decisions are documented.  
**Example:** `backend/` → `@alice`, `frontend/` → `@bob`. When someone changes backend code, Alice must approve.  
**Related Sections:** 09 (Governance), 01 (Root Directory - .github folder)

### Commit (Git)
**Simple:** A saved snapshot of your code changes with a message explaining what changed.  
**Why it matters:** Creates a history of changes so you can see who changed what and why.  
**Example:** `git commit -m "fix: handle null user response"` saves your change with an explanation.  
**Related Sections:** 12 (Change Management - conventional commits)

### Conventional Commits
**Simple:** A standard format for commit messages that makes them machine-readable.  
**Why it matters:** Enables automated changelog generation and clear history reading.  
**Example:** `feat(auth): add OAuth2 support` vs just `update auth stuff` - the first is clear and parseable.  
**Related Sections:** 12 (Change Management)

---

## D

### DORA Metrics
**Simple:** Four measurements of software team productivity and quality (Deployment Frequency, Lead Time, MTTR, Change Failure Rate).  
**Why it matters:** Helps teams identify bottlenecks and improvement areas objectively.  
**Example:** If your deployment frequency is monthly but your team wants weekly, DORA metrics help you see that as a goal.  
**Related Sections:** 13 (Quality Health Checks - DORA metrics)

### Dependency
**Simple:** An external library or package your code uses.  
**Why it matters:** Dependencies can break, have security vulnerabilities, or conflict with other dependencies.  
**Example:** A Python project might depend on "requests" library to make web requests. If "requests" has a security bug, your project is affected.  
**Related Sections:** 04 (Dependency Management), 10 (Security - vulnerability scanning)

---

## E

### Environment Variable
**Simple:** A named value your program reads from the system (not hardcoded in code).  
**Why it matters:** Lets you use different values for different environments (dev/staging/production) without changing code.  
**Example:** `DATABASE_URL=localhost` on your computer, `DATABASE_URL=prod-db.example.com` in production - same code, different environment variable.  
**Related Sections:** 03 (Configuration & Environment)

---

## F

### FAIR Principles
**Simple:** Four criteria for good research/data repositories: Findable, Accessible, Interoperable, Reusable.  
**Why it matters:** Makes your work easier to discover, use, and build upon.  
**Example:** If someone searches for "user authentication", can they find your repo? (Findable)  
**Related Sections:** 07 (Documentation Standards - FAIR principles)

---

## G

### Git
**Simple:** A system for tracking changes to files over time, letting teams collaborate and revert changes if needed.  
**Why it matters:** Without version control, you can't safely collaborate or recover from mistakes.  
**Example:** You write code → commit it → push to GitHub → teammates pull your changes → they build on it.  
**Related Sections:** Throughout all sections

### GitHub Actions
**Simple:** GitHub's built-in system for automating tasks when code is pushed or PRs are created.  
**Why it matters:** Lets you run tests, security checks, deployments automatically without extra tools.  
**Example:** "When someone opens a PR, automatically run tests and lint checks."  
**Related Sections:** 05 (CI/CD Structure - GitHub implementation)

### GitLab CI
**Simple:** GitLab's built-in system for automating tasks (similar to GitHub Actions).  
**Why it matters:** If you use GitLab instead of GitHub, this is your automation system.  
**Example:** Similar to GitHub Actions but configured in `.gitlab-ci.yml`.  
**Related Sections:** 05 (CI/CD Structure - GitLab implementation)

---

## H

### Health Check
**Simple:** A test that verifies your application is working and responding correctly.  
**Why it matters:** Monitoring systems use health checks to know if services are up and responsive.  
**Example:** An API exposes `/health` endpoint that returns `{"status": "ok"}` if everything is working.  
**Related Sections:** 11 (Observability), 13 (Quality Health Checks)

---

## I

### Import Path
**Simple:** The "address" you use to load code from another file or package in your code.  
**Why it matters:** If you move files around, import paths can break. Understanding this helps with refactoring.  
**Example:** `from utils.helpers import get_user` or `import { getUser } from '@/utils/helpers'` - the path tells the program where to find the code.  
**Related Sections:** 02 (Source Code Layout)

### Infrastructure as Code (IaC)
**Simple:** Writing code (Terraform, Kubernetes manifests) that describes your servers, networks, databases - instead of clicking buttons to set them up manually.  
**Why it matters:** Makes infrastructure repeatable, version-controlled, and reviewable like application code.  
**Example:** Terraform code that says "create an AWS database with these settings" instead of manually clicking AWS console buttons.  
**Related Sections:** 08 (Infrastructure / IaC)

---

## J

### JSON
**Simple:** A text format for structured data that both humans and computers can read.  
**Why it matters:** Used for configuration, APIs, logging, and data interchange.  
**Example:** `{"name": "Alice", "age": 30, "email": "alice@example.com"}` - clearly structured compared to unstructured text.  
**Related Sections:** 03 (Configuration), 11 (Observability - structured logging)

---

## K

### Kubernetes
**Simple:** A system for running and managing many containerized applications automatically.  
**Why it matters:** Automates deployment, scaling, and management of containers - essential for large applications.  
**Example:** Instead of manually starting Docker containers on different servers, Kubernetes does it automatically.  
**Related Sections:** 05 (CI/CD - containerization), 08 (Infrastructure - Kubernetes manifests)

---

## L

### License (Software)
**Simple:** Legal rules about how others can use your code.  
**Why it matters:** Users need to know if they can use your code, modify it, sell it, etc. Different licenses have different rules.  
**Example:** MIT license is permissive (anyone can use it). GPL requires anyone using your code to share their modifications.  
**Related Sections:** 04 (Dependency Management - license checking), 01 (Root Directory - LICENSE file), 10 (Security - compliance)

### Lockfile (Package Lockfile)
**Simple:** A file that records the exact versions of dependencies installed so everyone gets the same versions.  
**Why it matters:** Without a lockfile, different people might install different versions, causing "works on my machine" problems.  
**Example:** `package-lock.json` for npm records that version 1.2.3 of library X was installed. Next install gets exactly 1.2.3, not 1.2.4.  
**Related Sections:** 04 (Dependency Management)

---

## M

### MAJOR.MINOR.PATCH
**Simple:** Three numbers in a version (like 2.3.1) that communicate what kind of change happened.  
**Why it matters:** Users can look at version numbers and understand if an update is risky or safe for them.  
**Example:** 1.0.0 → 1.1.0 (new feature, safe to update) vs 1.0.0 → 2.0.0 (breaking change, review carefully).  
**Related Sections:** 12 (Change Management - semantic versioning)

### Merge
**Simple:** Combining changes from one branch into another branch.  
**Why it matters:** Integrates finished work back into the main codebase.  
**Example:** Merge "feature/login" branch into "main" branch when login is ready.  
**Related Sections:** 09 (Governance - code review), 12 (Change Management)

### Monorepo
**Simple:** A single repository containing multiple related projects/packages (like multiple apps and shared libraries in one repo).  
**Why it matters:** Simplifies shared code management but requires careful structure to avoid confusion.  
**Example:** Repository with `apps/frontend/`, `apps/backend/`, `packages/shared-ui/` - three projects, one repo.  
**Related Sections:** 02 (Source Code Layout - monorepo structure), 01 (Root Directory)

---

## N

### npm / yarn / pnpm
**Simple:** Package managers for JavaScript/Node.js - they install libraries your code depends on.  
**Why it matters:** Manages dependencies so you don't have to manually download and track hundreds of libraries.  
**Example:** `npm install express` downloads the Express web framework and its dependencies.  
**Related Sections:** 04 (Dependency Management)

---

## P

### Package
**Simple:** A bundle of reusable code (library) that others can install and use.  
**Why it matters:** Lets you share code across projects and lets others benefit from your work.  
**Example:** React is a package. Flask is a package. You install them with package managers.  
**Related Sections:** 04 (Dependency Management), 02 (Source Code Layout - packages/ directory)

### Package Manager
**Simple:** A tool that downloads, installs, and manages libraries your project needs.  
**Why it matters:** Without it, managing hundreds of dependencies would be impossible.  
**Example:** npm, pip, Cargo, poetry - each language has its preferred package manager.  
**Related Sections:** 04 (Dependency Management), 00 (Prerequisites - decision point)

### P0 / P1 / P2
**Simple:** Priority levels. P0 = required now, P1 = recommended soon, P2 = advanced/future.  
**Why it matters:** Helps you focus on what matters first instead of trying to do everything.  
**Example:** P0 = get tests running, P1 = automate test execution, P2 = add performance testing.  
**Related Sections:** Throughout all sections

### Port
**Simple:** A virtual "door" on a computer where a service listens for connections.  
**Why it matters:** Multiple services can run on the same computer if they use different ports.  
**Example:** Your local API runs on port 3000, your database on port 5432 - they don't conflict.  
**Related Sections:** 03 (Configuration), 11 (Observability)

### Pre-commit Hook
**Simple:** Automatic checks that run before code is committed - can block bad commits.  
**Why it matters:** Catches issues before they enter version history (better than catching them in CI later).  
**Example:** Pre-commit hook checks formatting, lint issues, secret files before commit goes through.  
**Related Sections:** 03 (Configuration & Environment), 12 (Change Management - commit validation)

---

## R

### README.md
**Simple:** The first file people see about your project - explains what it is and how to use it.  
**Why it matters:** First impression of your project. Good README attracts users and contributors.  
**Example:** "# My Project - A lightweight task manager for teams. Install with: `npm install my-project`"  
**Related Sections:** 07 (Documentation), 01 (Root Directory - required files)

### Repository (Repo)
**Simple:** A folder (usually in Git/GitHub) containing your project files and their change history.  
**Why it matters:** Stores everything about your project with full version history and collaboration support.  
**Example:** `github.com/alice/my-awesome-app` is a repository containing the entire project code.  
**Related Sections:** Throughout all sections

---

## S

### SAST (Static Application Security Testing)
**Simple:** Automated tool that scans source code for security vulnerabilities WITHOUT running it.  
**Why it matters:** Catches security issues early before code runs.  
**Example:** SAST tool finds hardcoded passwords, SQL injection vulnerabilities, insecure functions.  
**Related Sections:** 05 (CI/CD - security scanning), 10 (Security & Compliance)

### SBOM (Software Bill of Materials)
**Simple:** A detailed list of every component, library, and dependency in your software.  
**Why it matters:** When a vulnerability affects a common library, you need to quickly check if you're affected.  
**Example:** "My app uses: React 18.0, Express 4.5, MongoDB driver 3.2, ..." - if any has a known vulnerability, you know immediately.  
**Related Sections:** 10 (Security & Compliance), 04 (Dependency Management)

### Secret
**Simple:** A sensitive value your code needs that MUST NOT be committed to Git (passwords, API keys, tokens).  
**Why it matters:** If secrets leak to GitHub, anyone can impersonate your service or access your data.  
**Example:** Database password, AWS access key, authentication token - these should NEVER be in code.  
**Related Sections:** 03 (Configuration & Environment), 10 (Security & Compliance)

### Semantic Versioning (SemVer)
**Simple:** Version numbering (MAJOR.MINOR.PATCH) that communicates what kind of change happened.  
**Why it matters:** Users can understand from the version number if an update might break their code.  
**Example:** 2.0.0 = breaking changes, 2.1.0 = new features (safe), 2.1.3 = bug fixes (safe).  
**Related Sections:** 12 (Change Management)

### Stack Trace
**Simple:** The path of function calls that led to an error - shows exactly where the error occurred.  
**Why it matters:** Essential for debugging - tells you which line failed and the chain of calls leading to it.  
**Example:** When an error happens, the stack trace shows: "line 45 called line 30 called line 15 - error on line 15".  
**Related Sections:** 11 (Observability & Monitoring), 10 (Security)

### Structured Logging
**Simple:** Logging in a consistent machine-readable format (usually JSON) instead of just plain text.  
**Why it matters:** Makes it easy to search, filter, and analyze logs automatically.  
**Example:** `{"level":"error","message":"Connection failed","timestamp":"2026-01-29T10:00:00Z"}` vs "Error: Connection failed"  
**Related Sections:** 11 (Observability & Monitoring)

---

## T

### Test Coverage
**Simple:** Percentage of your code that is tested by tests.  
**Why it matters:** Higher coverage means fewer untested edge cases and bugs.  
**Example:** If 80% of your code is covered by tests, 20% is untested and could have hidden bugs.  
**Related Sections:** 06 (Testing), 13 (Quality Health Checks)

### Test Pyramid
**Simple:** A strategy for organizing tests: lots of fast unit tests, fewer slower integration tests, even fewer slow end-to-end tests.  
**Why it matters:** Balances testing confidence with test speed - fast feedback loop.  
**Example:** 70% unit tests (fast, isolated), 20% integration tests, 10% E2E tests (slow but realistic).  
**Related Sections:** 06 (Testing Structure)

### Terraform
**Simple:** A tool for writing code that describes cloud infrastructure (servers, databases, networks).  
**Why it matters:** Lets you manage cloud infrastructure as code - repeatable, version-controlled, reviewable.  
**Example:** Terraform code that says "create AWS database with 20GB storage in us-east-1".  
**Related Sections:** 08 (Infrastructure / IaC)

---

## U

### Unstructured Logging
**Simple:** Logs written as plain text instead of structured format like JSON.  
**Why it matters:** Harder to search and analyze but simpler to read for humans.  
**Example:** "User login failed - timeout after 30 seconds" (plain text vs structured JSON).  
**Related Sections:** 11 (Observability & Monitoring - upgrade from unstructured)

---

## V

### Vulnerability
**Simple:** A security weakness in code that can be exploited to cause damage.  
**Why it matters:** Vulnerabilities can be exploited to steal data, crash services, or gain unauthorized access.  
**Example:** SQL injection vulnerability lets attacker run unauthorized database commands. Buffer overflow lets attacker crash program.  
**Related Sections:** 10 (Security & Compliance), 04 (Dependency Management - scanning)

---

## W

### Workflow (GitHub Actions)
**Simple:** A description of automated tasks that run when something happens in your repository.  
**Why it matters:** Automates repetitive work (tests, deployments, checks) without manual intervention.  
**Example:** "When PR is opened, run tests. If tests pass, auto-merge. If tests fail, request changes."  
**Related Sections:** 05 (CI/CD Structure - GitHub Actions)

---

## Y

### YAML
**Simple:** A human-readable text format for configuration (alternative to JSON).  
**Why it matters:** Used for CI/CD configs, Kubernetes manifests, and many configuration files.  
**Example:** YAML version of config: `database: postgres` vs JSON: `{"database": "postgres"}`  
**Related Sections:** 05 (CI/CD Structure - workflow syntax), 08 (Infrastructure - Kubernetes)

---

## Z

### Zone of Proximal Development (Learning Context)
**Simple:** The gap between what you can do alone and what you can do with help/guidance.  
**Why it matters:** Effective learning happens in this zone - not too easy, not too hard, with support.  
**Application in ALIGNMENT:** Mentor mode aims to provide scaffolding to help you learn concepts at the right level.  
**Related Sections:** Philosophy throughout ALIGNMENT standards

---

## How to Use This Glossary

### ✅ When You See a Term You Don't Know
1. Look it up here for a simple explanation
2. Read "Why it matters" to understand relevance
3. See example in context
4. Jump to related sections if you want to learn more
5. Use "Technical term" version if you're already familiar

### 📚 Building Domain Knowledge
**Beginner Path:**
1. Start with terms in Section 00 (Prerequisites)
2. Learn foundational terms (Git, Repository, Branch, Commit)
3. Then move through sections 1-13 in order

**Skip-to-Section Path:**
1. Find section you're working on
2. Look up any unfamiliar terms in its section
3. References below will link to explanations

### 🔗 Navigating References
**Related Sections:** Each term shows which standards sections use it. Click through to see practical applications.

---

## Terms to Add as Sections Modernize

**Pending Addition:**
- [ ] Algorithm
- [ ] Authentication vs Authorization
- [ ] Canary Deployment
- [ ] Container / Docker
- [ ] Continuous Learning
- [ ] Deployment
- [ ] Development Environment
- [ ] Distributed System
- [ ] Docker Compose
- [ ] Edge Cases
- [ ] Environment (dev/staging/prod)
- [ ] Flaky Test
- [ ] GraphQL
- [ ] gRPC
- [ ] Load Testing
- [ ] Microservices
- [ ] Migration (database)
- [ ] ML/AI specific terms
- [ ] Monitoring
- [ ] Mutation Testing
- [ ] Observable / Observability
- [ ] Onboarding
- [ ] Pair Programming
- [ ] Performance Budget
- [ ] Playwright / Cypress
- [ ] PR / Pull Request
- [ ] Provisioning
- [ ] Puppeteering (browser)
- [ ] Regression
- [ ] Release Candidate
- [ ] REST API
- [ ] Rollback
- [ ] SLA (Service Level Agreement)
- [ ] Sprint
- [ ] SQLi / SQL Injection
- [ ] State / Stateless
- [ ] Tenancy / Multi-tenant
- [ ] Telemetry
- [ ] Terraform Module
- [ ] Threshold
- [ ] Trace / Tracing
- [ ] Unit Test vs Integration Test vs E2E Test
- [ ] Vendoring
- [ ] Webhook
- [ ] X-Ray / Tracing
- [ ] Zero Trust Security

---

**Glossary Version:** 1.0  
**Last Updated:** 2026-01-29  
**Maintainer:** ALIGNMENT Standards Team
