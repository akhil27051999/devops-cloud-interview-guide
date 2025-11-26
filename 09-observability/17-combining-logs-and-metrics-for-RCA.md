## 17. Combining logs and metrics for root cause analysis
### Question

How do you correlate application logs and Prometheus metrics for troubleshooting?

### Short explanation

Tests your ability to combine observability pillars for root cause analysis.

### Answer

Use Grafana to display metrics (Prometheus) and logs (Loki) side by side. Use labels (like pod, request_id) to correlate spikes in metrics with log entries.

### Detailed Explanation
#### 🧾 Step 1: Ensure consistent labeling

- Promtail should add labels like job="myapp", pod="myapp-xyz"

#### 🔐 Step 2: Create dashboard with mixed panels
- Metrics panel: request latency or error rate
- Logs panel: |= "ERROR" filtered by pod

#### 🧠 Step 3: Investigate issues
- Identify spikes in latency or errors
- Correlate timestamps with log entries for root cause

### Real-world Insight

Correlating high latency with application exceptions in logs helped quickly fix a misconfigured external API call.

### Key takeaway

Combining logs and metrics provides end-to-end observability, enabling faster issue resolution.
