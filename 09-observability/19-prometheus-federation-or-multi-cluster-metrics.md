## 19. Prometheus federation / multi-cluster metrics
### Question

How can Prometheus be used to aggregate metrics from multiple clusters?

### Short explanation

Tests understanding of Prometheus federation for multi-cluster observability.

### Answer

Use Prometheus federation by configuring a central Prometheus to scrape metrics from remote Prometheus instances. Only aggregated or selected metrics are scraped to reduce load.

### Detailed Explanation
#### 🧾 Step 1: Configure remote Prometheus scrape
```yaml
scrape_configs:
- job_name: 'federate'
  honor_labels: true
  metrics_path: '/federate'
  params:
    'match[]': ['up', 'http_requests_total']
  static_configs:
  - targets: ['prometheus-cluster1:9090', 'prometheus-cluster2:9090']
```

#### 🧠 Step 2: Benefits
- Central dashboard for all clusters
- Avoids querying each cluster separately
- Can aggregate metrics like total request rate across clusters

### Real-world Insight

Federation allowed our team to monitor global request volume across 3 clusters and detect regional anomalies.

### Key takeaway

Prometheus federation provides scalable multi-cluster observability.
