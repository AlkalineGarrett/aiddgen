---
description: List all AI command system generator commands
---

# /aiddgen-help

Display available aiddgen commands and workflow.

## Purpose

Generate a customized AI command system for your project. Generated commands and rules tell an AI assistant how to help with planning, reviewing, committing, and other tasks—calibrated to your context.

## Commands

AiddgenCommands {
  /aiddgen-init => "Initialize: describe project, get complete command system"
  /aiddgen-stack => "Add technology-specific rules (React, Python, etc.)"
  /aiddgen-add-command => "Add a custom command beyond defaults"
  /aiddgen-add-rule => "Add a custom rule for specific AI behaviors"
  /aiddgen-status => "Show current configuration"
  /aiddgen-help => "Show this help"
}

## Workflow

workflow() {
  1. /aiddgen-init
     AI analyzes codebase, estimates lifecycle
     You verify and set target lifecycle
     Generates commands calibrated to context

  2. /aiddgen-stack (optional)
     Describe tech stack for technology-specific guidance

  3. Done!
     AI command system ready to use

  4. Later: re-run /aiddgen-init when transitioning lifecycle stages
}

## What Gets Generated

GeneratedOutput {
  /aiddgen-init => {
    rules/core.mdc => "AI behaviors calibrated to lifecycle, risk, team"
    commands/*.md => "plan, task, review, commit, explain, debug, plus context-specific"
  }
}

CommandAdaptation {
  Each command adapts to context
  "PoC /review is lighter than Mature Production /review"
}

## Approach

ApproachPrinciples {
  Ask one key question, infer the rest
  Show suggestions, let user accept or adjust
  Accept free-form descriptions, not just menus
}

showHelp() {
  """
  ## AI Command System Generator

  ### Commands
  - `/aiddgen-init` - Initialize command system
  - `/aiddgen-stack` - Add technology rules
  - `/aiddgen-add-command` - Add custom command
  - `/aiddgen-add-rule` - Add custom rule
  - `/aiddgen-status` - Show configuration
  - `/aiddgen-help` - This help

  ### Quick Start
  1. Run `/aiddgen-init` to analyze and configure
  2. Run `/aiddgen-stack` to add tech-specific rules
  3. Use generated commands in your project
  """
}
