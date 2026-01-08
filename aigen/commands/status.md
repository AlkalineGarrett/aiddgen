---
description: Show current configuration and what's been generated
---

# /aigen-status

Show current AI command system configuration and generated files.

## Process

showStatus() {
  checkInitialized |> readContext |> listGenerated |> reportStatus
}

checkInitialized() {
  (output dir missing) => notInitializedMessage()
  (output dir exists) => readCoreMdc()
}

notInitializedMessage() {
  """
  No AI command system found.

  Run /aigen-init to create one.
  """
}

readContext() {
  Parse core.mdc to extract:
    CurrentLifecycle, TargetLifecycle
    Risk, Team, Velocity, Scale
    IsTransition
}

listGenerated() {
  Enumerate:
    rules/*.mdc
    rules/stack/*.mdc
    commands/*.md
}

## Output

statusOutput() {
  """
  AI Command System Status
  ========================

  Location: ${outputDir}

  Strategic Context:
    Current Lifecycle: ${current}
    Target Lifecycle:  ${target}
    Risk Domain:       ${risk}
    Team Context:      ${team}
    Velocity:          ${velocity}
    Scale:             ${scale}

  ${transitionStatus}

  Generated Rules:
    ${rulesList}

  Generated Commands:
    ${commandsList}

  Available Actions:
    /aigen-stack       - Add more technology rules
    /aigen-add-command - Create a new command
    /aigen-add-rule    - Create a custom rule
    /aigen-init        - Update lifecycle
  """
}

transitionStatus() {
  (isTransition) => """
  Transition: ${current} → ${target}
  AI is calibrated to build ${target}-grade practices incrementally.
  """
}

## Suggestions

checkForSuggestions() {
  (current == target && codebaseImproved) => """
  Note: Your codebase appears to have reached ${target} practices.
  Consider running /aigen-init to reassess and set a new target.
  """

  (rules exist && context incomplete) => """
  Warning: core.mdc exists but may be incomplete.
  Run /aigen-init to establish strategic context.
  """
}

Constraints {
  Read configuration from generated files, don't assume.
  Show clear status with available next actions.
  Detect and suggest when reassessment may be needed.
}
