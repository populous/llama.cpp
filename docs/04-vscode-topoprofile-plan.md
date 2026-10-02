# VS Code Profiling and TDA Visualization Plan

## Status

- Repository: `populous/llama.cpp`
- Related PR: `#1`
- Scope: VS Code-based profiling integration and TDA-result visualization
- Current phase: Planning
- Inference semantics: unchanged

## Purpose

This document defines the staged plan for using VS Code as the fixed development environment for the `llama.cpp` research fork and for connecting build, profiling, trace, and topological-analysis workflows.

```text
VS Code
  -> CMake Presets
  -> CProfile/CDebug
  -> llama-bench baseline
  -> Tracy/Perfetto/perf/VTune
  -> execution trace
  -> GUDHI-based TDA
  -> VS Code visualization
```

The initial goal is not to change llama.cpp inference behavior. It is to establish a reproducible build, execution, measurement, and analysis environment.

## Scope

### Included

- VS Code as the fixed development environment.
- CMake Tools and `CMakePresets.json` integration.
- Selectable CProfile backends.
- `llama-bench` baseline collection.
- CDebug contract instrumentation.
- Tracy live timelines.
- Perfetto trace storage and analysis.
- Optional perf or VTune detailed profiling.
- GUDHI-based persistence analysis.
- PNG, HTML, and Webview visualization in VS Code.
- Linking results to the source revision and build preset.

### Excluded from the first stage

- Changes to llama.cpp inference semantics.
- Instrumenting every backend or kernel from the beginning.
- Claims that TDA improves performance before experiments.
- Unqualified direct comparisons across different hardware.
- MCP/A2A/3D-RAG expansion before the llama.cpp pipeline is reproducible.

## Architecture

```text
CMakePresets.json
  -> reproducible build identity

llama-bench
  -> throughput/latency baseline

CDebug contract
  -> pre/body/post execution events

Tracy
  -> live developer timeline

Perfetto
  -> persistent trace and SQL analysis

perf/VTune
  -> CPU, cache, memory, and microarchitecture analysis

GUDHI
  -> persistence diagrams and topological features

VS Code
  -> build, run, inspect, and visualize
```

## Fixed VS Code environment

Recommended extensions:

```json
{
  "recommendations": [
    "ms-vscode.cmake-tools",
    "llvm-vs-code-extensions.vscode-clangd",
    "vadimcn.vscode-lldb",
    "ms-python.python",
    "ms-python.vscode-pylance",
    "eamodio.gitlens"
  ]
}
```

Optional trace viewer extensions may be used for local convenience, but Perfetto UI or a dedicated project Webview remains the long-term visualization target.

Recommended settings:

```json
{
  "cmake.useCMakePresets": "always",
  "cmake.configureOnOpen": false,
  "cmake.generator": "Ninja",
  "cmake.parallelJobs": 8,
  "C_Cpp.default.configurationProvider": "ms-vscode.cmake-tools",
  "clangd.arguments": [
    "--background-index",
    "--clang-tidy"
  ]
}
```

All builds must use a CMake preset. Developers should not manually edit the CMake cache for experiment configuration.

## CProfile backends

CProfile is the selectable profiling layer rather than a single profiler implementation.

```text
CProfile
├── NONE
├── PERF
├── TRACY
├── PERFETTO
└── VTUNE
```

| Backend | Role |
|---|---|
| `NONE` | Pure performance baseline |
| `PERF` | Linux CPU, PMU, and sampling |
| `TRACY` | Live zones and contract timeline |
| `PERFETTO` | Persistent trace, system timeline, and SQL |
| `VTUNE` | Optional Intel CPU/GPU/XPU analysis |

VTune is an optional backend. The project must remain buildable on systems where VTune is not installed.

## CMake preset plan

The shared `CMakePresets.json` should define reproducible configurations such as:

```text
cprofile-baseline
cprofile-live
cprofile-perf
cprofile-vtune
debug-asan
```

The local VTune installation path and local model path belong in `CMakeUserPresets.json` or environment variables, not in the shared project preset.

The baseline preset must disable instrumentation. The live and detailed profiles must be measured separately from the baseline because instrumentation changes runtime overhead.

## CDebug contracts

The first instrumented target is `llama_decode`.

```text
llama.decode
├── contract.pre
├── decode.body
└── contract.post
```

Initial preconditions:

```text
context_valid
batch_non_empty
sequence_valid
input_shape_valid
```

Initial postconditions:

```text
decode_success
logits_finite
output_shape_valid
kv_cache_valid
```

Common event fields:

```json
{
  "schema_version": "cdebug.event.v1",
  "run_id": "run-001",
  "trace_id": "trace-001",
  "span_id": "span-0042",
  "parent_span_id": "span-0001",
  "name": "llama.decode",
  "phase": "decode",
  "stage": "body",
  "start_time_ns": 1000000,
  "end_time_ns": 2800000,
  "duration_ns": 1800000,
  "status": "ok",
  "attributes": {
    "token_index": 42,
    "backend": "cpu",
    "layer": -1
  },
  "metrics": {
    "cpu_cycles": null,
    "cache_misses": null,
    "memory_bytes": null
  }
}
```

All profiler adapters must preserve the same `run_id`, `trace_id`, and `span_id` values.

## Baseline and detailed profiling

Performance baselines are measured with instrumentation disabled:

