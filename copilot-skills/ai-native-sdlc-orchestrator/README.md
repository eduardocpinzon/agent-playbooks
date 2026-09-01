# AI-Native SDLC Orchestrator

A specialized **skill** for GitHub Copilot that implements the software factory model with 6 phases: **Planning → Design → Building → Testing → Deploy → Maintenance**.

This skill transforms rapid code generation into a verifiable process, where each phase produces a documented and tested artifact.

---

## 📋 Contents

- [Description](#description)
- [How to Install](#how-to-install)
- [Project Structure](#project-structure)
- [Using the Skill](#using-the-skill)
- [Phases Reference](#phases-reference)
- [Contribution Guide](#contribution-guide)

---

## 📝 Description

The **AI-Native SDLC Orchestrator** is a skill that functions as an implementation agent within a structured development workflow. It:

- 🎯 **Plans** requirements through `intent.md` documents
- 🏗️ **Designs** architecture with `spec.md`
- 💻 **Builds** source code and tests automatically
- ✅ **Tests** through integrated evals
- 🚀 **Deploys** with automatic code review
- 🔍 **Monitors** and restarts the cycle in production

Each phase is **artifact-driven**: nothing advances without verifiable documentation.

---

## 🚀 How to Install

### Prerequisites

- **GitHub Copilot** installed in VS Code (version 1.100+)
- **VS Code** version 1.80 or higher
- **Node.js** 18+ (for TypeScript/Node projects)
- **Git** (for version control)

### Option 1: Local Installation (Development)

#### 1. Clone or copy this repository

```bash
git clone <repo-url> ~/.vscode/extensions/ai-native-sdlc
cd ~/.vscode/extensions/ai-native-sdlc
```

**Or**, for macOS/Linux in specific locations:

```bash
# macOS
mkdir -p ~/Library/Application\ Support/Code/User/globalStorage/GitHub.copilot/skills
cp -r ai-native-sdlc ~/Library/Application\ Support/Code/User/globalStorage/GitHub.copilot/skills/

# Linux
mkdir -p ~/.config/Code/User/globalStorage/GitHub.copilot/skills
cp -r ai-native-sdlc ~/.config/Code/User/globalStorage/GitHub.copilot/skills/
```

**Windows:**

```powershell
# PowerShell
$skillsDir = "$env:APPDATA\Code\User\globalStorage\GitHub.copilot\skills"
New-Item -ItemType Directory -Path $skillsDir -Force
Copy-Item -Recurse ".\ai-native-sdlc" "$skillsDir\ai-native-sdlc" -Force
```

#### 2. Restart VS Code

```
Cmd/Ctrl + Shift + P → "Reload Window"
```

#### 3. Verify installation

- Open **Copilot Chat**
- Type `/` to see the list of available skills
- `ai-native-sdlc-orchestrator` should appear in the list

---

### Option 2: Installation via Marketplace (when published)

```
VS Code Extensions → Search "AI-Native SDLC Orchestrator" → Install
```

---

### Option 3: Installation in a Specific Workspace

Place the skill in `.vscode/skills/` at the root of your project:

```
your-project/
├── .vscode/
│   └── skills/
│       └── ai-native-sdlc-orchestrator/
│           ├── SKILL.md          ← Skill definition
│           ├── README.md         ← This file
│           └── templates/
│               ├── intent.md
│               └── spec.md
├── src/
├── tests/
└── README.md
```

When you open this workspace in VS Code, the skill will be loaded automatically.

---

## 📁 Project Structure

```
ai-native-sdlc-orchestrator/
│
├── SKILL.md                          # Main skill definition
│   ├── Description & purpose
│   ├── Operational rules
│   ├── Playbook of 6 phases
│   ├── Artifact conventions
│   └── How to start mid-loop
│
├── README.md                         # This file
│
├── templates/                        # Artifact templates
│   ├── intent.md                     # Template for Phase 1
│   └── spec.md                       # Template for Phase 2
│
├── examples/                         # Complete project examples
│   ├── hello-world-typescript/
│   │   ├── intent.md
│   │   ├── spec.md
│   │   ├── src/
│   │   ├── test/
│   │   └── REVIEW.md
│   │
│   └── crud-feature/
│       ├── intent.md
│       └── spec.md
│
├── .github/
│   └── AGENTS.md                     # Available agents configuration
│
└── docs/
    ├── phases.md                     # Details of each phase
    ├── artifact-conventions.md       # Naming conventions
    └── troubleshooting.md            # FAQ and troubleshooting
```

---

## 💬 Using the Skill

### Activate the Skill

In **Copilot Chat** in VS Code, use:

```
@ai-native-sdlc-orchestrator Creates an authentication feature
```

**Or without explicit mention** — the skill auto-invokes when it detects:
- A new ticket/issue
- A feature request ("create X")
- A bug report
- An idea described in natural language

### Example 1: New Feature

```
User: I need a login endpoint with JWT

Copilot (via skill): 
✅ Phase 1 — Plan: Creating intent.md...
[displays clarification questions]
→ Awaiting approval of intent.md...
```

You review and approve. The skill continues automatically:

```
✅ Phase 2 — Design: Creating spec.md...
✅ Phase 3 — Build: Generating source code and tests...
✅ Phase 4 — Test: Running tests...
✅ Phase 5 — Deploy: Reviewing code...
→ Ready for merge!
```

### Example 2: Fix a Bug

```
User: Login is not validating emails correctly

Copilot (via skill):
→ Creating intent.md for the bug...
→ What is the expected behavior? [question]
→ When does the error occur? [question]
✅ Diagnosis created → Restarting Phase 1 (Maintain)
```

---

## 🔄 Phases Reference

### Phase 1 — Plan
**Input:** Idea, ticket, or bug  
**Output:** `intent.md` (requires human approval)

```
intentions/
└── <slug>/
    └── intent.md
```

### Phase 2 — Design
**Input:** Approved `intent.md`  
**Output:** `spec.md` (technical design)

```
specifications/
└── specs/
    └── <slug>/
        └── spec.md
```

### Phase 3 — Build
**Input:** `spec.md`  
**Output:** Source code + tests

```
src/
├── <feature>.ts
├── <feature>.clj
└── ...

test/
└── <feature>_test.ts
```

### Phase 4 — Test
**Input:** Code + tests  
**Output:** `eval-report.md`

```
specifications/
└── evals/
    └── <slug>.md
```

### Phase 5 — Deploy
**Input:** Approved code (all tests passing)  
**Output:** Pull request with review

```
(Your PR on GitHub/GitLab)
```

### Phase 6 — Maintain
**Input:** Code in production  
**Output:** New `intent.md` if there's an incident

---

## 🛠️ Advanced Configuration

### Customize the Skill for Your Project

Create a `.github/copilot-instructions.md` file at the root of your project:

```markdown
# Copilot Instructions for This Project

## Conventions

- **Language:** TypeScript with strict mode
- **Tests:** Jest + ts-jest
- **Lint:** ESLint + Prettier
- **CI/CD:** GitHub Actions

## Directories

- `/src` — Source code
- `/test` — Tests
- `/specs` — Architecture documentation

## Scripts

```json
{
  "scripts": {
    "build": "tsc",
    "test": "jest",
    "lint": "eslint src/",
    "format": "prettier --write src/"
  }
}
```

## References

- [ADR-001: Use TypeScript](docs/adr/001-typescript.md)
- [Style Guide](docs/STYLE.md)
```

### Integrate with Agents

If you have **custom agents** (like `MCP AppService Builder` or `Explore`), the skill invokes them automatically when relevant:

```
1. Skill detects need for exploration → Invokes `Explore` agent
2. Agent returns context → Skill continues with Phase 2 Design
3. Result integrated into spec.md
```

---

## 📚 Recommended Directory Structure

For a new project:

```
your-project/
│
├── .github/
│   ├── skills/
│   │   └── ai-native-sdlc-orchestrator/
│   │       ├── SKILL.md
│   │       ├── README.md
│   │       └── templates/
│   │
│   ├── copilot-instructions.md
│   └── workflows/
│
├── specifications/
│   ├── intents/
│   │   ├── auth-feature/
│   │   │   └── intent.md
│   │   └── payment-integration/
│   │       └── intent.md
│   │
│   ├── specs/
│   │   ├── auth-feature/
│   │   │   └── spec.md
│   │   └── payment-integration/
│   │       └── spec.md
│   │
│   └── evals/
│       ├── auth-feature.md
│       └── payment-integration.md
│
├── src/
│   ├── auth/
│   ├── payments/
│   └── common/
│
├── test/
│   ├── auth/
│   └── payments/
│
├── docs/
│   ├── ADR/
│   ├── API.md
│   └── ARCHITECTURE.md
│
└── README.md
```

---

## 🔐 Best Practices

### ✅ Do

- ✅ Write `intent.md` before any code
- ✅ Review `spec.md` to verify architectural decisions
- ✅ Let the skill generate ~80% of the code; you implement the 20% complex
- ✅ Keep artifacts (intent, spec, eval) versioned in git
- ✅ Reuse previous specs as templates

### ❌ Don't

- ❌ Skip the `intent.md` — it costs more later
- ❌ Edit `spec.md` without agreeing with the skill
- ❌ Execute generated code without running tests
- ❌ Commit code without eval report
- ❌ Ignore type safety warnings or failing tests

---

## 🐛 Troubleshooting

### Skill doesn't appear in the list

**Solution:**
1. Verify that `SKILL.md` exists and has valid YAML frontmatter
2. Restart VS Code: `Cmd/Ctrl + Shift + P` → "Reload Window"
3. Check the log: `Help → Toggle Developer Tools → Console`

### Skill invokes now but throws an error

**Check:**
- Is Git initialized in the workspace?
- Do you have Node.js installed?
- Does the `SKILL.md` file have the correct structure?

```bash
# Debug
cat .github/skills/ai-native-sdlc-orchestrator/SKILL.md | head -20
```

### Templates aren't being used

**Make sure they exist in:**
```
.github/skills/ai-native-sdlc-orchestrator/templates/
├── intent.md
└── spec.md
```

---

## 📖 Recommended Reading

1. **[SKILL.md](./SKILL.md)** — Complete skill specification (read first!)
2. **[Phases.md](./docs/phases.md)** — Detail of each phase (if available)
3. **[Examples/](./examples/)** — Complete example projects

---

## 🤝 Contribution

This skill is **versioned and shareable**. To contribute:

```bash
git clone <repo>
cd ai-native-sdlc-orchestrator

# Make changes to SKILL.md or templates/
git add SKILL.md templates/
git commit -m "feat: adds support for React"
git push origin main
```

### What to contribute

- New templates for project types
- Refinement of phases
- Complete examples (feature, bug fix, etc.)
- Documentation and guides

---

## 📄 License

MIT — Free to use, modify, and distribute.

---

## 💡 Frequently Asked Questions

**Q: Can I use this skill without intent approval?**  
A: Not recommended. The `intent.md` approval is the first **human gate** — it prevents rework.

**Q: Does the skill work with languages other than TypeScript?**  
A: Yes! But templates and examples focus on TypeScript. We adapt as needed.

**Q: How do I integrate with CI/CD?**  
A: Phase 4 (Test) runs locally. Add a GitHub Action that calls the same scripts (`npm test`, `npm build`).

**Q: Can I start mid-loop (Phase 3 or 4)?**  
A: Yes, see the "Starting mid-loop" section in [SKILL.md](./SKILL.md#6-starting-mid-loop).

---

## 📞 Support

- **Issue:** Open an issue in the repository
- **Discussion:** Use Discussions for questions
- **Chat:** Copilot Chat in VS Code (`Ctrl+Shift+I`)

---

**Last updated:** 2026-09-01  
**Version:** 1.0.0
