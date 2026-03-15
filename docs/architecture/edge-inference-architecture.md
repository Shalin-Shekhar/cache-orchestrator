# Edge Inference Architecture

## Overview
The CPM Runtime is a containerized inference engine deployed directly on edge PoPs. It processes real-time signal data and produces a ranked prefetch manifest every 500ms.

## Hardware Targets
| Platform | Chip | Inference Latency (P99) |
|----------|------|------------------------|
| Cloudflare Workers | V8 Isolate (x86) | 8ms |
| Fastly Compute | Wasm (x86/ARM64) | 6ms |
| Bare-metal PoP | ARM64 (Ampere Altra) | 4ms |
| AWS Lambda@Edge | x86_64 | 11ms |

## Model Serving Stack
```
Request signal stream
       │
       ▼
┌─────────────────────┐
│   Feature Extractor │  ← URL normalization, cohort embedding, time encoding
└──────────┬──────────┘
           ▼
┌─────────────────────┐
│   CPM (ONNX)        │  ← 4MB quantized transformer, INT8
│   Transformer       │
└──────────┬──────────┘
           ▼
┌─────────────────────┐
│  Confidence Filter  │  ← Threshold: 82% (configurable)
└──────────┬──────────┘
           ▼
┌─────────────────────┐
│  Prefetch Manifest  │  ← Ranked list: [path, score, TTL-override]
└─────────────────────┘
```

## Model Lifecycle at the Edge
1. **Deployment**: Model Hub pushes new CPM version as signed ONNX artifact
2. **Validation**: Edge node runs shadow mode for 5 minutes before switching traffic
3. **Rollback**: Automatic rollback if P99 latency exceeds 15ms or hit-rate drops >5%
4. **Update cadence**: Model retraining every 6 hours; emergency hotfix path available