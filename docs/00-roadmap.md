# Research Roadmap

## Project position

This fork uses `llama.cpp` as the primary development and validation target. It does not begin with an abstract compiler concept; it begins with a real C/C++ inference system that can be built, modified, measured, and profiled.

The upstream llama.cpp code and user-facing documentation remain the reference implementation. Research-specific material is maintained under `docs/` and is intended to remain separable from upstream behavior changes.

## Architecture

```text
CMake++
  -> reproducible build and experiment configuration
CDebug
  -> correctness, performance, and execution observation
llama-bench
  -> repeatable performance baseline
Trace/TDA
  -> structural analysis of execution behavior
Feedback
  -> source, backend, and build refinement
```

In this project, “CMake++” is a project name for the CMake-based build and experiment orchestration layer. It does not claim to replace CMake.

## Stages

1. Document the source tree and main inference path.
2. Define CMake presets for Debug, sanitizer, RelWithDebInfo, profile, and release builds.
3. Explain model loading, context state, KV cache, decode, ggml graphs, and backend scheduling.
4. Automate `llama-bench` JSONL collection and CDebug baseline database ingestion.
5. Trigger detailed profiling only for persistent regressions.
6. Convert selected traces into feature vectors and topological profiles.
7. Extend the observation model to MCP, A2A, and 3D RAG workflows.

## Initial experiment

Use one model, one quantization format, a fixed prompt, fixed generation length, fixed backend, fixed thread count, and repeated measurements. Compare code or build changes only after the baseline is stable.

## Success criteria

- builds and experiments are reproducible;
- benchmark conditions are recorded with results;
- performance regressions are detected statistically;
- detailed profiler runs can be linked to a baseline run;
- topological features are repeatable and compared with conventional latency, throughput, memory, and variability metrics.
