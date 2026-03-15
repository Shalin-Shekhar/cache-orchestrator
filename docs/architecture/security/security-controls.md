# Security Controls

## Control Catalogue

| Control ID | Control | Category | Status |
|------------|---------|----------|--------|
| SC-01 | Mutual TLS for all edge ↔ control plane communication | Transport | Implemented |
| SC-02 | Ed25519 model artifact signing | Integrity | Implemented |
| SC-03 | Differential privacy on gradient uploads (ε=1.0) | Privacy | Implemented |
| SC-04 | Threshold secret sharing for gradient aggregation | Privacy | Implemented |
| SC-05 | Edge node certificate rotation every 24h | Identity | Implemented |
| SC-06 | Rate limiting on gradient upload endpoint | Availability | Implemented |
| SC-07 | Tenant namespace isolation in Model Hub | Authorization | Implemented |
| SC-08 | SOC 2 Type II audit | Compliance | In Progress (Q2 2026) |
| SC-09 | Penetration test (external) | Assurance | Planned (Q3 2026) |
| SC-10 | ISO 27001 certification | Compliance | Planned (2027) |