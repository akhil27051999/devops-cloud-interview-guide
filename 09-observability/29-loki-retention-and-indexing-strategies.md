## 29. Loki retention and indexing strategies
### Question

How do you manage log retention and indexing in Loki?

### Short explanation

Tests understanding of log lifecycle management in Loki for long-term observability.

### Answer

- Set retention periods using table_manager.retention_period.
- Use labels efficiently for indexing.
- Enable chunking and compression for storage efficiency.

### Detailed Explanation
#### 🧾 Step 1: Configure Loki
```yaml
table_manager:
  retention_period: 168h # 7 days
```
- Use job, pod, namespace as labels
- Compress log chunks to save storage

#### 🔐 Step 2: Benefits
- Reduces disk usage
- Improves query performance
- Retains critical logs for analysis

### Real-world Insight

Proper labeling and 7-day retention allowed fast log searches and reduced storage costs by 50%.

### Key takeaway

Efficient log retention and indexing strategies make Loki scalable and performant.
