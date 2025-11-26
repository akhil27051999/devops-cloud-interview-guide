## 28. Prometheus metrics retention and compaction
### Question

How do you manage Prometheus metrics retention and storage?

### Short explanation

Tests understanding of Prometheus TSDB and best practices for long-term monitoring.

### Answer

- Set --storage.tsdb.retention.time flag for time-based retention (e.g., 30d).
- Use --storage.tsdb.retention.size for size-based retention.
- Compaction is handled automatically by Prometheus to reduce disk usage.

### Detailed Explanation
#### 🧾 Step 1: Configure Prometheus deployment
```yaml
args:
  - "--storage.tsdb.path=/prometheus"
  - "--storage.tsdb.retention.time=30d"
  - "--storage.tsdb.retention.size=50GB"
```

#### 🔐 Step 2: Benefits
- Avoids uncontrolled disk usage
- Retains metrics for historical analysis

### Real-world Insight

Setting a 30-day retention prevented Prometheus from filling disk on PVC while keeping sufficient historical data for trend analysis.

### Key takeaway

Proper retention and compaction settings ensure Prometheus remains performant and cost-efficient.
