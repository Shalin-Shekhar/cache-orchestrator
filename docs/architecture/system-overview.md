# System Overview

## Platform Architecture

Cache Orchestrator consists of five core subsystems that work together to deliver predictive edge caching.

```
┌─────────────────────────────────────────────────────────────────┐
│                        EDGE NODE                                │
│  ┌──────────────┐   ┌──────────────┐   ┌────────────────────┐  │
│  │ Signal       │──▶│ CPM Runtime  │──▶│ Cache Population   │  │
│  │ Collector    │   │ (Inference)  │   │ (Prefetch Engine)  │  │
│  └──────────────┘   └──────┬───────┘   └────────────────────┘  │
│                            │ gradient updates                   │
└────────────────────────────┼────────────────────────────────────┘
                             ▼
┌─────────────────────────────────────────────────────────────────┐
│                      CONTROL PLANE                              │
│  ┌──────────────┐   ┌──────────────┐   ┌────────────────────┐  │
│  │ Federated    │   │  Model Hub   │   │ Analytics &        │  │
│  │ Trainer      │   │  (Registry)  │   │ Dashboard          │  │
│  └──────────────┘   └──────────────┘   └────────────────────┘  │
│  ┌──────────────┐   ┌──────────────┐                           │
│  │ Prediction   │   │ Policy       │                           │
│  │ API (gRPC)   │   │ Engine       │                           │
│  └──────────────┘   └──────────────┘                           │
└─────────────────────────────────────────────────────────────────┘
```

## Component Responsibilities

| Component | Location | Responsibility |
|-----------|----------|----------------|
| Signal Collector | Edge | Capture request sequences, timing, content metadata |
| CPM Runtime | Edge | Run quantized ML model, output prefetch manifest |
| Prefetch Engine | Edge | Pull predicted content into cache proactively |
| Federated Trainer | Control plane | Aggregate gradient updates, publish new model versions |
| Model Hub | Control plane | Version, A/B test, deploy, and roll back CPM models |
| Prediction API | Control plane | Expose prefetch manifest via REST/gRPC to integrations |
| Policy Engine | Control plane | Rule-based overrides, compliance, emergency invalidation |
| Analytics Dashboard | Control plane | Real-time metrics, model confidence, cost attribution |

## Key Design Principles
1. **Edge-first**: All latency-sensitive inference runs on the edge node, never the cloud
2. **Privacy-preserving**: Raw request data never leaves the edge; only gradients are transmitted
3. **Platform-agnostic**: Integrates via standard APIs with any CDN or edge runtime
4. **Graceful degradation**: If CPM inference fails, falls back to standard TTL-based caching