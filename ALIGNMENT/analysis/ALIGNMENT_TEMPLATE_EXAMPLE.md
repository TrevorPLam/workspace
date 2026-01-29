# Section Template & Example - Modernized Format

**Purpose:** Show exactly what modernized sections should look like  
**Example:** Section 00 (Prerequisites) adapted to new format  
**Use this:** As reference while refactoring all 14 sections

---

## Master Template Structure

```markdown
# [Number]. [Topic Name]

## 🎯 What You're Trying to Accomplish
[Explain the goal in 2-3 sentences. No jargon. Use analogies if needed.
Answer: "Why should I care about this section?"]

## 🎓 Why This Section Exists
[Context: How does this fit into the bigger picture?
What problem does this solve?]

## 📚 Key Concepts (Beginner Explanations)

### Concept 1: [Technical Term]
**Simple version:** [2-3 sentences explaining to someone new to the field]

**Why it matters:** [Concrete consequence if ignored]

**Real-world analogy:** [Everyday comparison that makes sense]

**Example in code/practice:** [Concrete code or action example]

**Technical version (for reference):** [The actual technical definition]

**Where it's used:** [Which subtasks depend on understanding this]

---

## 🤔 Decision Checkpoints

### Decision 1: [Question]
**Why you need to decide this:**
[Explain why this choice affects the rest of the section]

**Option A: [Choice 1]**
- When to pick: [Scenario]
- Pros: [Benefits]
- Cons: [Downsides]
- Example: [Concrete case]

**Option B: [Choice 2]**
- When to pick: [Scenario]
- Pros: [Benefits]
- Cons: [Downsides]
- Example: [Concrete case]

**Option C: [Skip This Entirely]**
- When this applies: [Which projects don't need this?]
- How to know: [How to tell if you need it]

**Decision:** [ ] Option A [ ] Option B [ ] Skip

**Document your answer** in your progress file for reference later.

---

## ✅ Subtasks (With Owner Tags)

### Task Group 1: [Logical Grouping]

#### Subtask 1.1 - [Descriptive Title] | **AGENT**
**Estimated time:** 15-30 minutes

**Purpose:** [Why this task exists and what it accomplishes]

**What you need to know first:**
- Prerequisite concept from Key Concepts above? [Link]
- Have you completed Subtask X? [Link]

**Step-by-step:**
1. [Clear, actionable step]
2. [Next step]
3. [Final step]

**Expected outcome:** [What success looks like - be specific]

**How to verify:** [Checklist or test to confirm it worked]

**Common mistakes:** [What beginners usually get wrong]

**Resources/Templates:**
- Template link: [Link if provided]
- Example: [Code or output example]

**If you get stuck:** [Common error messages and solutions]

---

#### Subtask 1.2 - [Descriptive Title] | **USER**
[Same structure as Subtask 1.1]

---

### Task Group 2: [Next Logical Grouping]
[Continue with more subtasks...]

---

## 🔗 Cross-Section Dependencies

**Depends on:**
- [ ] Section [X] - [Reason]
- [ ] Subtask [Y] from Section [Z] - [Reason]

**Required for:**
- [ ] Section [A] - [Reason]
- [ ] Section [B] - [Reason]

---

## ⚠️ Common Pitfalls

### Pitfall 1: [Beginner Mistake]
**Why it happens:** [Why beginners typically do this]
**Symptoms:** [How you'll know you made this mistake]
**Fix:** [Step-by-step to recover]
**Prevention:** [How to avoid it next time]

### Pitfall 2: [Another Common Mistake]
[Same structure]

---

## 📖 Learn More

**Want to understand the "why" better?**
- Deep dive: [Link to explanation]
- Philosophy: [Link to principles document]
- Advanced: [Link to P2 advanced section]

**Examples from real projects:**
- Example 1: [Link/reference]
- Example 2: [Link/reference]

**Questions?**
- FAQ: [Link]
- Community: [Link]
- Contact: [How to get help]

---

## 📋 Completion Checklist

**After completing all subtasks in this section, verify:**
- [ ] All decision checkpoints documented
- [ ] All subtasks completed
- [ ] Expected outcomes match what you have
- [ ] Common pitfalls haven't caught you
- [ ] Next section prerequisites met (from Cross-Section Dependencies)

---
```

