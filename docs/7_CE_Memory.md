# Context Engineering - Memory

- Short Term memory - STM - Session memory
- Long Term Memory - LTM - across sessions

## Short Term Memory
- Everything relavant to current task/session.
    - Conversation history - all prior user and assistant turns in the session
    - Current plan - active task list, goals, and next steps
    - Tool outputs - API responses, code execution results, search outputs
    - Scratchpad - intermediate reasoning and chain-of-thought steps
    - Agent state - workflow position, loop counters, execution flags
    - Working context - retrieved docs or preferences injected for this call

- Storage:
    - RAM
    - Redis
    - SQL/NoSQL checkpoints
    - Workflow state stores (LangGraph, Temporal)
    - File-based checkpoints

- Managing Large Short Term Memory:
    - Sliding Window - Keep only the most recent N messages.
    - Rolling Summarization - periodically compress older messages into a running summary.
    - Messages Trimming - Remove low-value msgs that adds no value.
    - Semantic Recall - Move older history to vector database retrieve only what's relavant
    - State Extraction - Extract structured key facts instead of storing raw messages
        ```
        {
            "name": "Vasavi",
            "goal": "Build rag pipeline",
            "preferred_language": "Python"
        }
        ```
    - Hierarchical Summarization - Create summaries at multiple levels - for extreme long conversations.
    - Context Compression - Use an LLM to rewrite history into a shorter, information-dense representation.

- Common Production Pattern
    ```
    Recent Messages
          +
    Running Summary
          +
    Retrieved Memories
          +
    Structured State
    ```
    Example: Keep the last 10 messages verbatim + a running conversation summary + relevant memories retrieved from history + user profile (name, goal, preferred language). This covers recency, compression, fuzzy recall, and structured facts - all in one context.


## Long Term Memory
- Long-term memory persists outside the context window in external stores. 
    - Personalization
    - Historical recall
    - Knowledge retention

- It survives across sessions, and is selectively retrieved into context when relevant.

- 3 conceptual Types:

    The difference is not where they are stored, but what type of information they represent. A vector database can hold all three types. A SQL table can hold all three types. Type is about semantics and not infrastructure.

    - Semantic Memory - stores facts and knowledge
    - Episodic Memory - stores events and experiences
    - Procedural Memory - skills and workflows

- Storages used:
    - Vector Database - store semantic facts, episodic events, summaries and procedural steps.
    - SQL - Filterable by userID, date etc.
    - Key-Value / Graph - KV for exact fact lookups; graphs for relational semantic knowledge where entity connections matter.
