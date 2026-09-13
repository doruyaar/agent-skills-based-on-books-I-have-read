---
name: the-engineers-guide-to-rag
description: >-
  Practical retrieval-augmented generation rules distilled from Shivani Virdi's
  book "The Engineer's Guide to RAG": ingestion, chunking, hybrid retrieval,
  re-ranking, grounded answers, and RAG evaluation. Use only when the user
  explicitly asks for "The Engineer's Guide to RAG" or Shivani Virdi's guidance.
---

# The Engineer's Guide to RAG — Shivani Virdi

Rules distilled from *The Engineer's Guide to RAG* by Shivani Virdi. Apply them when building or improving a retrieval-augmented generation pipeline.

## Core Mission and Principles

- **Groundedness First**: Always use retrieved context before relying on the LLM's internal parametric memory.
- **RAG Is a Data Engineering Problem**: Treat output quality as bounded by the quality of the ingestion and retrieval layers, not just the model.
- **No Silent Failures**: Monitor retrieval quality and maintain prompt observability so degradations surface early.

## Indexing Pipeline (Knowledge Base Creation)

- **Clean and Curate on Ingest**: Remove junk text, footers, and irrelevant sections during ingestion to avoid "garbage in, garbage out".
- **Intelligent Chunking**: Chunk on structural boundaries (headings, paragraphs) or semantic shifts rather than arbitrary character counts.
- **Chunk Overlap**: Include a deliberate percentage of overlap between consecutive chunks to preserve contextual continuity.
- **Enrich with Metadata**: Attach metadata (source, timestamp, author, category) to each chunk to enable filtering and narrow the retrieval scope.
- **Domain-Appropriate Embeddings**: Choose embeddings for the domain; fine-tune when off-the-shelf models fail to capture specific jargon (e.g., legal or medical).

## Retrieval Strategies

- **Hybrid Retrieval**: Combine dense vector search with sparse keyword search (BM25) to handle both semantic meaning and exact identifiers like error codes or SKUs.
- **Re-Ranking**: Apply a cross-encoder re-ranker post-retrieval to prioritize the most useful context before passing it to the LLM.
- **Query Optimization**: Use techniques like multi-query expansion to generate semantic variants of a question and improve recall for vague queries.
- **Knowledge Graph RAG**: Consider graph-based RAG for multi-hop reasoning where relationships between entities matter more than individual text snippets.

## Augmentation and Prompt Engineering

- **Controlled Generation**: Explicitly instruct the model to say "I don't know" when the answer is absent from the provided context.
- **Contextual Prompting**: Guide the LLM to adhere strictly to retrieved information, reducing the risk of prior training overriding grounded data.
- **Few-Shot / Chain-of-Thought**: Apply few-shot or chain-of-thought prompting when specific output formats or step-by-step reasoning are required.

## Evaluation and Monitoring (The RAG Triad)

- **Retrieval Quality**: Measure Recall@k and Precision@k to confirm the right context is being fetched.
- **Faithfulness (Groundedness)**: Check that the generated answer is derived solely from the retrieved context, without hallucination.
- **Answer Quality**: Assess fluency, helpfulness, and relevance of the final response to the user's query.
- **LLM-as-a-Judge**: Use frameworks like RAGAs for automated, scalable evaluation of these metrics in development and production.

## Production and Maintenance (RAGOps)

- **Optimize Latency**: Use semantic caching to store and reuse responses for frequently asked queries.
- **Data Security**: Implement PII masking and redaction during both pre-processing and post-processing.
- **Plan for Scalability**: Use cloud-native, auto-scaling vector databases and infrastructure.
- **Iterate on Feedback**: Track which chunks contributed to successful responses and re-rank or fine-tune based on user interactions.
