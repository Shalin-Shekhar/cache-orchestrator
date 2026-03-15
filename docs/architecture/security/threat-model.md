# Threat Model

## Scope
This threat model covers the Cache Orchestrator edge deployment, control plane, and federated learning pipeline.

## Assets
| Asset | Sensitivity | Description |
|-------|-------------|-------------|
| CPM model weights | High | Proprietary ML model; IP and competitive advantage |
| Edge request telemetry | Medium | Aggregate usage patterns (no PII stored) |
| Gradient updates | Medium | Could reveal request patterns if intercepted |
| Model Hub API credentials | Critical | Full model deployment access |
| Customer configuration | High | Cache policy rules, API keys |

## Threat Categories (STRIDE)

### Spoofing
- **T1**: Attacker spoofs edge node identity to inject malicious gradient updates
  - **Mitigation**: Mutual TLS for all edge ↔ control plane communication; edge node certificates rotated every 24h

### Tampering
- **T2**: Attacker tampers with CPM model artifact in transit
  - **Mitigation**: All model artifacts signed with Ed25519; edge node validates signature before loading

### Information Disclosure
- **T3**: Gradient updates leak request pattern information
  - **Mitigation**: Differential privacy (ε=1.0) applied before upload; secure aggregation with threshold secret sharing

### Denial of Service
- **T4**: Malicious edge node floods control plane with gradient updates
  - **Mitigation**: Rate limiting per edge node; anomalous gradient norm rejection (>3σ)

### Elevation of Privilege
- **T5**: Compromised edge node attempts to access other customers' model configurations
  - **Mitigation**: Tenant isolation at Model Hub API layer; edge nodes scoped to single customer namespace