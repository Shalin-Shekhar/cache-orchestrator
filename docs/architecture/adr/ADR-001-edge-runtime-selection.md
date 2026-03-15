# ADR-001: Edge Runtime Selection

**Date**: 2025-11-10
**Status**: Accepted
**Deciders**: Priya Sharma (CTO), Platform Team

## Context
We need to run ML inference (the CPM transformer) directly on edge nodes across heterogeneous hardware (x86_64, ARM64) and multiple edge platforms (Cloudflare Workers, Fastly Compute, bare-metal PoPs). The runtime must fit within a 10MB memory budget and achieve <10ms P99 inference latency.

## Decision
Use **ONNX Runtime** as the inference engine for all edge deployments.

- Models are exported from PyTorch to ONNX format post-training
- INT8 quantization applied via ONNX Runtime quantization tools
- WebAssembly compilation target used for Cloudflare Workers and Fastly Compute
- Native ARM64 binary used for bare-metal PoPs

## Consequences
**Positive:**
- Single model format works across all target platforms
- INT8 quantization reduces model size from ~48MB to 4MB
- ONNX Runtime WASM achieves 6–8ms P99 on all target platforms
- Strong ecosystem: well-maintained, Microsoft-backed, broad hardware support

**Negative:**
- ONNX export can be fragile for non-standard PyTorch ops (mitigated by CI validation)
- WASM execution ~20% slower than native — acceptable given latency budget

## Alternatives Considered
- **TensorFlow Lite**: Good mobile support but weaker WASM story; rejected
- **TorchScript**: Tighter PyTorch integration but no WASM target; rejected
- **Custom C++ kernel**: Maximum performance but prohibitive maintenance cost; rejected