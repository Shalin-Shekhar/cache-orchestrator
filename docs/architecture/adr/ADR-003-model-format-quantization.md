# ADR-003: Model Format & Quantization Strategy

**Date**: 2026-01-15
**Status**: Accepted
**Deciders**: ML Team, Platform Team

## Decision
Use **INT8 post-training quantization** via ONNX Runtime quantization tools. Target size: <5MB. Calibration dataset: 10,000 representative request sequences sampled from production telemetry.

## Consequences
- Model size: 48MB (FP32) → 4MB (INT8) — 12x reduction
- Accuracy degradation: <0.8% on Precision@10 benchmark — acceptable
- P99 latency: 6ms on ARM64 (vs. 18ms FP32) — meets budget