# Research Scope

## Phase 1: Source-centered explanation

Document the following execution path with source paths, symbols, and commit references:

```text
model file
  -> GGUF/model loading
  -> llama_model
  -> llama_context and memory
  -> tokenization and batch
  -> llama_decode
  -> ggml graph
  -> backend scheduler
  -> logits and sampling
```

## Phase 2: Build and experiment control

Create reproducible configurations for:

- Debug;
- AddressSanitizer/UndefinedBehaviorSanitizer;
- RelWithDebInfo;
- profiling;
- Release;
- CPU and selected accelerator backends.

## Phase 3: Baseline automation

Run a fixed `llama-bench` matrix and store:

- source revision;
- build preset and cache;
- compiler and operating system;
- hardware and backend;
- model path and SHA-256;
- quantization;
- prompt and generation sizes;
- batch and thread settings;
- average, deviation, and individual samples.

## Phase 4: Detailed profiling

When a baseline regression persists, collect function, kernel, memory, synchronization, and host-device transfer data with the appropriate platform profiler.

## Phase 5: Topological analysis

Transform selected traces into event records and feature vectors. Compare persistence or other topological features with conventional regression metrics.

## Phase 6: Domain extension

After the llama.cpp pipeline is stable, extend the event model to MCP tool calls, A2A messages, retries, retrieval, generation, and 3D RAG actions.

## Constraints

The first result must be reproducible on a fixed machine and a fixed model. Debug builds are for correctness, not performance baselines. Hardware, model, backend, and benchmark parameters must be part of the comparison key.
