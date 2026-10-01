# Research Roadmap

## Project position

llama.cpp is the primary development and validation target. The project is not starting from an abstract compiler concept; it starts with a real C/C++ inference system that can be built, modified, measured, and profiled.

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

## Stages

1. Document the source tree and main inference path.
2. Define CMake presets for Debug, sanitizer, RelWithDebInfo, profile, and release builds.
3. Explain model loading, context state, KV cache, decode, ggml graphs, and backend scheduling.
4. Automate `llama-bench` JSONL collection and CDebug baseline database ingestion.
5. Trigger detailed profiling only for persistent regressions.
6. Convert selected traces into feature vectors and topological profiles.
7. Extend the trace model to MCP, A2A, and 3D RAG workflows.

## Initial experiment

Use one model, one quantization format, a fixed prompt, fixed generation length, fixed backend, fixed thread count, and repeated measurements. Compare code or build changes only after the baseline is stable.

## Success criteria

- builds and experiments are reproducible;
- benchmark conditions are recorded with results;
- performance regressions are detected statistically;
- detailed profiler runs can be linked to a baseline run;
- topological features are compared with conventional latency and throughput metrics.
