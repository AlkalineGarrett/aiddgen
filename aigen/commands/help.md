# /aigen-help

List all available AI command system generator commands.

## Purpose

Generate a customized AI command system for your project. The generated commands and rules tell an AI assistant how to help with planning, reviewing, committing, and other software engineering tasks—calibrated to your project's context.

## Commands

- `/aigen-init` - Initialize: describe your project, get a complete command system
- `/aigen-stack` - Add technology-specific rules (React, Python, etc.)
- `/aigen-add-command` - Add a custom command beyond the defaults
- `/aigen-add-rule` - Add a custom rule for specific AI behaviors
- `/aigen-status` - Show current configuration
- `/aigen-help` - Show this help

## Workflow

1. `/aigen-init` - AI analyzes your codebase to estimate current lifecycle stage, you verify and set target lifecycle, then it generates commands calibrated to your context.

2. `/aigen-stack` - (Optional) Describe your tech stack for technology-specific guidance.

3. Done! Your AI command system is ready to use.

4. Later: Re-run `/aigen-init` when transitioning lifecycle stages (e.g., MVP → Early Production) to update your AI command behaviors.

## What Gets Generated

`/aigen-init` generates:
- `rules/core.mdc` - AI behaviors calibrated to your lifecycle, risk, team, etc.
- `commands/*.md` - AI commands (plan, task, review, commit, explain, debug, plus context-specific ones)

Each command adapts to your context. A `/review` for a proof-of-concept is lighter than for mature production.

## Approach

Minimize required input:
- Ask one key question, infer the rest
- Show suggestions, let you accept or adjust
- Accept free-form descriptions, not just menu selections
