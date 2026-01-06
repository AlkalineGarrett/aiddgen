# /aigen-add-rule

Generate a custom rule for specific AI behaviors.

## References
@aigen/rules/generator.mdc

## Approach

Conversational gathering - ask about the rule, then build it together:

1. Ask what the rule should govern
2. Ask for the key principles or constraints
3. Infer structure and scope
4. Show draft, let user refine
5. Generate rule file

## Behavior

### Step 1: Ask What

```
What should this rule govern?

(e.g., "how we structure API endpoints", "error handling", "naming conventions", "security for auth")
```

### Step 2: Ask Core Guidance

```
What are the key principles or constraints?

Tell me the main things the AI should always do, never do, or prefer.
```

Accept free-form description. User might say:
- "Always use plural nouns for REST endpoints"
- "Never expose internal IDs, use public slugs"
- "Prefer composition over inheritance"
- "Error messages should be user-friendly but log details internally"

### Step 3: Infer and Draft

Based on input, infer:
- Category (patterns/, security/, stack/, process/)
- Scope (always, specific file types, when referenced)
- Whether examples would help

```
I'll create: rules/[category]/[name].mdc

Applies to: [scope]

Principles:
1. [derived]
2. [derived]

Constraints:
- Must: [derived]
- Must not: [derived]

Want me to include code examples? Any adjustments?
```

### Step 4: Refine if Needed

User can:
- Add more principles
- Adjust constraints
- Request examples
- Change scope

### Step 5: Generate

Create rule file.

## Interpreting Descriptions

| User says | Infer |
|-----------|-------|
| "API endpoint structure" | patterns/api-design.mdc, REST conventions |
| "how we handle errors" | patterns/error-handling.mdc |
| "auth security stuff" | security/auth.mdc, stricter scope |
| "React component patterns" | stack/react-patterns.mdc, *.tsx files |
| "commit messages" | process/commits.mdc |
| "never use any type" | stack/typescript.mdc, add constraint |

## Output Generation

Generate [output]/rules/[category]/[name].mdc:

```markdown
---
description: [Derived from purpose]
alwaysApply: [true if "Always" scope]
globs: "[pattern]" (if file-specific scope)
---

# [Rule Name]

[Brief context paragraph if helpful]

## Principles

1. **[Principle]**: [Explanation]
2. **[Principle]**: [Explanation]
...

## Patterns

[If examples requested]

### [Pattern Name]
[When to use]

```[language]
// Good example
```

### Anti-pattern
```[language]
// Bad example - don't do this
```

## Constraints

Must:
- [constraint]

Must not:
- [constraint]
```

## Common Rules to Generate

Suggest these if user is unsure:

### Patterns
- API design conventions
- Error handling patterns
- Component structure
- File organization
- Naming conventions

### Security
- Authentication requirements
- Input validation rules
- Secret handling

### Process
- Commit message format
- Code review checklist
- Deployment procedures
