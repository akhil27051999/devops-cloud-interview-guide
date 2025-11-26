## 26. REST API latency & error dashboards
### Question

How do you monitor a REST API’s latency and errors in Grafana?

### Short explanation

Tests knowledge of combining RED metrics (Rate, Errors, Duration) for service observability.

### Answer

Use Prometheus metrics exposed by the API, such as http_requests_total and http_request_duration_seconds. Create Grafana panels for p90/p95/p99 latency and 5xx error rate.

### Detailed Explanation
#### 🧾 Step 1: Prometheus queries

- Errors:
  increase(http_requests_total{status=~"5.."}[10m])

- p95 latency:
  histogram_quantile(0.95, sum(rate(http_request_duration_seconds_bucket[5m])) by (le))

#### 🔐 Step 2: Grafana panels
- Line charts for latency over time
- Table for error counts per endpoint
- Use alerts for spikes in latency or errors

### Real-world Insight

Dashboards allowed us to detect a sudden spike in errors after a deployment, correlating with p95 latency and fixing the faulty endpoint.

### Key takeaway

Dashboards combining error rate and latency are essential for monitoring API health.
