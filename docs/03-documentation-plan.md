# Documentation Plan

Each chapter is written as an executable code-reading note rather than a general overview.

## Chapter order

1. Project overview and source tree.
2. CMake targets, presets, and backend options.
3. GGUF model loading and tensor ownership.
4. `llama_model`, `llama_context`, memory, and KV cache.
5. Tokenization, batching, and `llama_decode`.
6. ggml tensors, computation graphs, attention, and FFN.
7. Backend discovery, buffer placement, graph splitting, and kernel dispatch.
8. `llama-bench` test matrix, repetitions, and JSONL output.
9. CDebug data model and instrumentation points.
10. Baseline database ingestion and regression detection.
11. Trace-to-feature conversion and topological profiling.
12. Extension to MCP, A2A, and 3D RAG.

## Chapter template

Every chapter should contain:

- objective and boundaries;
- prerequisite chapters;
- source files and relevant commit;
- call flow;
- key types and functions;
- state and memory changes;
- build or runtime conditions;
- profiling points;
- reproducible experiment;
- observed result;
- current limitations;
- next research question.

## Maintenance rules

- Link explanations to source paths and revisions.
- Separate observed behavior from hypotheses.
- Store commands and experiment metadata with results.
- Keep benchmark summaries separate from detailed profiler traces.
- Do not interpret topological features without a conventional metric comparison.
- Add one focused commit per coherent documentation or tooling change.
