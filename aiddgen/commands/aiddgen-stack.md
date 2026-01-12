---
description: Configure technology stack and generate technology-specific rules
---

# /aiddgen-stack

Configure L3-L5 choices and generate stack rules.

## References

@aiddgen/rules/generator.mdc
@aiddgen/reference/sudolang-style.mdc
@aiddgen/rules/guidance/choice-hierarchy.mdc
@aiddgen/rules/guidance/programming-principles.mdc
@aiddgen/rules/guidance/testing-methodology.mdc
@aiddgen/rules/guidance/security-patterns.mdc
@aiddgen/rules/guidance/spec-driven-generation.mdc

## Process

stackProcess() {
  loadChoices |> detectOrAsk |> confirmStack |> updateChoices |> generateStackRules |> generateSpecPatterns |> generateEffectRules
}

(no choices.mdc) => "Run /aiddgen-init first"

loadChoices() {
  Read L1-L2 from [output]/choices.mdc for calibration context
}

detectOrAsk() {
  (no source code) => askEcosystem |> recommendStack
  (source code exists) => analyzeStack |> presentFindings
}

## Detection

analyzeStack() {
  PackageManifests, FrameworkConfig, LanguageConfig, StateManagement, TestFramework, CICD, Deployment, Database
}

presentFindings() {
  Show detected L3 (ecosystem), L4 (stack including state management), L5 (libraries)
  Default state management if not detected but app has state needs
  Accept corrections: add, remove, or replace
}

## Output

updateChoices() {
  Update [output]/choices.mdc with L3-L5, marking detected vs explicit
}

generateStackRules() {
  For each confirmed technology, write [output]/rules/stack/${tech}.mdc
  Incorporate guidance files, calibrate to L1-L2 context
}

## Spec-Driven Code Generation Patterns

generateCodeGenerationSpecs() {
  For each slice of the L4 stack => identifyBoilerplatePatterns |> generateCodeGenerationSpecs
}

generateCodeGenerationSpecs() {
  Generate [output]/rules/patterns/${pattern}.mdc with spec format + generation rules
  Generate [output]/commands/${pattern}.md to invoke it
  Add to /help output
}

## Effect Isolation

generateEffectRules() {
  (async in stack) => generate [output]/rules/effects.mdc

  EffectPatterns {
    IO/Network => saga-style yield pattern (call/put)
    SideEffects => isolate at boundaries, pure core
    Testing => drive iterators without executing effects
  }
}

Constraints {
  Generated rules MUST use SudoLang patterns.
  Read L1-L2 from choices.mdc, don't re-ask.
  Update choices.mdc with L3-L5.
  Calibrate strictness to lifecycle stage.
  Mark detected vs explicit choices.
  Generate concrete spec patterns for detected stack, not just guidance.
  Generate effect isolation rules for async ecosystems.
}
