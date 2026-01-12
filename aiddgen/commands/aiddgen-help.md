---
description: List aiddgen commands
---

# /aiddgen-help

## Commands

AiddgenCommands {
  /aiddgen-init => "Full setup: lifecycle + stack"
  /aiddgen-stack => "Modify stack after setup"
  /aiddgen-add-command => "Add custom command"
  /aiddgen-add-rule => "Add custom rule"
  /aiddgen-status => "Show configuration"
  /aiddgen-help => "This help"
}

## Workflow

1. /aiddgen-init - analyzes codebase, gathers choices, generates everything
2. Later: /aiddgen-stack to modify, /aiddgen-init to change lifecycle

## Choice Hierarchy

L1: Lifecycle → L2: Parameters → L3: Ecosystem → L4: Stack → L5: Libraries

Higher informs lower; explicit overrides inferred; choices.mdc is source of truth.
