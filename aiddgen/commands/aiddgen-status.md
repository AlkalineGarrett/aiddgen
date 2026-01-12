---
description: Show current configuration and choice hierarchy
---

# /aiddgen-status

Display choices and generated files.

## Process

showStatus() {
  (no choices.mdc) => "Run /aiddgen-init first"
  (choices.mdc exists) => readChoices |> listGenerated |> showSuggestions
}

readChoices() {
  Parse [output]/choices.mdc for L1-L5 with explicit/inferred markers
}

listGenerated() {
  Enumerate choices.mdc, rules/*.mdc, rules/stack/*.mdc, commands/*.md
}

showSuggestions() {
  (L3-L5 missing) => "Run /aiddgen-stack"
  (current == target && practices improved) => "Consider reassessing lifecycle"
}
