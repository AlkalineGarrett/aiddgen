---
description: Add a custom rule for specific AI behaviors
---

# /aiddgen-add-rule

Add a rule for behaviors not covered by defaults.

## References

@aiddgen/rules/generator.mdc
@aiddgen/reference/sudolang-style.mdc

## Process

addRule() {
  askWhat |> askGuidance |> inferStructure |> draftRule |> refineIfNeeded |> generateRule
}

askWhat() {
  What should this rule govern? (API design, error handling, naming, security, etc.)
}

askGuidance() {
  What are the key principles or constraints? (always do, never do, prefer)
}

inferStructure() {
  From input, infer: category (patterns/security/stack/process), scope, needs examples
}

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

Constraints {
  Generated rule MUST use SudoLang patterns.
  Keep rules focused on one concern.
  Calibrate to lifecycle from choices.mdc.
}
