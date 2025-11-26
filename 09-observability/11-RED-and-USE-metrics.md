## 11. Understanding RED and USE metrics
### Question

What are RED and USE metrics, and how do they help observability?

### Short explanation

Tests your understanding of key observability principles for services and systems.

### Answer

- RED metrics (Rate, Errors, Duration) → Used for service-level monitoring.
- USE metrics (Utilization, Saturation, Errors) → Used for system/resource monitoring.
- They help identify bottlenecks, failures, and performance issues.

### Detailed Explanation
#### 🧾 Step 1: RED Metrics Example
- Rate: http_requests_total → requests per second
- Errors: http_requests_total{status=~"5.."}
- Duration: http_request_duration_seconds

#### 🧠 Step 2: USE Metrics Example
- Utilization: node_cpu_seconds_total
- Saturation: node_load1 → system queue depth
- Errors: node_filesystem_errors_total

#### ⚖️ Step 3: Application
- Combine metrics in Grafana for dashboards.
- Create alerts on thresholds to maintain service reliability.

### Real-world Insight

Using RED/USE metrics allowed us to differentiate between service issues (API latency) and infrastructure issues (node overload).

### Key takeaway

RED/USE metrics are foundational observability pillars for proactive monitoring.
