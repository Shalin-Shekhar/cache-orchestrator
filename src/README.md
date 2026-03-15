# Source Code

Application source code for the Cache Orchestrator platform.

## Planned Structure
```
src/
├── cpm-runtime/        ← ONNX inference engine (Go)
├── signal-collector/   ← Edge telemetry agent (Rust)
├── prediction-api/     ← REST + gRPC API server (Go)
├── model-hub/          ← Model registry service (Python)
├── federated-trainer/  ← Gradient aggregation server (Python)
├── policy-engine/      ← Rule evaluation service (Go)
└── analytics/          ← Dashboard backend (TypeScript)
```

**Prerequisites**: Go 1.22+, Rust 1.76+, Python 3.11+, Docker 24+