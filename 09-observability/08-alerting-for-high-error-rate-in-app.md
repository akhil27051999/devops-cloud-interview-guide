## 8. Alerting for high error rate in the application
### Question

How do you configure alerts for spikes in HTTP 5xx errors in a REST API?

### Short explanation

This tests understanding of RED metrics (Rate, Errors, Duration) and alerting on application errors.

### Answer

Use Prometheus to query http_requests_total metric with a filter for 5xx status codes. Trigger an alert if the error count exceeds a threshold over a given time window.

### Detailed Explanation
#### 🧾 Step 1: Define Prometheus alert
```yaml
groups:
- name: AppAlerts
  rules:
  - alert: HighErrorRate
    expr: increase(http_requests_total{status=~"5.."}[10m]) > 10
    for: 5m
    labels:
      severity: critical
    annotations:
      summary: "Spike in 5xx errors"
      description: "More than 10 errors in last 10 minutes"
```
#### 🔐 Step 2: Route alert via Alertmanager
```yaml
receivers:
- name: slack-alerts
  slack_configs:
  - channel: "#alerts"
    webhook_url: "<SLACK_WEBHOOK_URL>"
```
#### 🧠 Step 3: Verification

- Grafana dashboard shows http_requests_total with error breakdown.
- Slack receives a descriptive alert if threshold is breached.

### Real-world Insight

We detected a new deployment causing intermittent 5xx errors and fixed a misconfigured API route by correlating alert notifications with logs.

### Key takeaway

RED metrics (Rate, Errors, Duration) enable effective monitoring and alerting for service reliability.
