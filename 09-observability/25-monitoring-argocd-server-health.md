## 25. Monitoring ArgoCD server health
### Question

How do you monitor the health and uptime of the ArgoCD server?

### Short explanation

Checks understanding of monitoring a critical deployment with metrics and external probes.

### Answer

Use Blackbox Exporter for uptime, and Prometheus metrics from ArgoCD server. Alert if probe_success is 0 or API latency exceeds threshold.

### Detailed Explanation
#### 🧾 Step 1: Blackbox probe for ArgoCD
```yaml
static_configs:
  - targets:
      - https://argocd.example.com
params:
  module: [http_2xx]
```

#### 🔐 Step 2: Prometheus alert for uptime
```yaml
- alert: ArgoCDDown
  expr: probe_success{job="blackbox", instance="argocd.example.com"} == 0
  for: 1m
```

#### 🧠 Step 3: Prometheus alert for latency
```yaml
- alert: ArgoCDHighLatency
  expr: probe_duration_seconds{job="blackbox", instance="argocd.example.com"} > 0.5
  for: 5m
```

### Real-world Insight

Alerts on ArgoCD downtime allowed the team to react to a failed automated deployment before users were affected.

### Key takeaway

Combining uptime and latency monitoring provides a complete view of service health.
