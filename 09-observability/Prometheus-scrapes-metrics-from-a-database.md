## 1. How Prometheus scrapes metrics from a database

### Question
How does Prometheus collect metrics from a database like PostgreSQL?

### Short explanation
This checks your understanding of Prometheus architecture, exporters, and how metrics are exposed.

### Answer
Prometheus uses a database exporter (e.g., postgres_exporter) to expose metrics at an HTTP endpoint. Prometheus scrapes this endpoint periodically and stores metrics in its time-series database.

## Detailed Explanation

### 🧾 Step 1: Deploy database exporter
```sh
helm install pg-exporter prometheus-community/postgres-exporter \
  --set datasource.url=postgresql://user:password@mydb:5432/dbname
```

- Runs an HTTP server at port 9187 exposing /metrics.

Connects to DB and exposes metrics like pg_up, pg_stat_activity_count.

### 🔐 Step 2: Configure Prometheus to scrape
```yaml
scrape_configs:
  - job_name: 'postgres'
    static_configs:
      - targets: ['pg-exporter.observability.svc.cluster.local:9187']
```
### 🧠 Step 3: Scrape and store metrics

- Prometheus pulls /metrics every 15s.
- Stores metrics with labels (job, instance) in TSDB.

### ⚖️ Step 4: Use metrics in dashboards/alerts

Example alert:
```yaml
- alert: HighDBConnections
  expr: pg_stat_activity_count > 100
  for: 5m
  labels:
    severity: warning
  annotations:
    summary: "High DB connections"
```
### Summary Table
| Component         | Role                     |
| ----------------- | ------------------------ |
| PostgreSQL DB     | Source of metrics        |
| postgres_exporter | Exposes DB metrics       |
| Prometheus        | Scrapes & stores metrics |
| Grafana           | Visualizes metrics       |
| Alertmanager      | Sends alerts             |

### Real-world Insight

We had pg_up=0 alerts because the exporter couldn’t connect due to a NetworkPolicy. Understanding the scrape flow helped fix it quickly.

### Key takeaway

Prometheus requires exporters for metrics it cannot natively access; proper setup ensures monitoring and alerting for DBs.
