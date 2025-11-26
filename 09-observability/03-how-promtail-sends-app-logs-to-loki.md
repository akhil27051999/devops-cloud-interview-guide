## 3. How Promtail sends application logs to Loki
### Question

How does Promtail collect logs from applications and send them to Loki?

### Short explanation

Tests your knowledge of log aggregation and filtering using Loki stack.

### Answer

Promtail runs as an agent on nodes, reads log files, labels them, and pushes to Loki via HTTP. Only configured logs (e.g., application logs) are sent.

### Detailed Explanation
#### 🧾 Step 1: Deploy Promtail
helm install promtail grafana/promtail -n observability

#### 🔐 Step 2: Configure scrape_configs
```yaml
scrape_configs:
  - job_name: system
    static_configs:
      - targets:
          - localhost
        labels:
          job: myapp
          __path__: /var/log/myapp/*.log
```

- __path__ defines which logs to send.
- labels tag logs for querying in Grafana.

#### 🧠 Step 3: Logs reach Loki
- Loki stores logs as streams with labels.
- Promtail can filter out system logs and send only application logs.

#### ⚖️ Step 4: Query logs in Grafana
```yaml
{job="myapp"} |= "ERROR"
```

### Real-world Insight

Filtering only app logs avoided unnecessary volume of system logs and reduced storage costs in Loki.

### Key takeaway

Proper labeling and filtering in Promtail ensures efficient log collection and querying in Loki.
