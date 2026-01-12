---
description: Generate a custom rule for specific AI behaviors
---

# /aigen-add-rule

Generate a custom rule for AI behaviors not covered by defaults.

## References

@aigen/rules/generator.mdc
@aigen/rules/guidance/sudolang-style.mdc

## Process

addRule() {
  askWhat |> askGuidance |> inferStructure |> draftRule |> refineIfNeeded |> generateRule
}

## Step 1: Ask What

askWhat() {
  """
  What should this rule govern?

  (e.g., "how we structure API endpoints", "error handling",
   "naming conventions", "security for auth")
  """
}

## Step 2: Ask Core Guidance

askGuidance() {
  """
  What are the key principles or constraints?

  Tell me the main things the AI should always do, never do, or prefer.
  """
}

AcceptFreeForm {
  "Always use plural nouns for REST endpoints"
  "Never expose internal IDs, use public slugs"
  "Prefer composition over inheritance"
  "Error messages should be user-friendly but log details internally"
}

## Step 3: Infer Structure

inferStructure() {
  From input, infer:
    Category => patterns/ | security/ | stack/ | process/
    Scope => always | specificFiles | whenReferenced
    NeedsExamples => boolean
}

RuleCategoryInference {
  "API endpoint structure" => patterns/api-design.mdc
  "how we handle errors" => patterns/error-handling.mdc
  "auth security stuff" => security/auth.mdc
  "React component patterns" => stack/react-patterns.mdc
  "commit messages" => process/commits.mdc
  "never use any type" => stack/typescript.mdc
}

## Step 4: Draft Rule

draftRule() {
  """
  I'll create: rules/${category}/${name}.mdc

  Applies to: ${scope}

  Principles:
  ${derivedPrinciples}

  Constraints:
  - Must: ${mustDo}
  - Must not: ${mustNotDo}

  Want me to include code examples? Any adjustments?
  """
}

## Step 5: Refine

refineIfNeeded() {
  User can:
    Add more principles
    Adjust constraints
    Request examples
    Change scope
}

## Step 6: Generate

generateRule() {
  Write [output]/rules/${category}/${name}.mdc using SudoLang
}

## Output Template

ruleTemplate() {
  """
  ---
  description: ${derivedDescription}
  alwaysApply: ${isAlways}
  globs: "${pattern}" (if file-specific)
  ---

  # ${RuleName}

  ${contextParagraph}

  ## Principles

  PrincipleRules {
    ${principle1} => ${explanation1}
    ${principle2} => ${explanation2}
  }

  ## Patterns

  ${patternName}() {
    \"\"\"
    // Good example
    ${goodCode}
    \"\"\"
  }

  antiPattern() {
    \"\"\"
    // Bad - don't do this
    ${badCode}
    \"\"\"
  }

  Constraints {
    Must: ${mustConstraints}
    Must not: ${mustNotConstraints}
  }
  """
}

## Common Rules

CommonRuleSuggestions {
  Patterns => API design, error handling, component structure, file org, naming
  Security => auth requirements, input validation, secret handling
  Process => commit format, review checklist, deployment procedures
}

(user unsure) => suggest from CommonRuleSuggestions

Constraints {
  Generated rule MUST use SudoLang patterns.
  Infer category and scope from description.
  Offer examples when patterns would clarify.
  Keep rules focused on one concern.
}
