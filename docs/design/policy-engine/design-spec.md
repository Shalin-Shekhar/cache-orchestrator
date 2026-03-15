# Policy Engine — Design Spec

**Status**: In Development (v0.5)
**Owner**: @platform-team
**Last Updated**: 2026-03-05

## Overview
The Policy Engine provides rule-based overrides that take precedence over CPM predictions. It handles compliance requirements, geographic restrictions, emergency cache invalidation, and customer-defined exclusion lists.

## Goals
- Policy evaluation in <1ms (must not add latency to hot path)
- Support per-customer, per-region, and global policies
- Emergency invalidation propagates to all edge nodes within 30 seconds
- Full audit log of all policy matches and overrides

## Policy Types

| Type | Example | Behavior |
|------|---------|----------|
| Exclude path | `/api/user/*` | Never cache or prefetch matching paths |
| Force TTL | `/api/config → ttl=60s` | Override CPM-suggested TTL |
| Geographic block | Block `/eu-content` outside EU | Prevent cross-region prefetch |
| Emergency purge | Purge `/api/products/123` | Immediate invalidation, all PoPs |
| Confidence floor | min_confidence=0.90 | Only prefetch if CPM score ≥ 0.90 |

## Policy DSL (YAML)
```yaml
policies:
  - id: exclude-user-data
    type: exclude_path
    pattern: "/api/user/*"
    reason: "PII - never cache"

  - id: emergency-purge-product
    type: purge
    pattern: "/api/products/123"
    expires_at: "2026-03-15T18:00:00Z"
```