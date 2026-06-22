## RAG Metrics

### Golden Data Set:

- It is like having unit tests for normal application.
- TDD (Test Driven Development) -> EDD (Eval Driven Development)

- Types of data the dataset SHOULD have?
    - Fact
    - Multi-hop
    - Summary
    - Comparision
    - Procedure
    - Ambiguous

- How to create?
    - Human created
    - Production logs
    - Synthetic generation

### Metrics:
- Retrieval Metrics
- Generation Metrics
- End to End Metrics
- Chunking metrics

#### Retrieval Metrics

- Precision = Relevant Retrieved Items / Total Retrieved Items
    - Talks about accuracy
    - In RAG we use Precision@k = Relevant Retrieved Items / Top K Retrieved Items
    - High precision = little noise in results
    - Low precision = garbage in reuslts

- Recall = Relevant Retrieved Items / Total Relevant Items
    - Talks about completeness
    - In RAG we use Recall@k = Relevant Retrieved Items / Top K Relevant Items
    - High Recall = FEW useful docs missing
    - Low Recall = many useful docs missing

- Hit Rate = true/false = 1 if any relevant chunk exists in Top K
    - Relevant chunk at Rank 1 and Rank 20 get the same hit score.

but a relevant chunk at Rank 1 is much better than at Rank 10 because the LLM pays more attention to early chunks.

- MRR (Mean Reciprocal Rank)
    - Only cares about the first relevant chunk. All chunks treated as equally relevant (binary).
    - Use when you just need one good chunk to answer the question.

- NDCG (Normalized Discounted Cumulative Gain)
    - Cares about the entire ranking order and graded relevance. Rewards putting the best chunks first.
    - Use when evaluating reranking quality or when chunks have different levels of usefulness.

#### Generation Metrics

- Faithfulness
    - Measures whether the generated answer is grounded in the retrieved context
    - Faithfulness = Claims supported by retrieved context/Total claims in the answer
    - requires an LLM judge
    - High Faithfulness: LLM stays within the retrieved context. No hallucination
    - Low Faithfulness: LLM is adding facts from training data not in the retrieved chunks

- Answer Relevancy - 
    - Measures whether the generated answer actually addresses the question that was asked. 
    - An answer can be perfectly faithful to the context and factually correct — and still not answer the question.
    - requires an LLM judge
    - High Answer Relevancy = Answer directly addresses the question asked.
    - Low Answer Relevancy = Answer talks around the topic but misses the point of the question.

#### End to End Metrics

- Factual Correctness