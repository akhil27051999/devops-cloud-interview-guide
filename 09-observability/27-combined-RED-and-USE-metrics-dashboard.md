## 27. Combined RED/USE metrics dashboards
### Question

How do you combine RED (service) and USE (system) metrics for observability?

### Short explanation

Evaluates understanding of integrated monitoring.

### Answer

Create Grafana dashboards combining Prometheus metrics for service performance (RED) and system resources (USE). This helps identify whether issues are application-related or infrastructure-related.

### Detailed Explanation
#### 🧾 Step 1: RED metrics
- Rate: http_requests_total
- Errors: http_requests_total{status=~"5.."}
- Duration: http_request_duration_seconds

#### 🔐 Step 2: USE metrics
- CPU: node_cpu_seconds_total
- Memory: node_memory_MemAvailable_bytes
- Disk: node_filesystem_avail_bytes

#### 🧠 Step 3: Dashboard layout

- Rows for service metrics and system metrics
- Overlay resource usage with request rates and latency

### Real-world Insight

Correlating high API latency with high node CPU usage helped us identify that slow endpoints were caused by resource contention, not application code.

### Key takeaway

Integrated dashboards allow faster root cause analysis by combining system and application metrics.
