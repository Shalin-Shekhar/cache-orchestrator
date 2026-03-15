# Cache Prediction Model (CPM)

## Architecture

The CPM is a 4-layer transformer encoder trained to predict cache-worthy content from request sequence context.

### Model Specification
| Parameter | Value |
|-----------|-------|
| Architecture | Transformer encoder (4 layers, 8 heads) |
| Parameters | ~12M (pre-quantization) |
| Size on disk | 4MB (INT8 quantized) |
| Input sequence | 32 most recent requests (sliding window) |
| Output | Ranked prefetch candidates with confidence scores |
| Training data | Anonymized, aggregated edge request telemetry |
| Update cadence | Every 6 hours (federated aggregation) |

### Input Features
```
Per-request features (32 per window):
├── URL path embedding (512-dim, normalized)
├── Content-type token (API, HTML, JSON, media, static)
├── HTTP method (GET/HEAD/POST)
├── Response status (2xx/3xx/4xx/5xx bucket)
├── Response size bucket (logarithmic)
├── Time-since-last-request (ms, log-scaled)
└── Geographic cluster ID (edge PoP region)

Global context features:
├── Hour-of-day encoding (sinusoidal)
├── Day-of-week encoding
├── Rolling cache hit rate (5-min window)
└── Active user cohort size
```

### Training
- **Objective**: Next-request prediction (masked autoregressive)
- **Loss**: Cross-entropy over URL path vocabulary (top 100K paths)
- **Optimizer**: AdamW, lr=1e-4, cosine decay
- **Batch size**: 2048 sequences (federated micro-batch: 256)
- **Evaluation metric**: Precision@K (K=10, 20, 50)

## Performance Benchmarks
| Metric | CPM v2.1 | Baseline (LRU) | Improvement |
|--------|----------|----------------|-------------|
| Cache hit rate | 91.3% | 58.0% | +33.3pp |
| P95 prefetch precision | 87.2% | N/A | — |
| False positive rate | 4.1% | N/A | — |
| P99 inference latency | 6.2ms | 0ms (no inference) | +6.2ms overhead |