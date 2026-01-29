# Question Refinement Summary

**Date:** 2026-01-28  
**Purpose:** Refined questions throughout ALIGNMENT standards to gather actionable information rather than basic existence checks

## Philosophy

**Before:** Questions asked "Do you have X?" (existence checks)  
**After:** Questions ask "What should X contain?" (content-focused, actionable)

## Key Changes

### 1. Documentation Questions (Section 7)

**Before:**
- "Do you have a README?"
- "Does README exist?"

**After:**
- "What should the README say about installation?"
- "What are the exact run commands?"
- "What example use cases should be demonstrated?"
- "What's the first contribution workflow?"

### 2. Configuration Questions (Section 3)

**Before:**
- "Do you have secrets?"
- "Do you need environment variables?"

**After:**
- "What secrets/credentials does your application need?"
- "What environment variables do you need?" (with specific categories)
- "What's the purpose of each variable?"

### 3. CI/CD Questions (Section 5)

**Before:**
- "Do you have CI/CD configured?"
- "What tools do you use?"

**After:**
- "What CI/CD pipeline stages do you need?"
- "What security scanning should your pipeline include?"
- Focus on what should be included, not what exists

### 4. Testing Questions (Section 6)

**Before:**
- "Do you have existing tests?"
- "What framework do you use?"

**After:**
- "What types of tests do you need?" (unit, integration, e2e, performance, a11y)
- Focus on test strategy, not existence

### 5. Security Questions (Section 10)

**Before:**
- "What is your security compliance level?"
- "Do you handle sensitive data?"

**After:**
- "What security requirements apply?" (with specific compliance types)
- "What sensitive data does your application handle?" (with data types)
- "What types of PII?" (specific information)

### 6. Observability Questions (Section 11)

**Before:**
- "Do you have logging?"
- "Do you have error tracking?"

**After:**
- "What observability do you need?" (with specific types)
- "What should be logged?"
- "What metrics matter?"
- "What endpoints/services need health checks?"

### 7. Change Management Questions (Section 12)

**Before:**
- "Do you use SemVer?"
- "Do you have a CHANGELOG?"

**After:**
- "What versioning strategy will you use?" (with starting version)
- "What should your CHANGELOG.md include?" (specific sections)
- Focus on content and strategy

### 8. Quality Questions (Section 13)

**Before:**
- "Do you have automated health checks?"
- "What metrics do you track?"

**After:**
- "What quality metrics should you track?" (with target percentages)
- "What compliance checks should be automated?" (specific checks)
- Focus on what should be measured

### 9. Source Code Layout (Section 2)

**Before:**
- "Do you have code in root?"
- "What structure type?"

**After:**
- "Where should your source code live?" (with specific directories)
- "What files need to move?" (if code in root)
- "How many apps/packages?" (for monorepos)

### 10. Infrastructure Questions (Section 8)

**Before:**
- "Do you have IaC?"
- "Do you deploy to cloud?"

**After:**
- "What infrastructure needs to be managed?" (with specific resources)
- "What infrastructure tools will you use?" (with specific use cases)
- "What services?" (for Docker Compose)
- "What workloads?" (for Kubernetes)

### 11. Dependency Management (Section 4)

**Before:**
- "Which package manager?"
- "Does lockfile exist?"

**After:**
- "Is lockfile committed?" (verification question)
- "What dependencies are critical?" (top 5-10 most important)
- Focus on understanding critical dependencies

### 12. Governance Questions (Section 9)

**Before:**
- "What branching strategy?"
- "Do you have code review standards?"

**After:**
- "What branching strategy fits your workflow?" (with use case guidance)
- "What should code reviews cover?" (specific aspects)
- Focus on review content and process

## Benefits

1. **Actionable Information:** Questions gather information needed to create/improve content
2. **Less Redundant:** No need to ask "do you have X?" when we can ask "what should X contain?"
3. **Better Guidance:** Questions guide users to think about what's needed
4. **More Efficient:** Users provide useful information upfront
5. **Content-Focused:** Emphasis on quality and completeness, not just existence

## Examples of Improved Questions

### Example 1: README
**Old:** "Does README exist?"  
**New:** "What should the README say about installation? What are the exact commands?"

### Example 2: Environment Variables
**Old:** "Do you need environment variables?"  
**New:** "What environment variables do you need? (Database credentials, API keys, service URLs, feature flags)"

### Example 3: Testing
**Old:** "Do you have tests?"  
**New:** "What types of tests do you need? (Unit, integration, e2e, performance, accessibility)"

### Example 4: Security
**Old:** "Do you handle sensitive data?"  
**New:** "What sensitive data does your application handle? (PII types, credentials, financial data, health data)"

## Verification Updates

Updated verification sections to focus on content quality:

**Before:**
- "Verify README exists"

**After:**
- "Verify README content is complete and accurate: Does it clearly explain purpose? Can a new user follow installation steps?"

## Impact

- **Reduced Questions:** Fewer redundant existence checks
- **Better Information:** More actionable data gathered
- **Improved Guidance:** Questions guide users to think about content
- **Quality Focus:** Emphasis on completeness and accuracy

## Files Modified

All standards files (00-13) have been updated with refined questions:
- `standards/00-Prerequisites.md` - Already well-structured
- `standards/01-Root-Directory-Structure.md` - Verification refined
- `standards/02-Source-Code-Layout.md` - Questions refined
- `standards/03-Configuration-Environment.md` - Questions refined
- `standards/04-Dependency-Management.md` - Decision section refined
- `standards/05-CI-CD-Structure.md` - Questions refined
- `standards/06-Testing-Structure.md` - Questions refined
- `standards/07-Documentation-Standards.md` - Questions and verification refined
- `standards/08-Infrastructure-IaC.md` - Questions refined
- `standards/09-Governance-Ownership.md` - Questions refined
- `standards/10-Security-Compliance.md` - Questions refined
- `standards/11-Observability-Monitoring.md` - Questions refined
- `standards/12-Change-Management.md` - Questions refined
- `standards/13-Quality-Health-Checks.md` - Questions refined

---

**Status:** ✅ Complete - All questions refined to gather actionable, content-focused information
