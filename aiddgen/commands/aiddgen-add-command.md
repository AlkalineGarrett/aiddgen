---
description: Add a custom command beyond defaults
---

# /aiddgen-add-command

Add a command for workflows not covered by defaults.

## References

@aiddgen/rules/generator.mdc
@aiddgen/reference/sudolang-style.mdc

## Process

addCommand() {
  askPurpose |> inferStructure |> confirmWithUser |> generateCommand
}

## Step 1: Ask

askPurpose() {
  What should AI help with that isn't covered by existing commands?
}

inferStructure() {
  From description, infer: name, behavior, references, guardrails
}

generateCommand() {
  Write [output]/commands/${name}.md using SudoLang
  Update [output]/commands/help.md
}

Constraints {
  Generated command MUST use SudoLang patterns.
  Reference core.mdc for inherited behaviors.
  Calibrate to lifecycle from choices.mdc.
}
