## 7. Alerting for CPU & memory utilization
### Question

How do you configure alerts in Prometheus/Grafana for high CPU or memory usage?

### Short explanation

Tests ability to set up threshold-based alerts for system resource utilization.

### Answer

Define alert rules in Prometheus or Grafana based on node metrics. Alerts can be routed via Alertmanager to Slack.

### Detailed Explanation
#### 🧾 Step 1: Prometheus alert rule
```yaml
groups:
- name: NodeAlerts
  rules:
  - alert: HighCPU
    expr: 100 - avg(irate(node_cpu_seconds_total{mode="idle"}[5m])) * 100 > 90
    for: 5m
    labels:
      severity: critical
    annotations:
      summary: "CPU usage high on node"
```
#### 🔐 Step 2: Alertmanager config
```yaml
receivers:
- name: slack-notifications
  slack_configs:
  - channel: "#alerts"
    webhook_url: "<SLACK_WEBHOOK_URL>"
```
#### 🧠 Step 3: Visual verification

- Grafana panel shows CPU usage trend.
- Alerts trigger Slack messages when thresholds are exceeded.

### Real-world Insight

Alerts helped detect runaway processes that caused CPU spikes, enabling automated Slack notifications and faster response.

### Key takeaway

Threshold-based alerts allow teams to proactively address resource issues before impacting services.
