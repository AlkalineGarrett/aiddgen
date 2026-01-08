---
description: Generate technology-specific rules based on your stack
---

# /aigen-stack

Generate technology-specific rules calibrated to lifecycle context.

## References

@aigen/rules/generator.mdc
@aigen/rules/guidance/sudolang-style.mdc
@aigen/rules/guidance/programming-principles.mdc
@aigen/rules/guidance/testing-methodology.mdc
@aigen/rules/guidance/security-patterns.mdc

## Prerequisites

Run /aigen-init first to establish strategic context.

## Process

stackProcess() {
  detectCodebase |> (newRepo => flowA) | (existingRepo => flowB) |> generateStackRules
}

detectCodebase() {
  (no source code) => newRepo
  (source code exists) => existingRepo
}

## Flow A: New Repo

flowA() {
  confirmProjectType |> askEcosystem |> recommendStack |> letUserAdjust
}

confirmProjectType() {
  """
  From your init setup:
  - Building: ${projectType}
  - Target lifecycle: ${targetLifecycle}
  - Risk level: ${riskLevel}

  Let's choose your tech stack.
  """
}

askEcosystem() {
  """
  What language or ecosystem do you want to work in?

  1. JavaScript/TypeScript (Node, React, etc.)
  2. Python
  3. Go
  4. Rust
  5. Ruby
  6. Java/Kotlin
  7. C# / .NET
  8. Other: [specify]
  9. No preference - recommend based on my project type
  """
}

recommendStack() {
  Based on projectType + lifecycle + ecosystem, recommend complete stack

  RecommendationCriteria {
    PoC => minimize setup, batteries-included, simple storage
    MVP => balance speed with foundation, easy deployment
    Production => reliability, observability, team scalability
    ProjectType => appropriate frameworks
    Ecosystem => stay within preference
    TeamFamiliarity => weight toward known tech
    RiskLevel => higher risk → proven choices
  }
}

## Flow B: Existing Codebase

flowB() {
  analyzeStack |> presentFindings |> verifyWithUser
}

analyzeStack() {
  Examine for:
    PackageManifests => package.json, requirements.txt, go.mod, Cargo.toml
    FrameworkConfig => next.config, vite.config, etc.
    LanguageConfig => tsconfig, pyproject.toml
    TestFramework => vitest.config, pytest.ini
    CICD => .github/workflows, gitlab-ci
    Deployment => vercel.json, Dockerfile, terraform
    Database => prisma/schema, alembic
    CodePatterns => imports, directory structure
}

presentFindings() {
  """
  Detected stack:

  Language/Runtime: ${languages}
  Framework(s): ${frameworks}
  Database/Storage: ${database}
  Testing: ${testFramework}
  Deployment: ${deployment}
  Other: ${otherLibraries}

  Confirm, add, remove, or correct?
  """
}

AcceptCorrections {
  "We're also using X for Y"
  "Remove X, we actually use Y"
  "We're migrating from X to Y"
}

## Lifecycle Calibration

LifecycleCalibration {
  | Aspect | PoC | MVP | EarlyProd | Mature | Legacy |
  | TypeSafety | Loose | Prefer | Strict new | Strict all | Preserve |
  | Patterns | Whatever | Basic | Consistent | Strict | Preserve |
  | Testing | Skip | Critical | New tested | Comprehensive | Before change |
  | DataSchema | Free | Basic | Safe | Reviewed | Extreme care |
  | ErrorHandling | Minimal | UserPaths | Comprehensive | Monitored | Preserve |
  | Dependencies | Whatever | Reasonable | Evaluate | Proven | Minimize |
}

## Technology Categories

TechCategories {
  Frontend => Framework, StateManagement, Styling, BuildTool
  Backend => Runtime, Framework, APIStyle
  Data => Database, ORM, Caching
  Infrastructure => Deployment, CICD
  Quality => Testing, Linting
}

(only generate rules for confirmed technologies)

## Incorporating Guidance

incorporateGuidance() {
  For each technology rule, include from guidance files:

  ProgrammingPrinciples {
    DOT, YAGNI, KISS, DRY, SDA => adapted to language idioms
    StylePreferences => where language supports
    NamingConventions => adapted to language
  }

  TestingMethodology {
    FiveQuestions => adapted to framework syntax
    RITEWay => Readable, Isolated, Thorough, Explicit
    AssertionPatterns => framework-specific
  }

  SecurityPatterns {
    SecretComparison => language-specific hash pattern
    InputValidation => framework patterns
    OutputEncoding => framework escaping
    OWASPAwareness => calibrated to risk
  }
}

RiskCalibration {
  Personal | Internal => basic patterns, flag obvious
  Business => full patterns, require validation
  Financial | Healthcare => paranoid, audit, compliance
}

## Output Generation

generateStackRules() {
  For each confirmed technology:
    generateRule(technology)
}

generateRule(tech) {
  Write [output]/rules/stack/${tech}.mdc using SudoLang:

  """
  ---
  description: ${tech} guidelines for ${lifecycle}
  globs: "${filePatterns}"
  ---

  # ${TechName}

  ## Context
  Current: ${current} | Target: ${target}
  ${contextDescription}

  ## Patterns
  ${lifecyclePatterns}

  ## New Code
  ${targetGradeRules}

  ## Existing Code
  ${lenientRules} (if transitioning)

  Constraints {
    ${hardRules}
  }
  """
}

## Completion

reportCompletion() {
  """
  Generated stack rules calibrated to your lifecycle:

    Current: ${current} | Target: ${target}
    Risk: ${riskLevel}

    ${generatedRulesList}

  Stack rules reference core.mdc and guide AI behavior
  appropriately for your project's stage.

  To update after lifecycle transition:
    Run /aigen-stack again after updating /aigen-init
  """
}

Constraints {
  All generated files MUST use SudoLang patterns.
  Only generate rules for confirmed technologies.
  Calibrate strictness to lifecycle stage.
  Include security patterns based on risk level.
  Reference guidance files for consistent patterns.
}
