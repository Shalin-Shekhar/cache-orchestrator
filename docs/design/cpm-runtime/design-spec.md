# CPM Runtime — Design Spec

**Status**: Implemented (v1.0)
**Owner**: @platform-team
**Last Updated**: 2026-03-01

## Overview
The CPM Runtime is a containerized ML inference engine that runs on edge nodes. It accepts a window of recent request signals, runs the Cache Prediction Model, and returns a ranked prefetch manifest.

## Goals
- Run CPM inference in <10ms P99 on all target platforms
- Fit within 10MB total memory footprint (model + runtime)
- Support hot model swaps with zero request impact
- Expose simple gRPC interface for integration

## Non-Goals
- Training or fine-tuning models (handled by Federated Trainer)
- Persistent storage of request logs
- Direct cache manipulation (handled by Prefetch Engine)

## Interface

### Input
```protobuf
message SignalWindow {
  repeated RequestSignal signals = 1;  // Last 32 requests
  string edge_pop_id = 2;
  string customer_id = 3;
}

message RequestSignal {
  string url_path = 1;
  string content_type = 2;
  int32 status_code = 3;
  int64 response_size_bytes = 4;
  int64 timestamp_ms = 5;
}
```

### Output
```protobuf
message PrefetchManifest {
  repeated PrefetchCandidate candidates = 1;
  float model_confidence = 2;
  string model_version = 3;
}

message PrefetchCandidate {
  string url_path = 1;
  float confidence_score = 2;
  int32 suggested_ttl_seconds = 3;
}
```

## Deployment
- Distributed as OCI container image (~18MB compressed)
- Sidecar deployment alongside CDN edge worker
- Health check: `GET /healthz` → 200 OK
- Metrics: Prometheus-compatible `/metrics` endpoint