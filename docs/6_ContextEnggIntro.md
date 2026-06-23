# Context Engineering Introduction

It deals with:
- What to send to LLM
- How to structure it - Possible Architectures
- Context Evaluation
- Token Budgeting
- Memory (Short-term, Long-term)

Context contains:
- System Prompt
- Conversation History
- Tool outputs
- Current user query

**Context Order also Matters**:
- Start
    - contains Instructions, query
    - called Primary effect
    - It is important because how training data is usually structured.
- Middle - Generally gets least attention
    - White Paper: [Lost in the Middle](https://arxiv.org/pdf/2307.03172)
- End 
    - called Recency effect
    - It is important because LLM's are designed in such a way that to generate next word they see the last few words.
    - Example from my observation:
        ```
        You: Who is modi ?
        LLM: Narendra Modi is an ......

        You: who is lata ?
        LLM: Lata Mangeshkar was a renowned ......

        You: when HE was born ?
        LLM: Lata Mangeshkar was born on September 28, 1929. It seems there might be some confusion, as Lata Mangeshkar is a female.....

        You: Which 2 persons i talked about now ?
        LLM: In your previous queries, you mentioned two individuals:

        1. **Lata Mangeshkar**: A legendary Indian playback singer,......

        2. **Narendra Modi**: The Prime Minister of India, serving since May 26, 2014. .......

        You: 
        ```
        **LLM resolves pronouns based on recency in context, not your intent.**

Strategies to improve the Context:
- Context Expansion - Example: Hyde RAG concept - Adding some extra context.
- Context Compression - Example: RAPTOR concept - Summarizing things.
- Context Selection - choosing Top K.
- Context pruning - deduplication, removing stale/irrelavant data
- Context Ordering - Reorder context so that we can give importance to middle also.