---

## ✏️ EXAMPLE: Section 00 (Prerequisites) - Modernized Format

# 0. Prerequisites & Setup

## 🎯 What You're Trying to Accomplish
Before you start organizing your repository, you need to understand what you're building. This section helps you make 10 key decisions about your project so we can give you guidance tailored to YOUR situation (not generic advice).

Think of this like a house blueprint questionnaire - before the architect designs your house, they need to know: "Single family home or apartment? Desert or snowy climate? Budget?" Same idea here.

## 🎓 Why This Section Exists
The other 13 sections have different instructions for different situations. A versioned library needs a CHANGELOG; an internal tool doesn't. A monorepo needs different structure than a single app. This section captures those differences so you follow the right path.

## 📚 Key Concepts (Beginner Explanations)

### Concept 1: Versioned Project
**Simple version:** A versioned project is code that other people install and use (like a library). You need to tell them when things change by changing the version number.

**Why it matters:** If you publish a version "1.0.0" and then later publish "2.0.0", users need to understand if "2.0.0" will break their code or not.

**Real-world analogy:** Like iPhone versions. iPhone 15 might not support apps made for iPhone 20. Version numbers warn people about compatibility.

**Example in code/practice:**
```
// Version 1.0 - User can call this
getUser("alice")

// Version 2.0 - Breaking change! Old code breaks
fetchUser("alice")  // renamed function
```

**Technical version (for reference):** Versioned projects expose a public API that external consumers depend on. Changes require version management per Semantic Versioning.

**Where it's used:** Section 12 (Change Management), Section 1 (requires CHANGELOG.md)

---

### Concept 2: Monorepo
**Simple version:** A monorepo is when you keep multiple related projects in one Git repository instead of separate repositories.

**Why it matters:** Changes that affect multiple projects can be tested together. Shared code is easier to maintain.

**Real-world analogy:** Think of a company with multiple departments. They could have separate buildings (multiple repositories) or one big building with different floors (monorepo). The monorepo approach makes shared services easier.

**Example in code/practice:**
```
Single repo contains:
- apps/web/          (Frontend)
- apps/mobile/       (Mobile app)
- services/api/      (Backend API)
- packages/shared/   (Shared UI components)

All in ONE Git repository, different directory trees
```

**Technical version (for reference):** Repository structure that uses top-level `apps/` and `packages/` directories to organize multiple workspaces within a single Git root.

**Where it's used:** Section 2 (Source Code Layout), Section 1 (Directory structure)

---

### Concept 3: Package Manager
**Simple version:** A tool that automatically downloads and keeps track of libraries your code needs.

**Why it matters:** Modern code depends on hundreds of libraries. Without a package manager, you'd have to manually download each one and track which version you're using.

**Real-world analogy:** Like a grocery shopping assistant. Instead of manually hunting for "3 cans of tomatoes, version 2024-crop," you say "npm install tomatoes" and it finds the right version and brings it home.

**Example in code/practice:**
```bash
# Without package manager: Manual download (tedious, error-prone)
# curl https://...tomatoes-2024.zip → unzip → update imports everywhere

# With package manager: One command
npm install tomatoes

# It downloads, saves version info, and updates your project
```

**Technical version (for reference):** Package managers (npm, pip, cargo, poetry) use manifest files (package.json, requirements.txt, Cargo.toml) and lockfiles to manage and record dependencies and versions.

**Where it's used:** Section 4 (Dependency Management)

---

## 🤔 Decision Checkpoints

### Decision 1: Is This a Versioned Project?
**Why you need to decide this:**
The rest of the standards assume you know whether you need a CHANGELOG file. Versioned projects do; internal tools don't.

**Option A: Versioned (You release public versions)**
- When to pick: Other people install/use your code. You publish updates with version numbers.
- Pros: Clear communication about changes. Users can see what changed between versions.
- Cons: You must maintain a CHANGELOG. You must follow semantic versioning.
- Example: React library, Django plugin, npm package that others install

