# /aigen-status

Show current configuration and what's been generated.

## Behavior

1. Check if output directory exists
2. If not initialized, suggest running /aigen-init
3. If initialized, read core.mdc to extract strategic choices
4. List all generated rules and commands
5. If transitioning, show progress indicators
6. Report status

## Output

### If Not Initialized
```
No AI command system found.

Run /aigen-init to create one.
```

### If Initialized

```
AI Command System Status
========================

Location: [output directory]

Strategic Context:
  Current Lifecycle: [value from core.mdc]
  Target Lifecycle:  [value from core.mdc]
  Risk Domain:       [value from core.mdc]
  Team Context:      [value from core.mdc]
  Velocity:          [value from core.mdc]
  Scale:             [value from core.mdc]

[If transitioning:]
  Transition: [current] → [target]
  AI is calibrated to build [target]-grade practices incrementally.

Generated Rules:
  rules/core.mdc              - Core AI behaviors
  rules/stack/react.mdc       - React guidelines
  [etc.]

Generated Commands:
  commands/help.md     - /help
  commands/plan.md     - /plan
  commands/task.md     - /task
  commands/review.md   - /review
  [etc.]

Available Actions:
  /aigen-stack       - Add more technology rules
  /aigen-add-command - Create a new command
  /aigen-add-rule    - Create a custom rule
  /aigen-init        - Update lifecycle (e.g., after reaching target)
```

## Suggestions

If current equals target and codebase shows improvement:
```
Note: Your codebase appears to have reached [target] practices.
Consider running /aigen-init to reassess and set a new target if desired.
```

If rules exist but core.mdc is missing strategic context:
```
Warning: core.mdc exists but may be incomplete.
Run /aigen-init to establish strategic context.
```
