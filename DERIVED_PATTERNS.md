# Pattern Knowledge Bits Examples

This document provides examples of good and bad pattern knowledge bits to guide the identification and creation of patterns observed across multiple knowledge bits.

## What are Pattern Knowledge Bits?

Pattern knowledge bits capture structural patterns, recurring relationships, or common forms that are observed across multiple knowledge bits. Unlike derived knowledge bits which generalize the *content* of related knowledge bits, pattern knowledge bits identify the *structure* or *format* that multiple knowledge bits share. They must include a `derivedFrom` field containing an array of the knowledge bit IDs that exhibit the pattern, and have `specialType: "pattern"`.

Pattern knowledge bits answer questions like:
- What structure or format do these knowledge bits share?
- What common pattern exists in how these items are described?
- What recurring relationship or form can be observed across these knowledge bits?

## Requirements

When creating pattern knowledge bits, the following requirements must be met:

- **Pattern Identification Requirement**: Pattern knowledge bits must identify a structural pattern, format, or recurring relationship that is visible across the source knowledge bits. The pattern should be about *how* the knowledge is structured, not *what* the knowledge says.

- **Concrete Pattern Details Requirement**: Pattern knowledge bits must describe the pattern with concrete details. Vague descriptions like "contains patterns" are insufficient - the specific structural elements must be called out.

- **Structural Focus Requirement**: Pattern knowledge bits focus on structure, format, or form rather than content generalization. If you're generalizing what the knowledge bits say, that's a derived knowledge bit, not a pattern knowledge bit.

- **Meaningful Pattern Requirement**: The identified pattern must be meaningful and useful. Trivial patterns that could apply to any knowledge bits are not valuable.

- **Context-Free Requirement**: Pattern knowledge bits must follow the Context-Free Knowledge Bits principle - they must be self-contained and not reference other knowledge bits.

## Examples

### Node: AIDD agent orchestrator rule: agents

**Original Knowledge Bits:**
- `knowledge-32.1`: "please: when user says please, use for general assistance, logging, committing, and proofing tasks"
- `knowledge-32.2`: "stack: when implementing NextJS + React/Redux + Shadcn UI features, use for tech stack guidance and best practices"
- `knowledge-32.3`: "productmanager: when planning features, user stories, user journeys, or conducting product discovery, use for building specifications and user journey maps"
- `knowledge-32.4`: "tdd: when implementing code changes, use for systematic test-driven development with proper test isolation"
- `knowledge-32.5`: "javascript: when writing JavaScript or TypeScript code, use for JavaScript best practices and guidance"
- `knowledge-32.6`: "log: when documenting changes, use for creating structured change logs with emoji categorization"
- `knowledge-32.7`: "commit: when committing code, use for conventional commit format with proper message structure"
- `knowledge-32.8`: "autodux: when building Redux state management, use for creating and transpiling Autodux dux objects"
- `knowledge-32.9`: "javascript-io-network-effects: when making network requests or invoking side-effects, use for saga pattern implementation"
- `knowledge-32.10`: "ui: when building user interfaces and user experiences, use for beautiful and friendly UI/UX design"
- `knowledge-32.11`: "requirements: when writing functional requirements for a user story, use for functional requirement specification"

#### Good Pattern Knowledge Bit

```json
{
  "id": "knowledge-32.13",
  "text": "Agent descriptions contain a context for application and a goal it helps achieve",
  "nextReviewDate": null,
  "derivedFrom": ["knowledge-32.1", "knowledge-32.2", "knowledge-32.3"],
  "specialType": "pattern"
}
```

**Why this is good:**
- ✅ Identifies a structural pattern: each agent description has two components (context + goal)
- ✅ Describes the format/structure that is consistent across the knowledge bits
- ✅ Focuses on *how* the descriptions are structured, not *what* they describe
- ✅ Provides concrete details about the pattern: "context for application" and "goal it helps achieve"
- ✅ Represents a meaningful pattern that is useful for understanding the structure of agent descriptions

#### Bad Example 1: Content Generalization Instead of Pattern

```json
{
  "id": "knowledge-32.13",
  "text": "Agents provide guidance for different software development tasks",
  "nextReviewDate": null,
  "derivedFrom": ["knowledge-32.1", "knowledge-32.2", "knowledge-32.3"],
  "specialType": "pattern"
}
```

**Why this is bad:**
- ❌ Generalizes the *content* (what agents do) rather than identifying a structural *pattern* (how they're described)
- ❌ This would be better as a "summary" specialType, not "pattern"
- ❌ Doesn't describe the structure or format of the descriptions

#### Bad Example 2: Enumeration Instead of Pattern

```
"Available agents include: please (general assistance), stack (tech stack guidance), productmanager (product discovery), tdd (test-driven development), javascript (JavaScript best practices), log (change logs), commit (conventional commits), autodux (Redux state management), javascript-io-network-effects (saga patterns), ui (UI/UX design), and requirements (functional requirements)"
```

**Why this is bad:**
- ❌ Enumerates specific agents instead of capturing a general pattern
- ❌ Violates the Hierarchy Principle: individual agents should be child nodes, not listed in a knowledge bit
- ❌ Too detailed and specific - doesn't provide a meaningful pattern
- ❌ Essentially just restates all the individual knowledge bits in a list format

## Complete Example: JSON Structure

```json
{
  "id": "node-32",
  "name": "AIDD agent orchestrator rule: agents",
  "knowledgeBits": [
    ... (input knowledge bits) ...
    {
      "id": "knowledge-32.13",
      "text": "Agent descriptions contain a context for application and a goal it helps achieve",
      "nextReviewDate": null,
      "derivedFrom": ["knowledge-32.1", "knowledge-32.2", "knowledge-32.3"],
      "specialType": "pattern"
    }
  ],
  "fileSegments": [
    {
      "filename": "ai/rules/agent-orchestrator.mdc",
      "lineRange": {
        "start": 13,
        "end": 27
      }
    }
  ]
}
```

## Key Principles Summary

For higher-level details, see the [Knowledge Progress Schema](./KNOWLEDGE_PROGRESS_SCHEMA.md).

