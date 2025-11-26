## 10. Alerting on DB, Vault, and ArgoCD restarts
### Question

How do you detect and alert if critical services restart unexpectedly?

### Short explanation

Tests monitoring of pod/container lifecycle events using kube-state-metrics.

### Answer

Monitor kube_pod_container_status_restarts_total metric from kube-state-metrics. Alert if the restart count increases within a time window.

### Detailed Explanation
#### 🧾 Step 1: Prometheus alert rule
```yaml
- alert: ServiceRestart
  expr: increase(kube_pod_container_status_restarts_total{namespace="default", pod=~"vault|argocd|postgres"}[5m]) > 0
  for: 1m
  labels:
    severity: critical
  annotations:
    summary: "Critical service restarted"
    description: "Vault, ArgoCD, or DB pod restarted in last 5 minutes"
```
#### 🔐 Step 2: Route alerts to Slack
- Same as other alerts using Alertmanager webhook.

#### 🧠 Step 3: Verification
- Grafana dashboards for pod restarts.
- Slack receives descriptive alert message.

### Real-world Insight

Detected unplanned ArgoCD restarts after cluster upgrade. Early alerting reduced downtime and accelerated root cause analysis.

### Key takeaway

Monitoring container restarts provides immediate insight into stability of critical services.
