## 22. Kubernetes observability edge case: CrashLoopBackOff
### Question

How do you detect and alert on pods in CrashLoopBackOff?

### Short explanation

Tests ability to monitor Kubernetes pod health.

### Answer

Use kube_pod_container_status_restarts_total metric from kube-state-metrics. Alert if the restart count increases rapidly.

### Detailed Explanation
#### 🧾 Step 1: Prometheus alert
```yaml
- alert: PodCrashLoop
  expr: increase(kube_pod_container_status_restarts_total[5m]) > 3
  for: 5m
  labels:
    severity: critical
```
#### 🔐 Step 2: Dashboard visualization
- Grafana panel for restart trends by namespace and pod.

### Real-world Insight

Detected a CrashLoopBackOff in a DB pod due to misconfigured secrets. Alerting enabled immediate remediation.

### Key takeaway

Monitoring container restarts helps detect unstable deployments and misconfigurations quickly.
