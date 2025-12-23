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
      "name": "AIDD commands",
      "knowledgeBits": [
        {
          "id": "knowledge-2.1",
          "text": "Commands are user-invokable actions that trigger specific workflows in AIDD",
          "nextReviewDate": null
        }
      ],
      "fileSegments": [],
      "children": [
        {
          "id": "node-3",
          "name": "AIDD commit command",
          "knowledgeBits": [
            {
              "id": "knowledge-3.1",
              "text": "Commits changes to the repository using conventional commit format",
              "nextReviewDate": null
            }
          ],
          "fileSegments": [
            {
              "filename": "ai/commands/commit.md",
              "lineRange": null
            }
          ],
          "children": [
            {
              "id": "node-4",
              "name": "AIDD commit command: purpose",
              "knowledgeBits": [
                {
                  "id": "knowledge-4.1",
                  "text": "Commits changes to the repository in non-interactive modes only",
                  "nextReviewDate": null
                }
              ],
              "fileSegments": [
                {
                  "filename": "ai/commands/commit.md",
                  "lineRange": {
                    "start": 1,
                    "end": 3
                  }
                }
              ]
            }
          ]
        }
      ]
    }
  ]
}
```

## Principles

- **Generality Principle**: Each node should capture content that applies to its level of generality. Higher-level nodes contain more general content, while lower-level nodes contain more specific content.
- **Content Alignment**: File segments in a node should relate to concepts at that node's level of abstraction, not concepts that are more specific (which belong in child nodes) or more general (which belong in parent nodes).
- **Knowledge Bits**: Each node should have knowledge bits that represent testable knowledge at that node's level of generality. Knowledge bits should be concise statements that can be used to test understanding.
- **Context-Free Knowledge Bits**: Each knowledge bit must be context-free and self-contained. Knowledge bits cannot refer to other knowledge bits that occur before them. Avoid using pronouns (e.g., "this", "that", "it", "they") or connecting words (e.g., "also", "additionally", "furthermore") at the beginning of a knowledge bit when the referent is in a previous knowledge bit. If a knowledge bit references a concept from another knowledge bit, they should be combined into a single knowledge bit. Examples: "This is very important..." should be combined with the previous bit it references; "Also a top tier motion designer..." should be rewritten as "Act as a top tier motion designer..." to be self-contained.
- **Hierarchy Principle**: If knowledge bits describe individual instances or specific items (e.g., individual commands, individual rules), those should be child nodes instead. Knowledge bits should capture general principles, concepts, or patterns that apply to the category as a whole, not enumerate specific items within that category.
- **Context-Independent Naming**: All node names must be context-independent and fully qualified. Names should be unambiguous even when viewed outside the tree structure. Examples: "AIDD commands" (not "commands"), "AIDD commit command" (not "commit"), "AIDD commit command: purpose" (not "purpose"), "AIDD autodux rule" (not "autodux").

## ID Assignment Rules

- **Node IDs**: Each node receives a unique identifier in the format `"node-#"` where `#` is a sequential integer starting at 1, with the root node receiving `"node-1"`.

- **Knowledge Bit IDs**: Each knowledge bit receives a unique identifier in the format `"knowledge-#.#"` where:
  - The first number corresponds to the ID number of the containing node (e.g., knowledge bits in `"node-3"` start with `"knowledge-3."`)
  - The second number is a sequential index starting at 1 for each knowledge bit within that node (e.g., `"knowledge-3.1"`, `"knowledge-3.2"`, etc.)

- IDs must be unique across the entire knowledge tree and should remain stable once assigned to preserve references. When adding new nodes or knowledge bits, existing IDs must be retained; new IDs should be assigned only to newly added elements.

## Notes

- The next review date for each knowledge bit should be updated after each knowledge review session.
- This schema is iterative and will be refined based on usage patterns.

