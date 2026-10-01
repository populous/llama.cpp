# llama.cpp Code Research

This branch establishes a code-centered research workspace using llama.cpp as the primary development target.

## Purpose

The project connects reproducible CMake configurations, CDebug instrumentation, llama-bench performance baselines, and later topological analysis of execution traces.

```text
CMake configuration
  -> llama.cpp build
  -> CDebug observation
  -> llama-bench baseline
  -> trace and topology analysis
  -> feedback to code and build design
```

## Initial scope

- document the llama.cpp source and execution flow;
- maintain reproducible CPU inference experiments;
- automate llama-bench JSONL baseline ingestion;
- separate correctness, performance, and structural debugging;
- extend the observation model later to MCP, A2A, and 3D RAG systems.

See `docs/00-roadmap.md` for the staged plan.
