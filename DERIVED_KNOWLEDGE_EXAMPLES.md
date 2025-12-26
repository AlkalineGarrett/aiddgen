# Derived Knowledge Bits Examples

This document provides examples of good and bad derived knowledge bits to guide the creation of meaningful generalizations from specific knowledge bits.

## What are Derived Knowledge Bits?

Derived knowledge bits are generalizations extracted from multiple specific knowledge bits in a leaf node. They capture the essential essence of related knowledge bits while being more general than the originals. They must include a `derivedFrom` field containing an array of the knowledge bit IDs that contributed to the generalization.

## Requirements

When creating derived knowledge bits, the following requirements must be met:

- **Usefulness Requirement**: Derived knowledge bits must provide meaningful information. Generic statements that could apply to any rule-based system are not useful.

- **Concrete Details Requirement**: Derived knowledge bits must include concrete, specific details about what makes them unique to that node, while being more general than the original bits. A cue that a derived knowledge bit is too vague is when it wording like "contains specific patterns"; the concrete instances would need to be called out to be sufficiently detailed.

- **Meaningful Reduction Requirement**: Derived knowledge bits must represent a meaningful reduction, not just a more concise version of the key pieces. If a derived bit is essentially just restating the individual knowledge bits in a shorter form, it doesn't merit being a derived knowledge bit. A good derived knowledge bit captures the essential essence while omitting secondary characteristics and implied details.

- **Uniqueness Requirement**: If derived information is exactly the same between two different nodes, it is too general and doesn't provide useful information. Each derived knowledge bit should capture something unique to its node's context. If a pattern appears identical across multiple nodes, it should either be removed or made more specific to each node's unique characteristics.

- **Context-Free Requirement**: Derived knowledge bits must also follow the Context-Free Knowledge Bits principle - they must be self-contained and not reference other knowledge bits.

## Examples by Node

### Node: AIDD commit command: constraints

**Original Knowledge Bits:**
- `knowledge-6.1`: "Don't log about logging in the commit message"
- `knowledge-6.2`: "Use multiple -m flags, one for each log entry"
- `knowledge-6.3`: "Limit the first commit message line length to 50 characters"
- `knowledge-6.4`: "Use conventional commits with the supplied template"
- `knowledge-6.5`: "Do NOT add new things to the CHANGELOG.md file"

#### Good Derived Knowledge Bit

```json
{
  "id": "knowledge-6.6",
  "text": "Commit constraints contain best practices related to length, salience, and structure",
  "nextReviewDate": null,
  "derivedFrom": ["knowledge-6.1", "knowledge-6.2", "knowledge-6.3", "knowledge-6.4"]
}
```

**Why this is good:**
- ✅ Concrete: specifies "length" (from knowledge-6.3), "salience" (from knowledge-6.1 about avoiding logging references), and "structure" (from knowledge-6.2 and knowledge-6.4 about format and template)
- ✅ More general than originals but still specific enough to be useful
- ✅ Captures the essence of the formatting and content constraints while omitting knowledge-6.5 (CHANGELOG restriction) which is a separate concern

#### Bad Example

```
"Multiple constraints and restrictions must be followed"
```

**Why this is bad:**
- ❌ Too generic - could apply to any rule-based system, thus provides no useful information
- ❌ Doesn't specify the nature of the constraints or restrictions

### Node: AIDD discover command: constraints

**Original Knowledge Bits:**
- `knowledge-9.1`: "Begin by reading the file and asking the user relevant questions to spark the discovery process"
- `knowledge-9.2`: "Before beginning, read and respect the constraints in please.mdc"

#### Bad Derived Knowledge Bit

```
"Command execution requires: asking user questions, reading constraints from please.mdc"
```

**Why this is bad:**
- ❌ Just a more concise version of the key pieces
- ❌ Doesn't represent a meaningful reduction
- ❌ Essentially restates the individual knowledge bits in shorter form

**What to do instead:**
Don't create a derived knowledge bit because there's not very much information to start with, so a derived bit isn't helpful.

### Node: AIDD agent orchestrator rule: task prompt

**Original Knowledge Bits:**
- `knowledge-33.1`: "taskPrompt format: # Guides section listing guide file refs in markdown format, followed by # User Prompt section with the user's prompt"
- `knowledge-33.2`: "taskPrompt structure: Read each of the following guides for important context, and follow their instructions carefully: ${list guide file refs in markdown format}, then include # User Prompt with ${prompt}"

#### Bad Derived Knowledge Bit

```
"Task prompts structure guides and user prompts in markdown format with specific section headers"
```

**Why this is bad:**
- ❌ Uses vague "specific" language without concrete instances
- ❌ Doesn't tell you what the section headers actually are

**What to do instead:**
Don't create a derived knowledge bit because there's no meaningful generalization to extract.

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

