# /aigen-stack

Generate technology-specific rules based on your stack, calibrated to your lifecycle context.

## References
@aigen/rules/generator.mdc
@aigen/rules/guidance/programming-principles.mdc
@aigen/rules/guidance/testing-methodology.mdc
@aigen/rules/guidance/security-patterns.mdc

## Prerequisites

Run `/aigen-init` first to establish strategic context. Stack rules build on core behaviors and lifecycle.

## Approach

**For existing codebases:**
1. Analyze codebase to detect current tech stack
2. Present findings and let user verify/adjust
3. Generate stack rules calibrated to technology + lifecycle

**For new/empty repos:**
1. Ask highest-level question first (language/ecosystem preference)
2. Recommend complete stack based on project type + lifecycle + ecosystem
3. Let user accept, adjust, or specify exactly
4. Generate stack rules with setup guidance

## Behavior

### Step 1: Detect New vs Existing Codebase

First, determine if this is a new or existing project.

---

## Flow A: New/Empty Repo

If no source code is detected, guide the user through stack selection with high-level questions first.

### A1: Confirm Project Type (from init)

Reference what was captured in `/aigen-init`:
```
From your init setup:
- Building: [Web app / API / CLI / Library / etc.]
- Target lifecycle: [PoC / MVP / Production-ready]
- Risk level: [Personal / Business / etc.]

Let's choose your tech stack.
```

### A2: Language/Ecosystem Preference

Start with the highest-level question:
```
What language or ecosystem do you want to work in?

1. JavaScript/TypeScript (Node, React, etc.)
2. Python
3. Go
4. Rust
5. Ruby
6. Java/Kotlin
7. C# / .NET
8. Other: [specify]
9. No preference - recommend based on my project type
```

### A3: Recommend Stack Based on Context

Based on project type + lifecycle + ecosystem, recommend a complete stack.

**Recommendation criteria:**

| Factor | How it affects recommendation |
|--------|------------------------------|
| Lifecycle (PoC) | Minimize setup, batteries-included, simple storage |
| Lifecycle (MVP) | Balance speed with foundation, easy deployment |
| Lifecycle (Production) | Reliability, observability, team scalability |
| Project type | Appropriate frameworks for web/API/CLI/library |
| Ecosystem | Stay within user's preferred language |
| Team familiarity | Weight toward what they already know |
| Risk level | Higher risk → more established/proven choices |

**Present recommendations with:**
- What you're recommending and why
- What the stack optimizes for given their context
- Clear invitation to adjust or specify differently

### A4: Let User Specify Exactly

User can override recommendations at any point:
- "I need to use X because that's what our team knows"
- "We're adding to an existing Y system"
- "I've already decided on Z"

Adjust accordingly and ask follow-up questions if needed (e.g., version, existing patterns to match).

### A5: Generate Stack Rules for New Project

Generate stack rules that include:
- Setup guidance (how to initialize the project)
- Directory structure recommendations
- Initial patterns to follow
- Lifecycle-appropriate constraints

---

## Flow B: Existing Codebase

### B1: Analyze Codebase

Examine the codebase to detect technologies by looking for:
- Package manifests (package.json, requirements.txt, go.mod, Cargo.toml, Gemfile, etc.)
- Framework config files
- Language config files (tsconfig, etc.)
- Test framework config
- CI/CD config
- Deployment config
- Database/ORM config
- Code patterns (imports, directory structure)

### B2: Present Findings and Verify

Present detected stack organized by category:
- Language/runtime
- Framework(s)
- Database/storage
- Testing
- Deployment
- Other significant libraries

Ask user to confirm, add, remove, or correct.

Accept corrections like:
- "We're also using X for Y"
- "Remove X, we actually use Y"
- "We're migrating from X to Y"

### B3: Generate Lifecycle-Aware Stack Rules

Generate rules calibrated to BOTH the tech stack AND the lifecycle context from core.mdc.

Show what rules will reflect:
- How each technology's rules are calibrated to lifecycle
- What's stricter for new code vs existing code (if transitioning)
- How risk level affects the rules

Ask user to confirm or adjust before generating.

## Lifecycle-Calibrated Stack Rules

Stack rules should vary based on lifecycle. The same technology needs different rules at different stages:

