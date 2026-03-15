# ADR-002: Federated Learning Framework

**Date**: 2025-12-01
**Status**: Accepted
**Deciders**: Priya Sharma (CTO), ML Team

## Context
We need a federated learning system to aggregate gradient updates from thousands of edge nodes without centralizing raw request data. Requirements: supports differential privacy, handles stragglers, works with ONNX models.

## Decision
Build a **custom gradient aggregation server** using FedAvg with differential privacy, rather than adopting an existing FL framework.

## Consequences
**Positive:**
- Full control over aggregation protocol, privacy budget, and model compatibility
- Lighter weight than Flower or PySyft — critical for edge node memory constraints
- Can optimize specifically for our gradient shape and communication pattern

**Negative:**
- Higher initial engineering investment (~8 weeks)
- Must maintain custom implementation long-term

## Alternatives Considered
- **Flower (flwr)**: Mature framework but Python-heavy; ONNX integration awkward; rejected
- **PySyft**: Strong privacy primitives but heavy dependency tree; rejected
- **TensorFlow Federated**: TF-only ecosystem; incompatible with our ONNX stack; rejected