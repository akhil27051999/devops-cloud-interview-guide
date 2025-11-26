## 6. How to create Grafana dashboards for application error logs
### Question

How do you monitor application errors using Grafana and Loki?

### Short explanation

This evaluates your ability to correlate logs with metrics for application observability.

### Answer

Use Loki as a data source in Grafana and query structured application logs. Apply labels for filtering (e.g., job="myapp").

### Detailed Explanation
#### 🧾 Step 1: Add Loki as data source

- Grafana → Data Sources → Add Loki
- URL: http://loki.observability.svc.cluster.local:3100

#### 🔐 Step 2: Create log panels
- Query for errors:
```sh
{job="myapp"} |= "ERROR"
```
- Filter by severity or request path using labels.

#### 🧠 Step 3: Visualize logs
- Use table or logs panel to show recent errors.
- Combine with Prometheus metrics for request rates, latency.

### Real-world Insight

By combining Loki error logs with Prometheus latency metrics, we quickly pinpointed slow endpoints causing error spikes.

### Key takeaway

Structured logs in Loki + Grafana visualization provide deep insights into application health.