| Aspect | PoC | MVP | Early Prod | Mature | Legacy |
|--------|-----|-----|------------|--------|--------|
| Type safety | Loose, speed matters | Prefer types, shortcuts ok | Strict on new code | Strict everywhere | Preserve existing |
| Patterns | Whatever works | Basic structure | Consistent patterns | Strict patterns | Preserve, modernize incrementally |
| Testing | Skip unless needed | Critical paths | New code tested, add when modifying | Comprehensive | Add tests before any change |
| Data/schema | Change freely | Basic migrations | Migration safety | Thorough review, rollback plans | Extreme caution |
| Error handling | Minimal | User-facing paths | Comprehensive | Thorough + monitoring | Preserve behavior |
| Dependencies | Whatever helps | Reasonable choices | Evaluate carefully | Proven, stable | Minimize changes |

---

## Technology Categories

When generating rules, consider these aspects based on what the user mentions:

### Frontend
- Framework (React, Vue, Svelte, Angular, etc.)
- State management (built-in, Redux, Zustand, etc.)
- Styling (Tailwind, CSS Modules, styled-components, etc.)
- Build tool (Vite, webpack, framework CLI)

### Backend
- Runtime/language (Node, Python, Go, Ruby, etc.)
- Framework (Express, FastAPI, Rails, etc.)
- API style (REST, GraphQL, tRPC)

### Data
- Database (PostgreSQL, MongoDB, SQLite, etc.)
- ORM/query layer (Prisma, SQLAlchemy, raw SQL)
- Caching if mentioned

### Infrastructure
- Deployment (Vercel, AWS, Railway, Docker, etc.)
- CI/CD if mentioned

### Quality
- Testing framework (Vitest, Jest, pytest, etc.)
- Linting/formatting tools

Only generate rules for technologies the user actually mentions or confirms.

---

## Incorporating Guidance

When generating stack rules, incorporate principles from the guidance files, adapted to the specific technology:

### Programming Principles (@aigen/rules/guidance/programming-principles.mdc)

For each language/framework rule, include:
- **Core principles** (DOT, YAGNI, KISS, DRY, SDA) adapted to language idioms
- **Style preferences** (immutability, composition, declarative style) where language supports
- **Naming conventions** adapted to language conventions (camelCase, snake_case, etc.)
- **Comment guidelines** calibrated to team context

**Calibration**: Scale strictness with lifecycle (PoC = relaxed, Production = strict)

### Testing Methodology (@aigen/rules/guidance/testing-methodology.mdc)

For each testing framework rule, include:
- **5 Questions** structure adapted to framework syntax
- **RITE Way** principles (Readable, Isolated, Thorough, Explicit)
- **Assertion patterns** using framework-specific matchers
- **Test organization** following framework conventions

**Required sections in testing rules:**
- Given/should or arrange/act/assert structure
- Isolation requirements (no shared mutable state)
- Factory pattern over fixtures
- State management testing patterns (if applicable)

### Security Patterns (@aigen/rules/guidance/security-patterns.mdc)

For each security-relevant rule, include:
- **Secret comparison** via hash-then-compare (language-specific implementation)
- **Input validation** patterns for the framework
- **Output encoding** using framework's built-in escaping
- **OWASP awareness** calibrated to risk level

**Risk level determines:**
- Personal/Internal: Basic patterns, flag obvious issues
- Business: Full patterns, require validation/encoding
- Financial/Healthcare: Paranoid patterns, audit logging, compliance considerations

---

## Output Generation

For each confirmed technology, generate a rule file in [output]/rules/stack/[tech].mdc

Each generated rule should:
1. State the lifecycle context it's calibrated for
2. Reference the technology's current conventions and best practices
3. Include lifecycle-appropriate patterns and constraints
4. Differentiate between existing code and new code (if transitioning)

Structure:
```markdown
---
description: [Technology] guidelines for [lifecycle context]
globs: "[appropriate file patterns]"
---

# [Technology]

## Context
Current: [lifecycle] | Target: [lifecycle]
[Brief description of what this means for this technology]

## Patterns
[Lifecycle-appropriate patterns for this technology]

## New Code
[Rules for new code - calibrated to target lifecycle]

## Existing Code (if transitioning)
[Rules for existing code - typically more lenient, boy scout rule]

## Constraints
[Hard rules calibrated to lifecycle + risk level]
```

## Completion

Report:
```
Generated stack rules calibrated to your lifecycle:

  Current: [lifecycle] | Target: [lifecycle]
  Risk: [risk level]

  rules/stack/[technology].mdc  - [for each confirmed technology]

Stack rules reference your core.mdc context and will guide
AI behavior appropriately for your project's stage.

To update stack rules after lifecycle transition:
  Run /aigen-stack again after updating /aigen-init
```
