## 18. Promtail log filtering strategies
### Question

How do you ensure Promtail only sends relevant logs to Loki?

### Short explanation

Checks understanding of efficient log collection and labeling for observability.

### Answer

Promtail uses scrape_configs and pipeline_stages to filter logs based on file paths, labels, or content patterns. Only application logs are sent, ignoring system logs.

### Detailed Explanation
#### 🧾 Step 1: Filter by log path
```yaml
scrape_configs:
- job_name: myapp-logs
  static_configs:
  - targets:
      - localhost
    labels:
      job: myapp
      __path__: /var/log/myapp/*.log
```

#### 🔐 Step 2: Filter by log content
```yaml
pipeline_stages:
- match:
    selector: '{job="myapp"}'
    stages:
    - regex:
        expression: "ERROR|WARN"
```

#### 🧠 Step 3: Benefits
- Reduces volume of logs in Loki
- Focuses on actionable logs (errors, warnings)
- Improves query performance in Grafana

### Real-world Insight

Filtering only error logs reduced Loki storage usage by 70% while keeping all critical information for troubleshooting.

### Key takeaway

Proper filtering in Promtail ensures efficient and meaningful log collection.
