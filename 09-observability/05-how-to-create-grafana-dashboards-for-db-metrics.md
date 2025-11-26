## 5. How to create Grafana dashboards for DB metrics
### Question

How do you visualize database metrics in Grafana?

### Short explanation

Tests your understanding of dashboards, Prometheus queries, and visualization for observability.

### Answer

Create a dashboard in Grafana, add Prometheus as a data source, and use PromQL queries to visualize key DB metrics.

### Detailed Explanation
#### 🧾 Step 1: Add Prometheus data source
- Grafana → Configuration → Data Sources → Add Prometheus
- URL: http://prometheus.observability.svc.cluster.local:9090

#### 🔐 Step 2: Create Dashboard Panels
- Panel 1: DB up status
```sh
pg_up{job="postgres"}
```

- Panel 2: Active connections
```sh
pg_stat_activity_count{job="postgres"}
```

- Panel 3: Cache hit ratio
```sh
(pg_cache_hit_ratio{job="postgres"})
```

#### 🧠 Step 3: Customize visualization

- Use line graphs for trends
- Threshold colors for alerts (red > 80% usage)
- Group related metrics in a single row

### Real-world Insight

We created DB dashboards to monitor connection saturation and query latency. It helped us detect spikes during batch jobs and optimize queries.

### Key takeaway

Grafana dashboards turn raw metrics into actionable visualizations, simplifying observability.
