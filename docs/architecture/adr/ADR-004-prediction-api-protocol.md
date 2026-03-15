# ADR-004: Prediction API Protocol

**Date**: 2026-02-20
**Status**: Proposed
**Deciders**: Platform Team

## Context
The Prediction API must serve prefetch manifests to CDN integrations. We need to decide between gRPC and REST as the primary protocol.

## Options
1. **gRPC only** — lower latency, streaming support, strong typing; less accessible for simple integrations
2. **REST only** — maximum compatibility; higher overhead per call
3. **REST + gRPC** — both supported; higher maintenance burden

## Proposed Decision
Ship **REST as primary** (JSON, OpenAPI spec) with **gRPC as secondary** (Protobuf, opt-in). REST covers 80% of integration use cases; gRPC offered for high-throughput enterprise integrations.

_Status: Under review — decision expected 2026-03-30_