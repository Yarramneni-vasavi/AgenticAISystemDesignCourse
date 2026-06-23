## Re-ranking

Ranking is the process of ordering documents by how relevant they are to the query — so the best documents reach the LLM and the noise does not.

- Retreival Ranking
- Fusion Ranking
- Re-ranking

### Retrieval Ranking

- The DB scores and orders documents internally using BM25, cosine similarity, dot product, etc. 
- Your app only sees the output — a ranked Top-K list. 
- You do not control this ranking directly; you choose the retriever and its parameters.
- Types:
    - Sparse methods:
        - BM25
        - TF-IDF
        - Splade
    - Dense methods:
        - Cosine similarity
        - Dot product
        - DPR (Dense Passage Retrieval)
        - ColBERT (Retrieval Mode)
    
### Fusion Ranking

- Different retrievers find different relevant documents. We want to combine multiple ranked lists into a single, better list — without re-reading any document text.

- RRF (Reciprocal Rank Fusion)
    - A document should rank higher if:
        - It appears high in a retriever's results.
        - It appears in multiple retrievers' results.
    - RRF rewards both rank position and agreement across retrievers.
    - Simple, robust, and parameter-free (except k). 
    ```
    Formula : 
        RRF score (d) = Σ  1 / (k + rank)
        
        where:
            d = document/chunk
            rank = position of the document in a retriever's ranking
            k = smoothing constant (typically 60)

    ```

- Weighted Score Fusion
    - Combines raw scores directly. 
    - Requires scores to be on comparable scales (normalize first). 
    - α is a tunable hyperparameter.


### Re-ranking

- The retrieved Top-100 documents still contain noise. 
- We need a stronger, slower model to actually read query + document pairs and assign precise relevance scores.

- Strategies:
    - Cross-Encoder
        - Both query and document are concatenated and fed into a transformer together. 
        - Self-attention can attend across query and document tokens — capturing deep interactions impossible in bi-encoders.
        - Popular Cross Encoders:
            - BGE Reranker (BAAI) — open source, state of the art
            - Cohere Rerank — hosted API, easy integration
            - MonoT5 — T5-based sequence-to-sequence reranker
            - ms-marco-MiniLM — lightweight, fast
    - COLBERT Reranking
        - Encodes query and document separately (like bi-encoders), but stores per-token embeddings. 
        - At query time, computes MaxSim between every query token and every document token.
    - Learning-To-Rank (LTR)
        - Instead of running a transformer, extract hand-crafted features per (query, document) pair and train a gradient-boosted model or neural ranker on click/relevance data.
        - Common LTR models:
            - RankNet — pairwise neural ranking (early work from Burges)
            - LambdaMART — gradient boosted trees, optimizes NDCG directly
            - XGBoost Ranker — fast, widely used in production
            - ListNet / ListMLE — listwise approaches
    - MMR (Maximal Marginal Relevance)
        - MMR greedily selects documents that are relevant to the query AND dissimilar to already-selected documents.
        - Creates a Diverse context


# Design rationale: 

- Retrieval is cheap — cast a wide net. 
- Fusion is free — combine signals. 
- Re-ranking is expensive — run it on a small set. 
- LLM generation is most expensive — only feed it the best 5–10 docs to minimize hallucination and token cost.