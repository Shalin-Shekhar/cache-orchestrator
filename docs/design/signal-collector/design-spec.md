# Signal Collector — Design Spec

**Status**: Implemented (v1.0)
**Owner**: @platform-team
**Last Updated**: 2026-01-20

## Overview
The Signal Collector is a lightweight telemetry agent co-located with the CDN edge worker. It intercepts request/response events, extracts features, and maintains an in-memory sliding window of recent signals for CPM Runtime consumption.

## Goals
- <1ms overhead on request processing hot path
- Zero persistent storage of raw request data (privacy)
- Support configurable signal window size (default: 32 requests)
- Expose signals via local IPC to CPM Runtime

## Non-Goals
- Long-term telemetry storage
- User-level tracking or PII collection

## Privacy Design
- **No URL query parameters captured** — only URL paths (query strings stripped)
- **No IP addresses** — geographic cluster ID derived from edge PoP, not client IP
- **No user identifiers** — cohort assignment based on aggregate traffic patterns, not individual sessions
- **In-memory only** — signal window lives in process memory; never written to disk

## Signal Window Schema
```json
{
  "window": [
    {
      "path": "/api/v2/products",
      "content_type": "application/json",
      "status": 200,
      "size_bucket": 3,
      "ts_ms": 1741234567890
    }
  ],
  "pop_id": "lax-01",
  "window_size": 32,
  "captured_at_ms": 1741234567999
}
```