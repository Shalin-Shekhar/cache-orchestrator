# Helm Charts

Kubernetes deployment charts for all Cache Orchestrator services.

| Chart | Description |
|-------|-------------|
| `cpm-runtime/` | Edge inference sidecar DaemonSet |
| `model-hub/` | Model registry API + worker |
| `federated-trainer/` | Gradient aggregation server |
| `prediction-api/` | REST/gRPC prediction endpoint |
| `analytics/` | ClickHouse + dashboard backend |

## Deploy
```bash
helm upgrade --install cache-orchestrator ./helm/model-hub   --namespace cache-orchestrator   --values helm/model-hub/values.prod.yaml
```