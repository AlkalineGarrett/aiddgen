---
description: Add a custom command beyond the defaults
---

# /aigen-add-command

Add a custom command for workflows not covered by /aigen-init defaults.

## References

@aigen/rules/generator.mdc
@aigen/rules/guidance/sudolang-style.mdc

## Process

addCommand() {
  askPurpose |> inferStructure |> confirmWithUser |> generateCommand
}

## Step 1: Ask

askPurpose() {
  """
  What do you want the AI to help with that isn't covered by existing commands?

  Existing: ${existingCommands}
  """
}

## Step 2: Infer

inferStructure() {
  From description, infer:
    CommandName => verb-based, short
    Behavior => what AI should do
    References => which existing rules apply
    Guardrails => appropriate constraints
}

confirmInference() {
  """
  I'll create: /${commandName}

  Behavior:
  ${numberedSteps}

  References: core.mdc, ${otherRelevantRules}
  Guardrails: ${inferredGuardrails}

  Create this? Or tell me what to adjust.
  """
}

## Step 3: Generate

generateCommand() {
  Write [output]/commands/${name}.md using SudoLang
  Update [output]/commands/help.md
}

## Example Custom Commands

CustomCommandExamples {
  "help me write API documentation" => /api-docs
  "analyze test coverage gaps" => /coverage
  "help migrate from X to Y" => /migrate
  "generate mock data for testing" => /mock-data
  "help write release notes" => /release-notes
  "audit dependencies for issues" => /audit-deps
  "help with database schema changes" => /schema
}

## Output Template

commandTemplate() {
  """
  ---
  description: ${purpose}
  ---

  # /${name}

  ${description}

  ## References
  @rules/core.mdc
  ${additionalReferences}

  ## Process

  ${commandName}() {
    ${pipeComposition}
  }

  ${stepFunctions}

  ## Output

  ${outputTemplate}

  Constraints {
    ${derivedConstraints}
  }
  """
}

## If Unsure

(user unsure what to add) => {
  Reference "Included based on context" table from init.md
  These may not have been generated based on choices
  Or describe workflow need for custom command
}

Constraints {
  Generated command MUST use SudoLang patterns.
  Reference core.mdc for inherited behaviors.
  Infer appropriate guardrails from context.
  Update help.md with new command.
}
