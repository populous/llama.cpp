# Project Purpose

## Primary purpose

Use llama.cpp as the real development target for a code-centered research project that connects build configuration, debugging, profiling, and execution-structure analysis.

The project asks whether a reproducible build and observation pipeline can reveal not only whether llama.cpp is correct or fast, but also whether its execution structure changes under different models, backends, quantization formats, and build configurations.

## Research statement

> Build and observe llama.cpp reproducibly, establish statistically meaningful performance baselines, and analyze selected execution traces for structural changes without changing inference semantics in the initial stage.

## Main components

### CMake++

CMake++ is the project-level build and experiment configuration layer. It manages build profiles, backend choices, sanitizer configurations, benchmark parameters, and reproducibility metadata. It is a project name, not a replacement for CMake.

### CDebug

CDebug is the observation layer with three responsibilities:

- correctness debugging through debuggers and sanitizers;
- performance debugging through benchmarks and profilers;
- structural debugging through traces and derived execution features.

### llama-bench

`llama-bench` provides the first repeatable performance baseline. Its output is stored with source revision, model identity, build preset, hardware, backend, and benchmark parameters.

### Topological profiling

Topological analysis is a later exploratory layer. It consumes selected detailed traces rather than replacing ordinary benchmark metrics. Topological features must be evaluated for repeatability and additional explanatory value against latency, throughput, memory, and variability.

## Development target

llama.cpp is the center of the initial implementation because it provides a concrete inference path, multiple backends, quantization choices, CMake build configurations, and measurable prompt-processing and generation performance.

## Initial deliverables

- source-centered documentation of the main inference path;
- reproducible CMake build profiles;
- a CDebug observation and metadata model;
- automated `llama-bench` JSONL baseline ingestion;
- regression comparison using compatible experiment keys;
- a prototype trace-to-feature pipeline for later TDA evaluation.

## Non-goals for the first stage

- changing llama.cpp inference semantics;
- claiming TDA improves performance before experiments;
- mixing results from incompatible hardware or model configurations;
- extending to agent systems before the llama.cpp pipeline is reproducible;
- replacing upstream user-facing documentation.
