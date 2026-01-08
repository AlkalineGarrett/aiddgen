---
description: Initialize a new AI command system through guided conversation
---

# /aigen-init

Initialize or update an AI command system calibrated to project context.

## References

@aigen/rules/generator.mdc
@aigen/rules/guidance/sudolang-style.mdc
@aigen/rules/choices/lifecycle.mdc
@aigen/rules/choices/risk-domain.mdc
@aigen/rules/choices/team-context.mdc
@aigen/rules/choices/change-velocity.mdc
@aigen/rules/choices/scale.mdc
@aigen/rules/guidance/product-management.mdc
@aigen/rules/guidance/task-lifecycle.mdc

## State

InitState {
  OutputDir         // Where to generate (default: ./ai)
  CurrentLifecycle  // Analyzed from codebase
  TargetLifecycle   // User's goal
  Risk              // Consequence of failure
  Team              // Who works on this
  Velocity          // How often it changes
  Scale             // Expected usage
  IsTransition      // CurrentLifecycle != TargetLifecycle
}

## Process

initProcess() {
  checkExisting |> analyzeCodebase |> verifyWithUser |> askTarget |> suggestDefaults |> generateOutput
}

## Step 1: Check Existing

checkExisting() {
  (output dir exists) => askUpdateOrFresh()
}

askUpdateOrFresh() {
  """
  Found existing AI command system in ${outputDir}

  Would you like to:
  1. Update it (e.g., transitioning lifecycle stages)
  2. Start fresh (replace existing)
  """
}

## Step 2: Analyze Codebase

analyzeCodebase() {
  (new/empty repo) => askProjectType |> askStartingLifecycle
  (existing code) => detectLifecycle |> gatherEvidence
}

detectLifecycle() {
  LifecycleSignals {
    NoTests + MinimalStructure => PoC | Throwaway
    SomeTests + BasicErrorHandling => TeamTool | MVP
    GoodCoverage + CICD + Docs => EarlyProduction
    ComprehensiveTests + Security + Monitoring => MatureProduction
    OldPatterns + CautiousChanges => Legacy
  }

  Also examine:
    README quality, error handling patterns, security practices
    Documentation, git history, dependencies, CI/CD, logging
}

askProjectType() {
  """
  This appears to be a new project.

  What are you building?
  1. Web application
  2. API / Backend service
  3. CLI tool
  4. Library / Package
  5. Mobile app
  6. Something else: [describe]
  """
}

## Step 3: Verify With User

verifyWithUser() {
  """
  Based on my analysis:

  Current state appears to be: ${estimatedLifecycle}

  Evidence:
  - ${observation1}
  - ${observation2}
  - ${observation3}

  Is this accurate? Or is the actual state different?

  (Note: A codebase can have poor practices despite being in production,
  or good practices despite being a prototype. Tell me the reality.)
  """
}

AcceptCorrections {
  "It's actually in production but we have tech debt"
  "It's a prototype but I'm being careful because it handles payments"
  "That's right"
}

## Step 4: Ask Target

askTarget() {
  """
  What lifecycle stage are you targeting?

  1. Stay at current (${current}) - optimize for where you are
  2. Proof of Concept - validate fast, likely throwaway
  3. Throwaway Tool - solve immediate problem
  4. Team Tool - used by a small known group
  5. MVP - first version for real users
  6. Early Production - real users, growing
  7. Mature Production - stable, reliable, many depend on it
  8. Legacy - maintain but not actively developing

  Or describe your goal (e.g., "transition from MVP to early production")
  """
}

## Step 5: Suggest Defaults

suggestDefaults() {
  Based on target lifecycle, infer reasonable defaults for:
    risk, team, velocity, scale

  """
  Based on "${targetLifecycle}", I suggest:

  - Risk: ${risk} - ${reasoning}
  - Team: ${team} - ${reasoning}
  - Velocity: ${velocity} - ${reasoning}
  - Scale: ${scale} - ${reasoning}

  Accept these? Or tell me what's different about your situation.
  """
}

LifecycleDefaults {
  PoC | Throwaway => simpler options
  Production | Mature => more rigorous options
}

## Custom Descriptions

(user provides free-form text) => interpretCustomDescription()

interpretCustomDescription() {
  Look for:
    Combinations that override defaults
      "prototype but handles payments" => PoC + Financial risk
    Transitions or future state
      "might open source later" => note in rules
    Context affecting multiple dimensions
      "internal but company-wide" => Internal risk, Organization scale

  Synthesize into coherent rules, not closest canonical option
}

## Output Generation

generateOutput() {
  generateCoreMdc |> generateCommands |> generateHelp |> reportCompletion
}

### Core Rules

generateCoreMdc() {
  Write [output]/rules/core.mdc using SudoLang with:
    1. Project context (current, target, transition goals)
    2. Concrete behaviors (error handling, testing, security, docs, complexity)
    3. Constraints (must/must not)
    4. If transitioning: differentiate existing code (lenient) vs new code (target-grade)
}

### Commands

AlwaysInclude {
  /help => "List available commands"
  /plan => "Review current plan, suggest next steps"
  /task => "Break down and plan a task or feature"
  /review => "Review code changes"
  /commit => "Draft commit message"
  /explain => "Explain code, architecture, or decisions"
}

IncludeByContext {
  TeamTool+ => /discover, /story, /execute, /log, /status
  MVP+ => /feature, /journey, /architect
  BusinessRisk+ => /security-review
  NotPrototype => /refactor
  Always => /debug
  LargeTeam | OpenSource => /onboard
  TeamReview => /pr
  EarlyProduction+ => /changelog
}

generateCommands() {
  For each command to include:
    1. Reference core.mdc
    2. Use SudoLang patterns from @aigen/rules/guidance/sudolang-style.mdc
    3. Adapt depth to lifecycle/risk
    4. Include appropriate constraints
}

CommandCalibration {
  "PoC /review is lighter than Mature Production /review"
  "Solo /commit is simpler than Team /commit"
  "High-risk /security-review is more thorough"
}

## Completion

reportCompletion() {
  """
  Created AI command system in ${outputDir}/

  Generated:
    rules/core.mdc       - Core AI behaviors
    commands/
      help.md            - /help
      plan.md            - /plan
      task.md            - /task
      ${additionalCommands}

  Your context:
    Current lifecycle: ${currentLifecycle}
    Target lifecycle:  ${targetLifecycle}
    Risk: ${risk}
    Team: ${team}
    Velocity: ${velocity}
    Scale: ${scale}

  ${transitionNote}

  Ready to use! Run /aigen-stack to add technology-specific rules.

  To update later: Run /aigen-init again to reassess.
  """
}

transitionNote() {
  (isTransition) => """
  Transition: ${current} → ${target}
  AI will help you build ${target}-grade practices incrementally.
  """
}

Constraints {
  All generated files MUST use SudoLang patterns.
  Synthesize choices into coherent behavior, not independent settings.
  Ask one key question, infer the rest.
  Accept free-form descriptions, not just menu selections.
  Calibrate command depth to lifecycle and risk.
}