#### Good Derived Knowledge Bit

```json
{
  "id": "knowledge-32.13",
  "text": "Agent descriptions contain a context for application and a goal it helps achieve",
  "nextReviewDate": null,
  "derivedFrom": ["knowledge-32.1", "knowledge-32.2", "knowledge-32.3"]
}
```

**Why this is good:**
- ✅ Captures a high-level pattern that is consistent among all of the knowledge bits
- ✅ Represents a meaningful reduction that describes the structure of agent descriptions
- ✅ Omits the specific agent names and details, focusing on the pattern

#### Bad Example

```
"Available agents include: please (general assistance), stack (tech stack guidance), productmanager (product discovery), tdd (test-driven development), javascript (JavaScript best practices), log (change logs), commit (conventional commits), autodux (Redux state management), javascript-io-network-effects (saga patterns), ui (UI/UX design), and requirements (functional requirements)"
```

**Why this is bad:**
- ❌ Enumerates specific agents instead of capturing a general pattern
- ❌ Violates the Hierarchy Principle: individual agents should be child nodes, not listed in a knowledge bit
- ❌ Too detailed and specific - doesn't provide a meaningful generalization
- ❌ Essentially just restates all the individual knowledge bits in a list format

### Node: AIDD log rule: constraints

**Original Knowledge Bits:**
- `knowledge-64.1`: "Always use reverse chronological order"
- `knowledge-64.2`: "Keep descriptions brief (< 50 chars)"
- `knowledge-64.3`: "Focus on epic-level accomplishments, not implementation details"
- `knowledge-64.4`: "Never log meta-work or trivial changes"
- `knowledge-64.5`: "Omit the 'epic' from the description"

#### Good Derived Knowledge Bit

```json
{
  "id": "knowledge-64.6",
  "text": "Change logs for epics should be concise and describe high-level accomplishments",
  "nextReviewDate": null,
  "derivedFrom": ["knowledge-64.2", "knowledge-64.3", "knowledge-64.4", "knowledge-64.5"]
}
```

**Why this is good:**
- ✅ Captures the essential essence: "concise" from knowledge-64.2 and knowledge-64.5 (implied connection), "high-level accomplishments" from knowledge-64.3 and knowledge-64.4 (implied connection)
- ✅ Omits secondary characteristics: reverse-chronological ordering (knowledge-64.1) is a secondary detail
- ✅ Omits implied details: omitting 'epic' from the description (knowledge-64.5) is a detailed aspect of conciseness; not logging meta-work (knowledge-64.4) is implied by focusing on high-level accomplishments
- ✅ Represents a meaningful reduction, not just a concise summary

## Complete Example: JSON Structure

```json
{
  "id": "node-64",
  "name": "AIDD log rule: constraints",
  "knowledgeBits": [
    {
      "id": "knowledge-64.1",
      "text": "Always use reverse chronological order",
      "nextReviewDate": null
    },
    {
      "id": "knowledge-64.2",
      "text": "Keep descriptions brief (< 50 chars)",
      "nextReviewDate": null
    },
    {
      "id": "knowledge-64.3",
      "text": "Focus on epic-level accomplishments, not implementation details",
      "nextReviewDate": null
    },
    {
      "id": "knowledge-64.4",
      "text": "Never log meta-work or trivial changes",
      "nextReviewDate": null
    },
    {
      "id": "knowledge-64.5",
      "text": "Omit the 'epic' from the description",
      "nextReviewDate": null
    },
    {
      "id": "knowledge-64.6",
      "text": "Change logs for epics should be concise and describe high-level accomplishments",
      "nextReviewDate": null,
      "derivedFrom": ["knowledge-64.2", "knowledge-64.3", "knowledge-64.4", "knowledge-64.5"]
    }
  ],
  "fileSegments": [
    {
      "filename": "ai/rules/log.mdc",
      "lineRange": {
        "start": 47,
        "end": 54
      }
    }
  ]
}
```

### Node: AIDD stack rule: Redux

**Original Knowledge Bits:**
- `knowledge-114.2`: "Build the Autodux dux object and save it as ${slice name}-dux.sudo, then transpile to JavaScript and save it as ${slice name}-dux.js"

#### Bad Derived Knowledge Bit

```json
{
  "id": "knowledge-114.5",
  "text": "Autodux dux objects are saved as .sudo files and transpiled to .js files",
  "nextReviewDate": null,
  "derivedFrom": ["knowledge-114.2"]
}
```

**Why this is bad:**
- ❌ Doesn't represent a meaningful reduction - essentially just removes the specific file naming pattern (${slice name}-dux) and the "Build" instruction

**What to do instead:**
Don't create a derived knowledge bit.

## Key Principles Summary

For higher-level details, see the [Knowledge Progress Schema](./KNOWLEDGE_PROGRESS_SCHEMA.md).
