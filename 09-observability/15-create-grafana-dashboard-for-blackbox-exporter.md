## 15. Creating dashboards for Blackbox metrics
### Question

How do you visualize endpoint availability and latency using Grafana?

### Short explanation

Checks understanding of combining Prometheus and Blackbox Exporter metrics for external monitoring.

### Answer

Use metrics like probe_success and probe_duration_seconds from Blackbox Exporter. Grafana panels can show uptime and latency for critical services.

### Detailed Explanation
#### 🧾 Step 1: Create panels
- Endpoint uptime:
```sh
probe_success{job="blackbox"}
```
- Latency:
```sh
probe_duration_seconds{job="blackbox"}
```

#### 🔐 Step 2: Visualization
- Use gauges for success/failure.
- Use line graphs for latency trends.

### Real-world Insight

Visualizing latency trends allowed early detection of network congestion affecting API performance.

### Key takeaway

Blackbox dashboards complement internal metrics by providing an external view of service health.
