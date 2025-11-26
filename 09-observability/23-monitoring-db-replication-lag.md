## 23. Monitoring database replication lag
### Question

How do you monitor replication lag in a database like PostgreSQL?

### Short explanation

Checks understanding of DB-specific metrics monitoring.

### Answer

Use DB exporter metrics like pg_replication_lag or pg_stat_replication to measure lag between master and replicas. Alert if lag exceeds threshold.

### Detailed Explanation
#### 🧾 Step 1: Prometheus query
```sh
pg_replication_lag{job="postgres"} > 1000
```

#### 🔐 Step 2: Alert rule
```yaml
- alert: DBReplicationLag
  expr: pg_replication_lag{job="postgres"} > 1000
  for: 5m
  labels:
    severity: critical
```

### Real-world Insight

Alerts for replication lag prevented stale reads from replicas in production, improving SLA compliance.

### Key takeaway

Monitoring replication lag is critical for data consistency and system reliability.