**Option B: Internal Project (Only your team uses it)**
- When to pick: Code is only used inside your organization. Not published for external use.
- Pros: Less formal version management needed. Simpler release process.
- Cons: Teams using your code still need to know what changed (just less formally).
- Example: Internal company tool, microservice, one-off script

**Decision:** [ ] Yes - Versioned [ ] No - Internal-only

**Document your answer** in your progress file: "decision_points.is_versioned_project = true/false"

---

### Decision 2: What is Your Project Type?
**Why you need to decide this:**
Different project types have different structures. A library structure looks different from a web app structure.

**Option A: Library/Package**
- When to pick: Others install and import your code into their projects.
- Example: React, express, numpy - libraries that get installed as dependencies
- Structure needs: Clear public API, stable exports, version management

**Option B: Application**
- When to pick: Users run your app directly (web app, desktop app, mobile app).
- Example: Gmail, Slack, iPhone app
- Structure needs: Entry point, configuration, deployment artifacts

**Option C: CLI Tool**
- When to pick: Users run it from command line with arguments.
- Example: Git, npm, Docker - command-line tools
- Structure needs: Argument parsing, help documentation, exit codes

**Option D: API/Service**
- When to pick: Other services make requests to your service over the network.
- Example: REST API, GraphQL endpoint, microservice
- Structure needs: Health checks, deployment readiness, monitoring

**Option E: Monorepo**
- When to pick: Multiple of the above in one repository (web app + API + shared library).
- Example: Next.js app with API routes, Turborepo monorepo
- Structure needs: Workspace organization, dependency boundaries

**Option F: Infrastructure/IaC**
- When to pick: Pure infrastructure code (Terraform, Kubernetes manifests).
- Example: Cloud provisioning scripts, Kubernetes configs
- Structure needs: Environment separation, secret management

**Decision:** [ ] Library [ ] Application [ ] CLI [ ] API/Service [ ] Monorepo [ ] Infrastructure [ ] Other: ___

**Document your answer** in your progress file.

---

### Decision 3: Package Manager?
**Why you need to decide this:**
Different languages have different package managers. Your choice affects dependency management (Section 4).

**Option A: npm/yarn/pnpm (JavaScript/TypeScript)**
- Pros: Rich ecosystem, widely used, good tools
- Cons: Large installed size, can have resolution issues
- Decision point: Which? pnpm = fastest, npm = most common, yarn = in-between

**Option B: pip/poetry/pipenv (Python)**
- Pros: Simple, Python standard
- Cons: Various tools with different philosophies
- Decision point: poetry = modern+recommended, pip = basic, pipenv = structured

**Option C: Cargo (Rust)**
- Pros: Built-in, excellent, no choice really
- Cons: None

**Option D: Go modules (Go)**
- Pros: Built-in, minimal
- Cons: No choice

**Decision:** [ ] npm/yarn/pnpm [ ] pip/poetry/pipenv [ ] Cargo [ ] Go modules [ ] Other: ___

**Document your answer** in your progress file.

---

### Decisions 4-10: [Continue with same structure...]
[Abbreviated for space, but follow same pattern]

---

## ✅ Subtasks (With Owner Tags)

### Task Group 1: Answer Decision Questions

#### Subtask 1.1 - Gather Project Information | **USER**
**Estimated time:** 20-30 minutes

**Purpose:** Collect the facts about your project so you can make informed decisions

**What you need to know first:**
- Have you read the Key Concepts section above? (It explains the terms)

**Step-by-step:**
1. **Open a document** where you can write answers (text editor, Google Doc, Notion)
2. **Answer each decision question** above (1-10)
   - Don't overthink it - first answer is usually right
   - You can change these later if needed
3. **Look at project files** to confirm:
   - Do you have `package.json`? (JavaScript project)
   - Do you have `go.mod`? (Go project)
   - Do you have `setup.py`? (Python project)
4. **Ask your team** if unsure about project type or audience

