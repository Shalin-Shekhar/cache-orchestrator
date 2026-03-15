# Competitive Landscape

## Competitive Matrix

| Competitor | Category | ML Caching | Edge Inference | Federated Learning | Verdict |
|------------|----------|-----------|----------------|-------------------|---------|
| Cloudflare | CDN | ✗ | ✗ | ✗ | No ML layer |
| Fastly | CDN | ✗ | ✗ | ✗ | No prediction |
| Akamai | CDN | Partial | ✗ | ✗ | Rule-based only |
| Redis/Memcached | Cache store | ✗ | ✗ | ✗ | Reactive only |
| Varnish + VCL | Cache proxy | ✗ | ✗ | ✗ | Manual & brittle |
| **Cache Orchestrator** | **ML Cache Orchestration** | **✓** | **✓** | **✓** | **Our moat** |

## Moat Analysis
Our defensible advantage compounds over time: more edge deployments → more training signal → better CPM models → better cache outcomes → more customers. A CPM trained on 10B daily requests outperforms one trained on 100M by 23% on precision. Scale is our structural advantage.

## Competitive Risks
- Major CDN (Cloudflare/Fastly) acquires or builds ML caching layer — mitigated by 18-24 month head start and data moat
- Open-source alternative emerges — mitigated by enterprise support, federated learning, and Model Hub differentiation