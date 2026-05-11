# Markdown AI Cross-Review Skill

A skill for comparing Markdown analysis written by multiple AI systems such as Codex, Claude, Gemini, and ChatGPT, then turning that comparison into a practical recommendation.

It is built for moments where one AI review is not enough and you want a more disciplined synthesis of agreement, disagreement, evidence quality, and risk.

## Best For

- Code review and architecture review
- Performance analysis
- Security review
- Requirement analysis
- Technical decision review
- Postmortem and incident analysis
- Comparing AI-generated reports before acting on them

## What This Skill Does

- Reads multiple Markdown analysis files
- Extracts claims, recommendations, assumptions, evidence, and caveats
- Builds an agreement or disagreement view across analysts
- Highlights unverified claims and risky recommendations
- Rates evidence quality
- Produces a clear action plan and final verdict
- Supports report mode, dialogue mode, and visual summary mode

## Input Pattern

Typical inputs look like this:

- Two or more Markdown files such as `codex_analyst.md`, `claude_review.md`, or `gemini_summary.md`
- Optional supporting material such as source code, benchmarks, logs, requirements, or design docs
- An optional objective such as:
  - "Help decide what to fix first"
  - "Which analyst is more convincing?"
  - "Turn this into a dialogue"
  - "Summarize this for executives"
  - "Convert this into an action plan"

## Output Style

The skill is designed to produce structured output that usually includes:

1. Executive summary
2. What analysts agree on
3. Where they disagree
4. Which side is more convincing on disputed points
5. Risk and correctness notes
6. Recommended action plan
7. Final verdict

When the user writes in Thai, the final answer should default to Thai.

## Example Prompts

```text
Use $markdown-ai-cross-review to compare these analyst Markdown files and tell me what we should do first.
```

```text
Use $markdown-ai-cross-review to evaluate which review is more convincing, especially on security and correctness risk.
```

```text
Use $markdown-ai-cross-review to turn these two AI reviews into a dialogue and end with a shared recommendation.
```

## Repository Layout

```text
.
├── README.md
├── SKILL.md
└── agents/
    └── openai.yaml
```

## Core Principle

This skill does not assume the loudest or most confident AI is correct.

It prefers recommendations that are:

- Low risk
- Easy to verify
- Supported by multiple analysts
- Backed by code, benchmarks, logs, or reproducible evidence
- Safe for correctness, security, and maintainability

It is intentionally skeptical of recommendations that promise performance wins while weakening security, changing behavior without a migration plan, or adding caching without an invalidation strategy.

## Main Files

- `SKILL.md`: the full operating instructions for the skill
- `agents/openai.yaml`: UI-facing metadata and default invocation prompt