**Expected outcome:** Document with answers to 10 decision questions

**How to verify:**
- [ ] All 10 questions answered
- [ ] Answers match your actual project
- [ ] If uncertain, I have a note about what I'm unsure about

**Common mistakes:**
- ❌ "I don't know the answer" - That's OK! Write "Unsure, leaning toward X"
- ❌ "All answers are perfect" - Unlikely. If every answer feels obvious, you might be overthinking
- ❌ Skipping decisions - Don't skip any. Even "N/A" is an answer.

**Resources/Templates:**
- Template: `alignment-progress.json` - we'll create this in next subtask
- Reference: Key Concepts section above explains each term

**If you get stuck:**
- "What's the difference between app and service?" → App = end user runs it. Service = other code calls it over network.
- "Should I choose npm or pip?" → Choose based on your programming language (JavaScript → npm, Python → pip)

---

#### Subtask 1.2 - Create alignment-progress.json File | **USER**
**Estimated time:** 10 minutes

**Purpose:** Save your decisions in a format that all sections can reference later

**What you need to know first:**
- Subtask 1.1 completed (you have answers)
- Basic text editor (VS Code, Notepad, whatever)

**Step-by-step:**
1. **Create a new file** named `alignment-progress.json` in your repository root
2. **Copy this template:**
```json
{
  "repository_name": "your-project-name",
  "started_date": "2026-01-29",
  "decision_points": {
    "is_versioned_project": true,
    "project_type": "application",
    "primary_language": "JavaScript",
    "package_manager": "npm",
    "cicd_platform": "github",
    "is_public_repo": true,
    "structure_type": "application",
    "has_code_in_root": false,
    "has_secrets_in_repo": false,
    "repository_owners": ["alice@company.com"]
  },
  "notes": "Starting alignment process. Team discussed and agreed on above decisions."
}
```

3. **Replace values:**
   - `"your-project-name"` → Your actual repo name
   - `true` → `false` (or vice versa)
   - `"application"` → Your actual project type
   - etc.
4. **Save the file**
5. **Commit to Git:**
   ```bash
   git add alignment-progress.json
   git commit -m "Add alignment decision points"
   ```

**Expected outcome:** 
- File exists: `alignment-progress.json` 
- File is committed to Git
- File contains your answers in JSON format

**How to verify:**
- [ ] File exists in repository root: `alignment-progress.json`
- [ ] `git log --oneline` shows commit with message "Add alignment decision points"
- [ ] File content is valid JSON (no error messages if you open it)

**Common mistakes:**
- ❌ Saving as `.txt` instead of `.json` - Use `.json` extension
- ❌ Invalid JSON syntax - Use template exactly, just change values
- ❌ Forgetting to commit - Must `git add` and `git commit`

**Resources/Templates:**
- Template provided above
- Example: Section 0 in standards shows full example JSON

**If you get stuck:**
- "JSON format looks weird" → It's just a structured data format. Follow template exactly.
- "How do I commit files?" → `git add filename`, then `git commit -m "message"`

---

#### Subtask 1.3 - Verify Your Answers Make Sense | **USER**
**Estimated time:** 10 minutes

**Purpose:** Sanity check - catch contradictions before moving forward

**What you need to know first:**
- Subtasks 1.1 and 1.2 completed

**Step-by-step:**
1. **Review your answers** in `alignment-progress.json`
2. **Check for contradictions:**
   - If `is_versioned_project = true`, do you actually release versions? (If no, change to false)
   - If `project_type = "library"`, do others install it? (If not, change to "application")
   - If `has_secrets_in_repo = true`, do you know which files? (If yes, go to Section 3 immediately)
3. **Ask yourself:** "Would this make sense to a teammate?"
4. **Update if needed:** Edit `alignment-progress.json` and commit again

**Expected outcome:**
- Answers feel accurate when you re-read them
- No obvious contradictions

**How to verify:**
- [ ] Re-read all answers
- [ ] At least 2 decisions feel right, 0 feel obviously wrong
- [ ] I can explain each answer in 1 sentence

