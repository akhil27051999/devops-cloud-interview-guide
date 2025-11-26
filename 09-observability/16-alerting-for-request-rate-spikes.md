## 16. Alerting for request rate spikes
### Question

How do you alert if the number of requests suddenly increases?

### Short explanation

Evaluates monitoring traffic patterns and detecting abnormal load.

### Answer

Use Prometheus metric http_requests_total. Trigger an alert if request rate exceeds a threshold over a given time window.

### Detailed Explanation
#### 🧾 Step 1: Define Prometheus alert
```yaml
- alert: HighRequestRate
  expr: increase(http_requests_total[5m]) > 1000
  for: 5m
  labels:
    severity: warning
  annotations:
    summary: "High request rate detected"
```

#### 🔐 Step 2: Notification
- Send alert via Alertmanager to Slack or email.

#### 🧠 Step 3: Verification
- Grafana shows request trends, correlate with latency and error metrics.

### Real-world Insight

Early detection of traffic spikes helped prevent server overload and auto-scale pods to maintain SLAs.

### Key takeaway

Monitoring request rate is critical for proactive scaling and preventing service degradation.
