## 9. Alerting for latency spikes (p90, p95, p99)
### Question

How do you monitor and alert for high latency in APIs?

### Short explanation

Evaluates understanding of latency metrics and Prometheus histogram queries.

### Answer

Use Prometheus histogram metrics (e.g., http_request_duration_seconds_bucket) and histogram_quantile() to calculate p90, p95, p99 latencies. Trigger alerts when thresholds exceed limits.

### Detailed Explanation
#### 🧾 Step 1: Example PromQL query
```sh
histogram_quantile(0.95, sum(rate(http_request_duration_seconds_bucket[5m])) by (le)) > 0.5
```
- Calculates the 95th percentile of request duration over 5 minutes.

#### 🔐 Step 2: Define alert rule
```yaml
- alert: HighLatency
  expr: histogram_quantile(0.95, sum(rate(http_request_duration_seconds_bucket[5m])) by (le)) > 0.5
  for: 5m
  labels:
    severity: warning
  annotations:
    summary: "High latency detected"
```

#### 🧠 Step 3: Dashboard visualization
- Grafana panel for p90, p95, p99 latency over time.
- Can overlay request rate for correlation.

### Real-world Insight

Latency alerts helped identify network congestion between microservices, enabling early mitigation.

### Key takeaway

Percentile-based latency monitoring ensures users experience consistent performance even under load spikes.
