# Model Hub — Design Spec

**Status**: Implemented (v1.0)
**Owner**: @platform-team
**Last Updated**: 2026-02-15

## Overview
The Model Hub is the central registry for CPM model versions. It handles model publishing, versioning, A/B testing, and rollback. Edge nodes poll the Model Hub to receive updated model artifacts.

## Goals
- Zero-downtime model deployments via gradual rollout
- A/B testing with traffic split control
- Automatic rollback on performance regression
- Full audit trail for all model deployments

## Non-Goals
- Model training (handled by Federated Trainer)
- Raw telemetry storage

## Key Workflows

### Deploy New Model Version
1. Federated Trainer uploads signed ONNX artifact to Model Hub
2. Model Hub validates artifact signature and runs benchmark suite
3. Admin configures rollout policy (e.g., 5% → 25% → 100% over 2 hours)
4. Model Hub pushes deployment config to edge nodes
5. Edge nodes pull artifact, run shadow validation, then swap

### A/B Test
1. Define two model versions (control, treatment) and traffic split
2. Model Hub routes requests to control/treatment per edge node assignment
3. Analytics Dashboard surfaces hit-rate and latency comparison
4. Admin promotes winner or rolls back loser

### Rollback
- Automatic: if treatment P99 latency > 15ms or hit-rate drops >5%
- Manual: admin clicks rollback in dashboard or calls `POST /models/{id}/rollback`