## Types of RAG:

- Semantic RAG
- Lexical RAG
- Hybrid RAG
- Agentic RAG
- Graph RAG or G-RAG
- C-RAG or Corrective RAG
- Self RAG
- SQL RAG
- Web RAG
- Hierarchial RAG
- HyDE RAG
- RAPTOR RAG
- Vectorless RAG

### Semantic RAG
- It uses Vector DB tool for semantic search of documents.

### Lexical RAG
- It uses BM25 algorithm for exact match of words in documents. It is enhancement of TF-IDF algorithm by adding normalization.
- BM25 = Saturated(TF x TDF) + Normalization
    - Term Frequency (TF) - How often this word appear in this document
    - Inverse Document Frequency (IDF) - How rear is this word appear across all documents.

### Hybrid RAG
- Calling multiple retriever tools and RE-RANK the results before sending to LLM.
- RE-RANK uses strategies like RRF (Reciprocal Rank Fusion)

### Agentic RAG
- Given multiple retrievers, We ask LLM to make a decision on which retriever to call and RE-RANK the results.
- RE-RANK uses strategies like Cross-Encoder

### Graph RAG
- Retrieve relationships between entities, not just text chunks.

```
sentence: Cricketer Virat Kohli and Bollywood actress Anushka Sharma have been one of India's most admired couples.

knowledge graph:

Nodes:[{id: 'virat', type: 'person'}, {id: 'anushka', type: 'person'}, 
        {id: 'cricketer', type: 'profession'}, {{id: 'actress', type: 'profession'}}]

Edges == Relationships: [Relationship(source = Node(id: 'virat', type: 'person'), 
                             target = Node(id: 'anushka', type: 'person'), 
                             type='MARRIAGE')]

_________               _____________
| VIRAT |___marriage____|  ANUSHKA  |
|_______|               |___________|

```
- Knowledge is pre-indexed as a graph (nodes = entities, edges = relationships), and queries traverse that graph at search time. 
- Used in GraphRAG (Microsoft) and enterprise knowledge graph systems.

### Corrective RAG or C-RAG
- Add a critic/evaluator after retrieval that judges whether the retrieved chunks are actually good enough before passing to LLM.
- Critic here can be another LLM model or Cross encoder(a re-ranker).

### Self RAG
- The LLM itself generates special reflection tokens throughout the process — deciding at each step what to do next. No separate critic — the LLM is its own judge.

### SQL RAG
- If we use SQL database as a retrieval tool. It is SQL RAG
- Used for data analytics queries.

### Web RAG
- If we use web search as a retrieval tool. It is Web RAG
- Used to fetch the current latest information.

### Hierarchial RAG
- Stores information in levels of granularity. Search proceeds top-down, narrowing scope at each level.
```
Company
  └── Department
        └── Project
              └── Document
                    └── Chunk
```
- Hierarchial RAG can be used if:
    - Documents are already well structured (legal docs, manuals, policy files)
    - You know queries will follow a predictable pattern
    - You want fast, cheap retrieval

### HyDE RAG
- Hypothetical Document Enbeddings
- If user queries are not good enough - We can generate information from hypothetical documents.
- Adding some context and relavant terms

### RAPTOR RAG
- RAPTOR = Recursive Abstractive Processing for Tree Organized Retrieval
- If we have 100 files, we cluster them and generate summary from that small clusters and we do it recursively to form a tree like strcuture.

```
                 [Root Summary]
                /              \
        [Summary A]          [Summary B]
        /        \            /        \
  [Chunk 1] [Chunk 2]  [Chunk 3] [Chunk 4]
```
- Here, data generation is Bottom-up, But search is top-down.
- That means, If given a query we will first search in top node and check if summary matches and drill down to below node. until we find actual documents.
- Summary can be stored in a Graph DB.
- RAPTOR is great for
    - Large corpus, loosely structured
    - Queries range from high-level to very specific ("Summarize this entire codebase" vs "What does fn X do")
    - Multi-document reasoning needed
    - You want the system to self-organize knowledge

### Vectorless RAG

- A design philosophy — spend more effort at index time so you need less semantic search at query time
- It builds a hierarchial indexes (just like a table of contents for a book), So LLM reason over the index and identifies which chunk it has to go to.
    - Example: We have 2 chapters say, Networking and Databases. If i want to understand about SQL I will search index and check for Databases chapter and go to it.
- So, there is NO SEMANTIC SEARCH, that means NO EMBEDDINGS being used here. Hence called VECTORLESS
