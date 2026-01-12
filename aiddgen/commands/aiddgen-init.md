---
description: Initialize a new AI command system through guided conversation
---

# /aiddgen-init

Initialize or update an AI command system calibrated to project context.

## References

@aiddgen/rules/generator.mdc
@aiddgen/reference/sudolang-style.mdc
@aiddgen/rules/guidance/choice-hierarchy.mdc
@aiddgen/rules/choices/*.mdc

## Process

initProcess() {
  checkExisting |> gatherL1L2 |> generateChoices |> generateCore |> generateCommands |> runStack
}

## Gather L1-L2

gatherL1L2() {
  analyzeCodebase |> verifyWithUser |> askTarget |> suggestL2Defaults
}

analyzeCodebase() {
  (new/empty repo) => askProjectType
  (existing code) => detectLifecycle from signals (tests, structure, CI, docs)
}

verifyWithUser() {
  Present estimated lifecycle with evidence, accept corrections
  "A codebase can have poor practices despite being in production"
}

askTarget() {
  Which lifecycle stage are you targeting?
  Accept menu selection or free-form description
}

suggestL2Defaults() {
  Infer L2 from target lifecycle per @choice-hierarchy.mdc InferenceChains
  Present suggestions, accept overrides
  Mark each as explicit or inferred
}

## Output Generation

generateChoices() {
  Write [output]/choices.mdc per @choice-hierarchy.mdc choicesTemplate
}

generateCore() {
  Derive [output]/rules/core.mdc from choices:
    AI behavioral calibration from L1 + L2
    Transition rules if current != target
}

generateCommands() {
  AlwaysInclude { /help, /plan, /task, /review, /commit, /explain, /debug }
  IncludeByContext { based on lifecycle and risk level }
  Calibrate depth to context
}

## Stack Integration

runStack() {
  Execute /aiddgen-stack flow inline to gather L3-L5
}

## Completion

reportCompletion() {
  List generated files, show choices summary, note transition if applicable
}

Constraints {
  All generated files MUST use SudoLang patterns.
  choices.mdc is source of truth; core.mdc derives from it.
  Record explicit vs inferred for every choice.
  Automatically flow into stack configuration.
  Accept free-form descriptions, not just menus.
}
