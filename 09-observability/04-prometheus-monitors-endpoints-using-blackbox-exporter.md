## 4. How Prometheus monitors endpoints using Blackbox Exporter
### Question

How does Prometheus monitor endpoint uptime and latency using the Blackbox Exporter?

### Short explanation

This checks if you understand active probing of services and endpoints for observability.

### Answer

Prometheus uses the Blackbox Exporter to actively probe endpoints (HTTP, HTTPS, ICMP) and expose results (success, latency) at a /probe endpoint, which Prometheus scrapes.

### Detailed Explanation
#### 🧾 Step 1: Deploy Blackbox Exporter
- helm install blackbox-exporter prometheus-community/blackbox-exporter -n observability

- Runs as Deployment/Service exposing /probe endpoint.

#### 🔐 Step 2: Configure Prometheus scrape
```yaml
scrape_configs:
  - job_name: 'blackbox'
    metrics_path: /probe
    params:
      module: [http_2xx]
    static_configs:
      - targets:
        - https://argocd.example.com
        - https://vault.example.com

```
- module defines probe type (HTTP, TCP, ICMP)
- targets are endpoints to monitor

#### 🧠 Step 3: Metrics collected
- probe_success → 1 if probe succeeded, 0 if failed
- probe_duration_seconds → Latency of endpoint
- probe_http_status_code → HTTP status code

#### ⚖️ Step 4: Alerts
- Example alert for downtime:
```yaml
- alert: EndpointDown
  expr: probe_success == 0
  for: 1m
  labels:
    severity: critical
  annotations:
    summary: "Endpoint is down"
```

### Real-world Insight

Monitoring Vault and ArgoCD endpoints using Blackbox exporter helped detect downtime caused by network misconfigurations before users noticed it.

### Key takeaway

Blackbox exporter allows active monitoring of endpoints, enabling proactive detection of service downtime or latency spikes.
