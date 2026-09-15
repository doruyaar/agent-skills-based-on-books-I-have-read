---
name: book-ai-engineering
description: >-
  Engineering rules for building reliable applications on top of foundation
  models, distilled from Chip Huyen's book "AI Engineering": prompts as
  versioned assets, structured outputs, evaluation, and reliability. Use only
  when the user explicitly asks for "AI Engineering" or Chip Huyen's guidance.
---

# AI Engineering — Chip Huyen

Rules distilled from *AI Engineering* by Chip Huyen. Apply them when building applications on top of foundation models.

## AI Architectural Invariants

- **Prompts as Versioned Assets**: Decouple prompt logic from application logic by treating prompts as versioned, reviewable assets rather than inline literals.
- **No Raw Model Execution**: Never let a foundation model execute raw strings against a shell, database, or API without a predefined, sandboxed schema.
- **Output Analysis Layer**: Always validate model responses against a schema in a dedicated output-analysis layer before they reach the UI or downstream services.
- **Idempotent Pipelines**: Design data pipelines to be idempotent so they safely handle the non-deterministic nature of foundation model outputs and retries.

## Prompt Engineering & LLM Interaction

- **Negative Constraints**: Include explicit "must never do" constraints in system prompts, not just positive instructions.
- **Edge-Case Few-Shot Examples**: Use few-shot examples that represent edge cases, not only the happy path.
- **Externalize Model Parameters**: Never hardcode model-specific parameters (temperature, top_p, etc.) in functional code; source them from a configuration provider.
- **Prefer Structured Outputs**: Unless the task is trivial, prefer structured outputs (JSON/Pydantic) over free-form natural language strings.

## Data-Intensive Applications & RAG

- **Semantic Chunking**: Chunk documents by structure (headers, paragraphs) rather than fixed token counts.
- **Filter Before Vector Search**: Never query a vector database without a metadata filter when the context is known, to narrow the search space.
- **Source Attribution**: Always include source attribution in the final output to enable human-in-the-loop verification.
- **Distillation Pattern**: Use a "flashcard"/distillation pattern to convert high-volume document data into discrete, queryable knowledge units.

## Evaluation and Reliability

- **Golden Dataset**: Maintain a golden dataset of expected input/output pairs for prompt regression testing.
- **Evaluate Before Deploy**: Never ship a prompt change to production without running automated evaluation against a benchmark suite.
- **Retry with Backoff**: Implement retry logic with exponential backoff for 429 (rate limit) and 5xx (server error) responses.
- **Log Request ID and Latency**: Log `request_id` and latency for every foundation model call to enable bottleneck analysis.

## State and Context Management

- **Bound Conversation History**: Truncate or summarize conversation history once it exceeds ~60% of the model's context window.
- **Protect PII**: Never pass PII to a foundation model unless using a dedicated, privacy-compliant endpoint.
- **Contextual Compression**: Strip irrelevant tokens from retrieved documents via a contextual-compression step before passing them to the LLM.
