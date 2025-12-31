# Knowledge Progress Schema

This document describes the structure of the `knowledge_progress.json` file, which tracks knowledge acquisition progress through a hierarchical knowledge tree.

## Structure

The knowledge tree is compositional, with a single root node named "AIDD". Each node in the tree represents a knowledge domain or topic.

### Node Structure

Each node in the knowledge tree contains:

- **id**: A unique identifier for the node. See ID Assignment Rules section for format details.

- **name**: The name of the knowledge domain or topic. See the Context-Independent Naming principle for naming requirements.

- **knowledgeBits**: An array of knowledge bit objects that capture key concepts, facts, or principles at this node's level of generality. Each knowledge bit object contains:
  - **id**: A unique identifier for the knowledge bit. See ID Assignment Rules section for format details.
  - **text**: A string representing the testable knowledge that the user should understand about this domain.
  - **nextReviewDate**: An ISO 8601 date string (YYYY-MM-DD) or `null` indicating when the user should be asked about this specific knowledge bit again to test their knowledge retention. `null` means no review is scheduled.
  - **derivedFrom**: (Optional) An array of knowledge bit IDs that this knowledge bit was derived from. This field is used when a general principle or cross-cutting concept is identified across multiple specific knowledge bits. The derived knowledge bit captures the general pattern, while the `derivedFrom` field tracks which specific knowledge bits contributed to this generalization. See [Derived Knowledge Bits Examples](./DERIVED_KNOWLEDGE_EXAMPLES.md) for detailed examples of good and bad derived knowledge bits.
  - **specialType**: (Optional) A string indicating a special type of knowledge bit. Currently supported value is `"derived"`, which must be present when the `derivedFrom` field is present. This field helps identify knowledge bits that were derived from other knowledge bits.

- **fileSegments**: An array of file segments that apply to this node. Each file segment contains:
  - **filename**: The path to the file relative to the project root
  - **lineRange**: (Optional) An object with:
    - **start**: The starting line number (1-indexed)
    - **end**: The ending line number (inclusive)
    - If `lineRange` is `null` or omitted, the entire file is included.

- **children**: (Optional) An array of child nodes, each following the same structure recursively.

## Example

```json
{
  "id": "node-1",
  "name": "AIDD",
  "knowledgeBits": [
    {
      "id": "knowledge-1.1",
      "text": "AIDD is an AI agent system that assists with software development projects",
      "nextReviewDate": null
    }
  ],
  "fileSegments": [
    {
      "filename": "ai/rules/agent-orchestrator.mdc",
      "lineRange": null
    }
  ],
  "children": [
    {
      "id": "node-2",
      "name": "AIDD log rule",
      "knowledgeBits": [
        {
          "id": "knowledge-2.1",
          "text": "Guide for logging changes to activity-log.md",
          "nextReviewDate": null
        }
      ],
      "fileSegments": [
        {
          "filename": "ai/rules/log.mdc",
          "lineRange": null
        }
      ],
      "children": [
        {
          "id": "node-3",
          "name": "AIDD log rule: constraints",
          "knowledgeBits": [
            {
              "id": "knowledge-3.1",
              "text": "Always use reverse chronological order",
              "nextReviewDate": null
            },
            {
              "id": "knowledge-3.2",
              "text": "Keep descriptions brief (< 50 chars)",
              "nextReviewDate": null
            },
            {
              "id": "knowledge-3.3",
              "text": "Focus on epic-level accomplishments, not implementation details",
              "nextReviewDate": null
            },
            {
              "id": "knowledge-3.4",
              "text": "Never log meta-work or trivial changes",
              "nextReviewDate": null
            },
            {
              "id": "knowledge-3.5",
              "text": "Omit the 'epic' from the description",
              "nextReviewDate": null
            },
            {
              "id": "knowledge-3.6",
              "text": "Change logs for epics should be concise and describe high-level accomplishments",
              "nextReviewDate": null,
              "derivedFrom": ["knowledge-3.2", "knowledge-3.3"],
              "specialType": "derived"
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
      ]
    }
  ]
}
```

**Note:** The derived knowledge bit (`knowledge-3.6`) in the example above demonstrates a meaningful reduction. See [Derived Knowledge Bits Examples](./DERIVED_KNOWLEDGE_EXAMPLES.md) for detailed explanation of why this is a good example.

