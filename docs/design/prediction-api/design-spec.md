# Prediction API — Design Spec

**Status**: In Development (v0.8)
**Owner**: @platform-team
**Last Updated**: 2026-03-10

## Overview
The Prediction API exposes CPM prefetch manifests to external CDN integrations and developer tooling via REST and gRPC interfaces.

## Goals
- Serve prefetch manifests with <5ms additional latency
- Support both REST (primary) and gRPC (secondary) transports
- OpenAPI 3.0 spec published and kept up-to-date
- Rate limiting: 10,000 requests/minute per customer

## REST API

### POST /v1/prefetch
Request prefetch manifest for a given signal window.

```json
POST /v1/prefetch
Authorization: Bearer {api_key}
Content-Type: application/json

{
  "customer_id": "cust_abc123",
  "pop_id": "lax-01",
  "signals": [
    { "path": "/api/products", "content_type": "application/json", "status": 200, "ts_ms": 1741234567890 }
  ]
}
```

Response:
```json
{
  "candidates": [
    { "path": "/api/products/featured", "score": 0.94, "ttl_seconds": 300 },
    { "path": "/api/categories", "score": 0.87, "ttl_seconds": 600 }
  ],
  "model_version": "cpm-v2.1.4",
  "confidence": 0.91,
  "latency_ms": 4
}
```