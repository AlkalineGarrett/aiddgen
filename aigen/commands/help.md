---
description: List all AI command system generator commands
---

# /aigen-help

Display available aigen commands and workflow.

## Purpose

Generate a customized AI command system for your project. Generated commands and rules tell an AI assistant how to help with planning, reviewing, committing, and other tasks—calibrated to your context.

## Commands

AigenCommands {
  /aigen-init => "Initialize: describe project, get complete command system"
  /aigen-stack => "Add technology-specific rules (React, Python, etc.)"
  /aigen-add-command => "Add a custom command beyond defaults"
  /aigen-add-rule => "Add a custom rule for specific AI behaviors"
  /aigen-status => "Show current configuration"
  /aigen-help => "Show this help"
}

## Workflow

workflow() {
  1. /aigen-init
     AI analyzes codebase, estimates lifecycle
     You verify and set target lifecycle
     Generates commands calibrated to context

  2. /aigen-stack (optional)
     Describe tech stack for technology-specific guidance

  3. Done!
     AI command system ready to use

  4. Later: re-run /aigen-init when transitioning lifecycle stages
}

## What Gets Generated

GeneratedOutput {
  /aigen-init => {
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
  - `/aigen-init` - Initialize command system
  - `/aigen-stack` - Add technology rules
  - `/aigen-add-command` - Add custom command
  - `/aigen-add-rule` - Add custom rule
  - `/aigen-status` - Show configuration
  - `/aigen-help` - This help

  ### Quick Start
  1. Run `/aigen-init` to analyze and configure
  2. Run `/aigen-stack` to add tech-specific rules
  3. Use generated commands in your project
  """
}
