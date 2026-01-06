# /aigen-init

Initialize a new AI command system through guided conversation.

Also used to update an existing setup when transitioning between lifecycle stages.

## References
@aigen/rules/generator.mdc
@aigen/rules/choices/lifecycle.mdc
@aigen/rules/choices/risk-domain.mdc
@aigen/rules/choices/team-context.mdc
@aigen/rules/choices/change-velocity.mdc
@aigen/rules/choices/scale.mdc

## Approach

1. Analyze the codebase to estimate current lifecycle stage
2. Present findings and verify with user (analysis may be wrong)
3. Ask about target lifecycle (may differ from current)
4. Infer defaults for other dimensions based on target
5. Let user accept or adjust
6. Generate rules and commands calibrated to target, aware of current state

## Behavior

1. Check for existing setup (offer to update or start fresh)
2. Analyze codebase to estimate current lifecycle
3. Verify current state with user
4. Ask about target lifecycle
5. Based on target, suggest values for risk, team, velocity, scale
6. Generate core.mdc with synthesized behaviors
7. Generate default commands (adapted to context)
8. Report what was created

## Step 1: Check Existing Setup

If output directory already exists:
```
Found existing AI command system in ./ai

Would you like to:
1. Update it (e.g., transitioning lifecycle stages)
2. Start fresh (replace existing)
```

## Step 2: Analyze Codebase (or detect new repo)

First, determine if this is a new or existing codebase:

### If New/Empty Repo

```
This appears to be a new project (no source code detected).

What are you building?
1. Web application
2. API / Backend service
3. CLI tool
4. Library / Package
5. Mobile app
6. Something else: [describe]
```

Then ask about target lifecycle directly (skip current state analysis):
```
What stage are you starting at?
1. Proof of Concept - validate an idea quickly
2. MVP - first version for real users
3. Production-ready - building for reliability from the start
```

For new repos, "current" and "target" lifecycle will be the same initially.

### If Existing Codebase

Examine the codebase to estimate current lifecycle stage. Look for signals:

| Signal | Indicates |
|--------|-----------|
| No tests, minimal structure | PoC / Throwaway |
| Some tests, basic error handling | Team Tool / MVP |
| Good test coverage, CI/CD, docs | Early Production |
| Comprehensive tests, security practices, monitoring | Mature Production |
| Old patterns, cautious changes, legacy deps | Legacy |

Also look for:
- README quality and completeness
- Error handling patterns (try/catch, error boundaries, validation)
- Security practices (input validation, auth patterns, no secrets in code)
- Documentation (comments, API docs, architecture docs)
- Git history (commit discipline, PR usage)
- Dependencies (up to date? security scanning?)
- CI/CD configuration
- Logging/monitoring setup

## Step 3: Present Analysis and Verify

```
Based on my analysis of your codebase:

Current state appears to be: [estimated lifecycle]

Evidence:
- [observation 1]
- [observation 2]
- [observation 3]

Is this accurate? Or is the actual state different?

(Note: A codebase can have poor practices despite being in production,
or good practices despite being a prototype. Tell me the reality.)
```

Accept corrections like:
- "It's actually in production but we have tech debt"
- "It's a prototype but I'm being careful because it handles payments"
- "That's right"

## Step 4: Ask About Target Lifecycle

```
What lifecycle stage are you targeting?

1. Stay at current ([current]) - optimize for where you are
2. Proof of Concept - validate fast, likely throwaway
3. Throwaway Tool - solve immediate problem
4. Team Tool - used by a small known group
5. MVP - first version for real users
6. Early Production - real users, growing
7. Mature Production - stable, reliable, many depend on it
8. Legacy - maintain but not actively developing

Or describe your goal (e.g., "transition from MVP to early production")
```

## Step 5: Suggest Remaining Dimensions

Based on target lifecycle, suggest sensible defaults for the other dimensions.

Use @aigen/rules/choices/ to understand what each option implies:
- risk-domain.mdc - consequences of failure
- team-context.mdc - who works on this
- change-velocity.mdc - how often it changes
- scale.mdc - expected usage

Present suggestions with brief reasoning:
```
Based on "[lifecycle]", I suggest:

- Risk: [value] - [one-line why]
- Team: [value] - [one-line why]
- Velocity: [value] - [one-line why]
- Scale: [value] - [one-line why]

Accept these? Or tell me what's different about your situation.
```

Earlier lifecycle stages (PoC, Throwaway) default toward simpler options.
Later stages (Production, Mature) default toward more rigorous options.

## Handling Custom Descriptions

If the user provides free-form text instead of selecting an option, interpret it. Look for:
- Combinations that override defaults (e.g., "prototype but handles payments" → PoC + Financial risk)
- Transitions or future state (e.g., "might open source later" → note in rules)
- Context that affects multiple dimensions (e.g., "internal but company-wide" → Internal risk, Organization scale)

Synthesize into coherent behavioral rules, not just the closest canonical option.

## Output Generation

After collecting choices, generate:

### [output]/rules/core.mdc

Synthesize all choices into concrete AI behaviors:

1. State project context: current state, target state, and any transition goals
2. List concrete behaviors for error handling, testing, security, documentation, complexity, performance (use @aigen/rules/choices/*.mdc for what each lifecycle implies)
3. Include constraints (must/must not)
4. If transitioning, differentiate rules for existing code (lenient, boy scout rule) vs new code (target-grade)

### [output]/commands/ - Default Commands

Generate these commands, adapted to the project context:

#### Always included:
- `/help` - List available commands
- `/plan` - Review current plan, suggest next steps
- `/task` - Break down and plan a task or feature
- `/review` - Review code changes (depth adapted to lifecycle/risk)
- `/commit` - Draft commit message for staged changes
- `/explain` - Explain code, architecture, or decisions

#### Included based on context:

| Command | When to include |
|---------|-----------------|
| `/discover` | Team Tool or higher (product thinking matters) |
| `/document` | Team context involves others (not solo throwaway) |
| `/security-review` | Risk is Business or higher |
| `/refactor` | Velocity is not Prototype (code will live) |
| `/debug` | Always (everyone needs help debugging) |
| `/architect` | MVP or higher (architecture decisions matter) |
| `/estimate` | Team Tool or higher (planning with others) |
| `/onboard` | Large Team or Open Source (newcomers exist) |
| `/pr` | Team context involves code review |
| `/changelog` | Early Production or higher (tracking releases) |

Each generated command references core.mdc and adapts its behavior:
- PoC `/review` is lighter than Mature Production `/review`
- Solo `/commit` is simpler than Team `/commit`
- High-risk `/security-review` is more thorough

### [output]/commands/help.md

Lists all generated commands with brief descriptions.

## Completion

Report:
```
Created AI command system in [output]/

Generated:
  rules/core.mdc       - Core AI behaviors
  commands/
    help.md            - /help
    plan.md            - /plan
    task.md            - /task
    review.md          - /review
    commit.md          - /commit
    explain.md         - /explain
    [context-specific commands...]

Your context:
  Current lifecycle: [analyzed/verified value]
  Target lifecycle:  [selected value]
  Risk: [value]
  Team: [value]
  Velocity: [value]
  Scale: [value]

[If transitioning:]
  Transition: [current] → [target]
  AI will help you build [target]-grade practices incrementally.

Ready to use! Run /aigen-stack to add technology-specific rules.

To update later (e.g., after reaching your target):
  Run /aigen-init again to reassess and adjust.
```