## Principles

- **Generality Principle**: Each node should capture content that applies to its level of generality. Higher-level nodes contain more general content, while lower-level nodes contain more specific content.
- **Content Alignment**: File segments in a node should relate to concepts at that node's level of abstraction, not concepts that are more specific (which belong in child nodes) or more general (which belong in parent nodes).
- **Knowledge Bits**: Each node should have knowledge bits that represent testable knowledge at that node's level of generality. Knowledge bits should be concise statements that can be used to test understanding.
- **Context-Free Knowledge Bits**: Each knowledge bit must be context-free and self-contained. Knowledge bits cannot refer to other knowledge bits that occur before them.
  
  **What to avoid:**
  - Pronouns (e.g., "this", "that", "it", "they") at the beginning when the referent is in a previous knowledge bit
  - Connecting words (e.g., "also", "additionally", "furthermore") at the beginning when the referent is in a previous knowledge bit
  - Passive voice constructions that omit the subject (e.g., "Used to make network requests..." should be "call function is used to make network requests...")
  - Nouns without articles when they're not proper nouns (e.g., "Saga never calls..." should be "A saga never calls...")
  - Definite articles ("the") that reference context without sufficient specification (e.g., "the effect function" should be "the effect function returned from a call function invocation" to make it context-free)
  
  **What to do:**
  - If a knowledge bit references a concept from another knowledge bit, combine them into a single knowledge bit
  - Explicitly state the subject of the sentence (e.g., "call function is used to..." not "Used to...")
  
  **Examples:**
  - ❌ "This is very important..." → Should be combined with the previous bit it references
  - ❌ "Also a top tier motion designer..." → ✅ "Act as a top tier motion designer..." (self-contained)
  - ❌ "Used to make network requests..." → ✅ "call function is used to make network requests..." (explicitly states what is being used)
  - ❌ "Saga itself never calls the effect function, instead it yields the effect object" → ✅ "A saga never calls the effect function returned from a call function invocation, instead a saga yields the effect object" (uses indefinite article for general concept, specifies what "the effect function" refers to, avoids pronoun "it")
- **Hierarchy Principle**: If knowledge bits describe individual instances or specific items (e.g., individual commands, individual rules), those should be child nodes instead. Knowledge bits should capture general principles, concepts, or patterns that apply to the category as a whole, not enumerate specific items within that category.
- **Context-Independent Naming**: All node names must be context-independent and fully qualified. Names should be unambiguous even when viewed outside the tree structure. Examples: "AIDD commands" (not "commands"), "AIDD commit command" (not "commit"), "AIDD commit command: purpose" (not "purpose"), "AIDD autodux rule" (not "autodux").
- **Derived Knowledge Bits**: When analyzing leaf nodes (nodes with no children), general ideas, repeated concepts, or cross-cutting principles that appear across multiple knowledge bits should be extracted and added as new knowledge bits. These derived knowledge bits must be useful and contain concrete details specific to the node, while being more general than the individual knowledge bits they're derived from. They should include a `derivedFrom` field containing an array of the knowledge bit IDs that contributed to the generalization. Derived knowledge bits must represent a meaningful reduction, not just a restatement of a single knowledge bit with slightly different wording. See [Derived Knowledge Bits Examples](./DERIVED_KNOWLEDGE_EXAMPLES.md) for requirements, comprehensive examples, and detailed explanations.

## ID Assignment Rules

- **Node IDs**: Each node receives a unique identifier in the format `"node-#"` where `#` is a sequential integer starting at 1, with the root node receiving `"node-1"`.

- **Knowledge Bit IDs**: Each knowledge bit receives a unique identifier in the format `"knowledge-#.#"` where:
  - The first number corresponds to the ID number of the containing node (e.g., knowledge bits in `"node-3"` start with `"knowledge-3."`)
  - The second number is a sequential index starting at 1 for each knowledge bit within that node (e.g., `"knowledge-3.1"`, `"knowledge-3.2"`, etc.)

- IDs must be unique across the entire knowledge tree and should remain stable once assigned to preserve references. When adding new nodes or knowledge bits, existing IDs must be retained; new IDs should be assigned only to newly added elements.

## Notes

- The next review date for each knowledge bit should be updated after each knowledge review session.
- This schema is iterative and will be refined based on usage patterns.

