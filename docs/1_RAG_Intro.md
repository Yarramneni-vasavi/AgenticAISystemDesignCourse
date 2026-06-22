## Problems with LLMs:
1. Dated
2. No Memory, Every request considered as a fresh request
3. Fixed Context Window
4. Non-Deterministic
5. **Cannot perform any Action**

LLM is good at NATURAL LANGUAGE. Given a Question -> generate Answer
but LLM is SLOW, EXPENSIVE, OCCASIONALLY-WRONG, STATELESS.

## RAG - Retrieval Augmented Generation

Agent = LLM + Tool + Context + Harness

- Harness = Infrastructure we build around LLMs

### RAG AGENT

RAG Agent = LLM + Retrieval Tool

- Retrieval Tool is used to retrieve / fetch some information to pass additional context to LLM

The tool can be:
- A database - can be in-memory, persist or online solution(cloud)
    - Vector DB (Example: Chroma, Milvus, Pinecone, pgVector, Weaviate)
    - Graph DB (Example: Neo4j)
    - SQL DB (Example: Postgres, SQLite)
- web search
- A document (PDF, File)
- API
- Speadsheet (excel, csv)
- Code repo (github files)
- Gmail/Calendar
- Also a simple calculator function :p

### RAG pipelines:

2 types:
- Ingestion pipeline - source -> load -> chunk -> embed -> store
- Retrieval pipeline - user query -> embeddings -> search the store -> fetch top K chunks -> send to LLM  (user query + k chunks) -> generates response.

2 categories of RAG retrievals:
- Dense retrieval : the embedding vector has values in most or all of its dimensions
- Sparse retrieval : the keyword vector is mostly zeros - only terms that appear in the document get a non-zero value

### Chunking strategies:
- Chunk size - generally I prefer to choose 512 to 1024
- Chunk overlap
- AST (Abstract Syntax Tree) (used for codebases)
    - This is used because in the huge codebase, the normal test splitters will not work. The code has to be splitted understanding its semantics
    - For example: A 2 classes has 4 functions: with same function name in both. Now if we use normal text splitter, we CANNOT identify which function from which class.
    - So, AST splits first as per classes and if it seems class size is huge, It splits with in class, And if it still sees more size, Split as per functions.

