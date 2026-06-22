# Evaluations

Metrics Vs Evals:

- Metrics are the numbers you measure.
- Evals are the process of judging whether your system is good enough. They use metrics but are broader than just metrics.

```
EVAL PROCESS
├── Define test cases (golden dataset)
├── Run your RAG system on each test case
├── Measure metrics
│   ├── Retrieval → Precision, Recall
│   ├── Generation → Faithfulness, Relevance
│   └── End-to-end → Task success
├── LLM / Human judges scores
├── Compare against threshold
│   ├── Pass → ship it
│   └── Fail → debug which component broke
└── Iterate and re-eval
```


## Popular Eval Frameworks for RAG

- RAGAS - RAG specific metrics (faithfulness, context relevance)
- LangSmith - Eval runs, tracing, human feedback
- DeepEval - Unit test style evals for LLM outputs
- TruLens - RAG triad evaluation