**Common mistakes:**
- ❌ "All my answers are perfect!" - Unlikely. Most people change 1-2 after review.
- ❌ Skipping this verification - Quick sanity check saves hours later.

**If you get stuck:**
- "I'm not sure about my answers" → Pick what feels most likely. You can change it later in Section 1.

---

### Task Group 2: Share with Team

#### Subtask 1.4 - Share Decisions with Team | **USER**
**Estimated time:** 30 minutes + team discussion time

**Purpose:** Make sure the team agrees. Alignment works better with consensus.

**What you need to know first:**
- Subtasks 1.1-1.3 completed

**Step-by-step:**
1. **Share `alignment-progress.json`** with team (Slack, email, or meeting)
2. **Ask for feedback:**
   - "Do these decisions match your understanding?"
   - "Anything you'd change?"
3. **Discuss:** Answer any questions
4. **Update if team disagrees:** Modify file if consensus changes
5. **Document:** Add note in `alignment-progress.json`:
   ```json
   "team_review": "Discussed 2026-01-29. Alice, Bob, and Charlie agree. Carol suggested change to is_public_repo, will reconsider."
   ```

**Expected outcome:**
- Team has seen and acknowledged the decisions
- Any disagreements are documented
- File is updated with team feedback

**How to verify:**
- [ ] Team has reviewed the decisions
- [ ] Any changes are committed to Git
- [ ] Notes explain any disagreements

**Common mistakes:**
- ❌ "I'll make decisions myself" → Better with team input
- ❌ "We argued about everything" → Disagreement is OK, just document it

**If you get stuck:**
- "Team disagrees with me" → That's fine. Document it and move forward. You can revisit later.

---

## 🔗 Cross-Section Dependencies

**This section is self-contained:**
- No prerequisites (this is the first section)

**Required for:**
- [✅ All other sections] - Decisions made here guide Sections 1-13

---

## ⚠️ Common Pitfalls

### Pitfall 1: "I Don't Know My Project Type"
**Why it happens:** Some projects are hybrids (web app + API). You worry about picking wrong.

**Symptoms:** You wrote "Maybe application? But also provides API?" - uncertain answer.

**Fix:** Pick the PRIMARY function. Web app that has an API still = application (the API is secondary).

**Prevention:** Ask "What is the main thing this project does?" Not "What can it do?"

---

### Pitfall 2: "I Answered Everything Perfect"
**Why it happens:** Confirmation bias - you answered quickly and assume you're right.

**Symptoms:** All 10 answers feel obvious and unambiguous.

**Fix:** Re-read after a break (walk around, get coffee). Usually you'll see something to refine.

**Prevention:** Subtask 1.3 exists specifically for this - don't skip it.

---

### Pitfall 3: "Decisions Don't Matter, I'll Figure It Out Later"
**Why it happens:** Impatience - you want to start organizing code.

**Symptoms:** You rush through and commit without thinking.

**Fix:** Slow down. These 10 decisions change the advice in every section. Wrong decision = wrong structure.

**Prevention:** Do Subtask 1.4 (team review). Forces you to slow down and think.

---

## 📖 Learn More

**Want to understand the "why" better?**
- [Semantic Versioning Guide](https://semver.org) - Deep dive on versioning if you want to understand more
- [Monorepo Patterns](../principles/Monorepo-Principles.md) - Why monorepos matter
- [Package Manager Comparison](../resources/package-managers.md) - Detailed comparison

**Questions?**
- FAQ: Section 0 FAQ (if we build one)
- Community: [Slack channel]
- Contact: [Email]

---

## 📋 Completion Checklist

**After completing all subtasks in Section 0, verify:**
- [ ] `alignment-progress.json` created and committed
- [ ] All 10 decision points answered
- [ ] Answers verified for consistency
- [ ] Team has reviewed and agreed (or disagreement documented)
- [ ] No secrets in repository yet (if found, go to Section 3 immediately)
- [ ] Ready to proceed to Section 1

---

## 🎯 Next: Section 1 - Root Directory Structure
Once you've completed this section, you have the context to organize your root directory.

[Link to Section 1]

---

