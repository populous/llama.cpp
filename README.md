# llama.cpp

![llama](https://raw.githubusercontent.com/ggml-org/llama.brand/refs/heads/master/cover/llama-cpp/cover-llama-cpp-dark.svg)

<div align="center">
<b>LLM inference in C/C++</b>
</div>

## Research fork

This fork preserves the upstream llama.cpp user-facing documentation while adding a code-centered research workspace under `docs/`.

The research direction connects reproducible CMake configurations, CDebug instrumentation, llama-bench performance baselines, and later topological analysis of execution traces.

See [`docs/00-roadmap.md`](docs/00-roadmap.md), [`docs/01-project-purpose.md`](docs/01-project-purpose.md), and [`docs/02-research-scope.md`](docs/02-research-scope.md) for the research purpose and scope.

For the complete upstream project documentation, build instructions, supported backends, tools, and contribution guidance, refer to the upstream [llama.cpp repository](https://github.com/ggml-org/llama.cpp).

## Description

The main goal of `llama.cpp` is to enable LLM (and VLM) inference with minimal setup and state-of-the-art performance on a wide range of hardware - locally and in the cloud.

- Plain C/C++ implementation without any dependencies
- Apple silicon is a first-class citizen - optimized via ARM NEON, Accelerate and Metal frameworks
- AVX, AVX2, AVX512 and AMX support for x86 architectures
- RVV, ZVFH, ZFH, ZICBOP and ZIHINTPAUSE support for RISC-V architectures
- 1.5-bit, 2-bit, 3-bit, 4-bit, 5-bit, 6-bit, and 8-bit integer quantization for faster inference and reduced memory use
- Custom CUDA kernels for running LLMs on NVIDIA GPUs (support for AMD GPUs via HIP and Moore Threads GPUs via MUSA)
- Vulkan and SYCL backend support
- CPU+GPU hybrid inference to partially accelerate models larger than the total VRAM capacity

The `llama.cpp` project is built on top of the [ggml](https://github.com/ggml-org/ggml) library.

## Documentation

- [How to build](docs/build.md)
- [Research roadmap](docs/00-roadmap.md)
- [Project purpose](docs/01-project-purpose.md)
- [Research scope](docs/02-research-scope.md)

For the complete upstream documentation and current supported backend list, see the [upstream repository](https://github.com/ggml-org/llama.cpp).

## Contributing

Contributions to the upstream project should follow the upstream [CONTRIBUTING.md](CONTRIBUTING.md). Research documentation and experiments in this fork should preserve reproducibility and identify the upstream revision on which they are based.