```text
cprofile-baseline
  -> llama-bench
  -> tokens/s
  -> latency
  -> standard deviation
```

Detailed analysis uses separate configurations:

```text
cprofile-live
  -> Tracy/Perfetto

cprofile-perf
  -> perf stat/record

cprofile-vtune
  -> VTune hotspots/threading/memory
```

Instrumented results must not be mixed with pure baseline results.

## TDA pipeline

```text
CDebug events
  -> feature vector
  -> baseline normalization
  -> point cloud
  -> GUDHI RipsPersistence
  -> persistence diagram
  -> persistence features
  -> baseline distance
  -> VS Code visualization
```

Initial point-cloud features:

```text
log_duration_ns
cpu_cycles
cache_miss_rate
memory_log_bytes
thread_wait_ratio
depth
token_index
layer
phase_id
status_id
```

The first derived result schema should record the run, construction method, diagrams, and summary features:

```json
{
  "schema_version": "topoprofile.persistence.v1",
  "run_id": "run-001",
  "method": {
    "complex": "vietoris_rips",
    "metric": "euclidean",
    "max_dimension": 2,
    "max_edge_length": 2.0
  },
  "diagrams": {
    "h0": [],
    "h1": []
  },
  "features": {
    "h0_count": 0,
    "h1_count": 0,
    "h1_total_persistence": 0.0,
    "persistence_entropy": 0.0
  }
}
```

TDA results are interpreted together with llama-bench latency, throughput, and variability. Persistence distance alone is not a regression verdict.

## VS Code visualization phases

### Phase 1: file-based output

```text
GUDHI
  -> PNG/SVG
  -> VS Code image preview
```

Outputs:

```text
persistence-diagram.png
persistence-barcode.png
feature-comparison.png
```

### Phase 2: HTML report

```text
GUDHI/Plotly
  -> feature-comparison.html
  -> VS Code/browser
```

The report should show:

- tokens/s;
- latency;
- standard deviation;
- persistence diagram;
- barcode;
- H0/H1 features;
- bottleneck distance;
- contract status and failures.

### Phase 3: VS Code Webview

```text
*.topoprofile.json
  -> custom editor
  -> TopoProfile Viewer
```

The viewer should contain:

```text
Run metadata
Benchmark summary
Contract status
Persistence diagram
Barcode
Point cloud projection
Baseline/current comparison
```

The Webview is an eventual integration target after the result schema is stable.

## Artifact layout

```text
artifacts/<run-id>/
├── manifest.json
├── benchmark.jsonl
├── cdebug.events.jsonl
├── cdebug.spans.jsonl
├── perf.data
├── perf.report
├── tracy/
├── perfetto.trace
├── point-cloud.json
├── persistence.json
├── persistence-features.json
├── topology-comparison.json
├── persistence-diagram.png
├── persistence-barcode.png
└── report.md
```

`manifest.json` must include at least:

```json
{
  "run_id": "run-001",
  "git_commit": "abc123",
  "preset": "cprofile-live",
  "model_sha256": "...",
  "backend": "cpu",
  "profiler": "tracy+perfetto",
  "topology_method": "gudhi-rips",
  "schema_versions": {
    "cdebug": "1.0.0",
    "topoprofile": "1.0.0"
  }
}
```

## Step-by-step implementation

### Step 1: VS Code build

- Install CMake Tools.
- Add and validate CMake presets.
- Verify `cprofile-baseline` and `debug-asan`.
- Configure and build from VS Code.

### Step 2: Baseline automation

- Add the `llama-bench` runner.
- Store JSONL output.
- Generate `manifest.json`.
- Ingest results into the baseline database.
- Compare compatible runs.

### Step 3: CDebug contract

- Add pre/body/post only around `llama_decode`.
- Emit JSONL events.
- Measure instrumentation overhead.

### Step 4: Tracy and Perfetto

- Add Tracy zones.
- Add Perfetto Track Events.
- Correlate `run_id`, `trace_id`, and `span_id`.
- Run both through VS Code tasks.

### Step 5: GUDHI derived schema

- Convert events to feature vectors.
- Apply baseline normalization.
- Compute Rips persistence.
- Store persistence JSON.
- Compare bottleneck and Wasserstein distances.

### Step 6: Visualization

- Add PNG/SVG rendering.
- Add Plotly HTML report.
- Freeze the `.topoprofile.json` schema.
- Add a VS Code Webview viewer.

### Step 7: Validation

The first validated workflow must answer:

1. Are execution structures repeatable under identical conditions?
2. Do persistent performance regressions coincide with topological changes?
3. Does the topological representation add information beyond latency and throughput?
4. Is instrumentation overhead acceptable?
5. Does the common schema remain valid across profiler backends?

## Follow-up documentation

After PR #1, the recommended documentation sequence is:

```text
docs/04-vscode-topoprofile-plan.md
docs/05-cprofile-presets.md
docs/06-cdebug-contract-schema.md
docs/07-tracy-perfetto-integration.md
docs/08-topoprofile-derived-schema.md
docs/09-vscode-visualization.md
docs/10-experiment-protocol.md
```

## Research discipline

- Record the upstream revision for every source-centered explanation.
- Keep observed behavior separate from hypotheses.
- Store commands and metadata with experimental results.
- Keep benchmark summaries separate from detailed traces.
- Do not interpret topological features without conventional metric comparison.
- Use one focused commit per coherent documentation or tooling change.