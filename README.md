# Markdown AI Claim Graph

![Markdown AI Claim Graph banner](assets/banner.svg)

A graph-native skill for building typed claim graphs from Markdown analyst files.

Markdown AI Claim Graph does one thing: it constructs a claim graph and renders that graph as tables, Mermaid, JSON, and a graph-derived decision.

## Installation

Install this skill into your Codex skills directory as `markdown-ai-claim-graph`.

### Option 1: Clone into `~/.codex/skills`

```bash
git clone https://github.com/puwarun/markdown-ai-claim-graph.git ~/.codex/skills/markdown-ai-claim-graph
```

### Option 2: Copy this folder into an existing skills directory

Place this repository at:

```text
~/.codex/skills/markdown-ai-claim-graph
```

The final structure should look like:

```text
~/.codex/skills/markdown-ai-claim-graph/
├── SKILL.md
└── agents/openai.yaml
```

### Verify

```text
Use $markdown-ai-claim-graph to build a claim graph from these Markdown analyst files and output node and edge tables, Mermaid, JSON, and the final decision.
```

## Primary Output

The standard output order is:

1. Node Table
2. Edge Table
3. Mermaid Graph
4. JSON Graph
5. Decision Summary

This project is organized around those outputs.

## How to Use

Markdown AI Claim Graph is used by giving an AI assistant two or more Markdown analysis files and asking it to build a graph-native reasoning map from them.

The graph is the primary output.

### Basic Prompt

```text
Use Markdown AI Claim Graph with the attached Markdown analysis files.

Context:
<Briefly describe what the files are about>

Goal:
Build a graph-first review from the analysis.

Output:
1. Node Table
2. Edge Table
3. Mermaid Graph
4. JSON Graph
5. Decision Summary

Rules:
- Extract claims, evidence, risks, recommendations, and decisions as nodes
- Connect nodes with explicit relationships
- Show where analysts agree or disagree
- Do not write a long narrative report first
- Keep the graph as the main output
```

### Example: Codex + Gemini Analysis

Input files:

- `codex_analyst.md`
- `gemini_analyst.md`

Prompt:

```text
Use Markdown AI Claim Graph to analyze codex_analyst.md and gemini_analyst.md.

Context:
Both files are performance analyses of an encryption library.

Goal:
Create a graph that shows:
- which claims are shared
- which recommendations conflict
- which risks qualify the recommendations
- what should be done now, later, conditionally, or avoided

Output:
1. Node Table
2. Edge Table
3. Mermaid Graph
4. JSON Graph
5. Decision Summary
```

### Example Output Structure

#### Node Table

| ID | Type | Source | Text | Confidence |
|---|---|---|---|---|
| A1 | Analyst | `codex_analyst.md` | Codex Analyst | High |
| A2 | Analyst | `gemini_analyst.md` | Gemini Analyst | High |
| C1 | Claim | Gemini | `isHexString()` loop is a hot path | Medium |
| E1 | Evidence | Codex | Benchmark shows regex is faster than manual loop | High |
| R1 | Risk | Codex | Runtime defaults cached by `baseDir` only may stale `process.env` | High |
| REC1 | Recommendation | Shared | Replace `isHexString()` loop with regex | High |
| D1 | Decision | System | Do Now | High |

#### Edge Table

| From | Relation | To |
|---|---|---|
| A2 | recommends | REC1 |
| E1 | supports | C1 |
| C1 | supports | REC1 |
| R1 | qualifies | REC2 |
| REC1 | leads_to | D1 |

#### Mermaid Graph

```mermaid
graph TD
  A2["Gemini Analyst"] -->|recommends| REC1["Replace isHexString loop with regex"]
  E1["Codex Evidence: regex benchmark faster"] -->|supports| C1["Claim: isHexString loop is hot path"]
  C1 -->|supports| REC1
  REC1 -->|leads_to| D1["Decision: Do Now"]

  R1["Risk: baseDir-only cache may stale process.env"] -->|qualifies| REC2["Improve runtime defaults cache"]
  REC2 -->|leads_to| D2["Decision: Do Later / Benchmark First"]
```

#### JSON Graph

```json
{
  "nodes": [
    {
      "id": "A1",
      "type": "Analyst",
      "source": "codex_analyst.md",
      "text": "Codex Analyst",
      "confidence": "high"
    },
    {
      "id": "C1",
      "type": "Claim",
      "source": "gemini_analyst.md",
      "text": "`isHexString()` loop is a hot path",
      "confidence": "medium"
    },
    {
      "id": "E1",
      "type": "Evidence",
      "source": "codex_analyst.md",
      "text": "Benchmark shows regex is faster than manual loop",
      "confidence": "high"
    },
    {
      "id": "REC1",
      "type": "Recommendation",
      "source": "shared",
      "text": "Replace `isHexString()` loop with regex",
      "priority": "do_now"
    }
  ],
  "edges": [
    {
      "from": "E1",
      "relation": "supports",
      "to": "C1"
    },
    {
      "from": "C1",
      "relation": "supports",
      "to": "REC1"
    }
  ]
}
```

#### Decision Summary

```md
## Decision Summary

### Do Now
- Replace `isHexString()` manual loop with regex
- Merge duplicated RSA option helper functions
- Extract shared encryption/decryption preparation logic

### Do Later
- Improve runtime defaults cache only after confirming the current path is still hot
- Optimize key path resolution only if profiler shows measurable impact

### Conditional
- Add AES-key caching only if real traffic reuses the same `secretCode`
- Use worker threads only if throughput or event-loop blocking becomes a production issue

### Avoid
- Do not cache runtime defaults by `baseDir` only
- Do not weaken RSA settings purely for speed without an explicit security decision
```

## Graph Model

### Node Types

- `Analyst`
- `File`
- `Topic`
- `Claim`
- `Evidence`
- `Risk`
- `Recommendation`
- `Decision`

### Edge Types

- `supports`
- `contradicts`
- `qualifies`
- `based_on`
- `recommends`
- `warns_about`
- `depends_on`
- `mitigates`
- `leads_to`
- `belongs_to`

## Workflow

1. Create source nodes from input Markdown files.
2. Extract topics, claims, evidence, risks, recommendations, and decisions as nodes.
3. Connect those nodes with typed edges.
4. Render the same graph as tables, Mermaid, and JSON.
5. Write the decision summary from the rendered graph.

## What The Graph Captures

- analyst provenance
- file provenance
- claim structure
- support and contradiction
- qualification and dependency
- risk and mitigation
- recommendation flow
- decision logic

## Example Prompt

```text
Use $markdown-ai-claim-graph to build the claim graph for these analyst files. Show the node table first, then the edge table, Mermaid, JSON, and finally the decision summary.
```

## Included Examples

- `examples/codex_review.md`
- `examples/claude_review.md`
- `examples/claim_graph_output.md`

## Repository Layout

```text
.
├── assets/
│   ├── banner.svg
│   ├── icon-small.svg
│   └── icon.svg
├── examples/
│   ├── claim_graph_output.md
│   ├── claude_review.md
│   └── codex_review.md
├── README.md
├── SKILL.md
└── agents/
    └── openai.yaml
```

## Main Files

- `SKILL.md`: graph workflow and output contract
- `agents/openai.yaml`: UI metadata and default prompt
- `examples/`: source Markdown files plus graph output example
