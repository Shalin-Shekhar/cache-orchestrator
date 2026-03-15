# Analytics Dashboard — Design Spec

**Status**: Planned (v0.1 wireframes)
**Owner**: @product-team
**Last Updated**: 2026-02-28

## Overview
The Analytics Dashboard provides real-time and historical visibility into cache performance, CPM model behavior, cost attribution, and system health.

## Goals
- Real-time cache hit/miss rate (refresh: 10s)
- CPM model confidence score distribution per PoP
- Prefetch precision and false-positive rate trends
- Cost attribution: origin compute saved vs. CDN cost
- Model A/B test comparison view

## Key Metrics Displayed

| Metric | Refresh | Chart Type |
|--------|---------|------------|
| Cache hit rate (%) | 10s | Line chart |
| Prefetch precision@K | 1min | Line chart |
| CPM inference P99 (ms) | 10s | Line chart |
| Origin requests saved | 1min | Area chart |
| Cost saved ($/day) | 1hr | Bar chart |
| Model confidence distribution | 5min | Histogram |
| False positive rate (%) | 5min | Line chart |

## Tech Stack
- **Frontend**: React + Recharts
- **Backend**: GraphQL subscriptions over WebSocket (real-time)
- **Data store**: ClickHouse (time-series aggregates)
- **Alerting**: Integration with PagerDuty, OpsGenie