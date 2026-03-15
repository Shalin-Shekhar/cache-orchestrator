# Federated Learning Pipeline

## Overview
Cache Orchestrator uses federated learning to continuously improve the CPM without centralizing raw request data. Edge nodes compute local gradient updates; only gradients (never request logs) are transmitted to the control plane.

## Architecture

```
Edge Node 1 ──┐
Edge Node 2 ──┤──▶ Secure Aggregator ──▶ Global Model Update ──▶ Model Hub
Edge Node N ──┘         (FedAvg)
```

## Protocol
1. **Round initialization**: Model Hub broadcasts current CPM weights to participating edges (every 6h)
2. **Local training**: Each edge trains on its local request window (max 1,000 steps)
3. **Gradient upload**: Edge node encrypts and uploads gradient delta (not raw data)
4. **Aggregation**: Secure aggregator applies FedAvg with differential privacy noise (ε=1.0, δ=1e-5)
5. **Model publish**: Updated global model published to Model Hub; edges pull on next round

## Privacy Guarantees
- **Differential Privacy**: (ε=1.0, δ=1e-5) per round per edge node
- **Secure Aggregation**: Gradients encrypted with threshold secret sharing (t-of-n)
- **No raw data egress**: Signal Collector processes data in-memory only; no persistent logging of user request data
- **Audit logging**: All gradient uploads logged with edge node ID, timestamp, and gradient norm

## Participation Requirements
- Minimum 100 requests in local window to participate in round
- Edge nodes with <1% uptime in last 24h excluded from aggregation
- Anomalous gradient norms (>3σ) rejected before aggregation