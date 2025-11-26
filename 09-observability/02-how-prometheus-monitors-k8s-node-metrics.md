## 2. How Prometheus monitors Kubernetes node metrics
### Question

How does Prometheus collect CPU, memory, and disk metrics from Kubernetes nodes?

### Short explanation

Checks your understanding of node exporters and metric collection.

### Answer

Prometheus uses node_exporter running on each node. Node_exporter exposes /metrics including CPU, memory, disk, and network stats, which Prometheus scrapes periodically.

### Detailed Explanation
#### 🧾 Step 1: Deploy Node Exporter
- helm install node-exporter prometheus-community/prometheus-node-exporter -n observability
- Runs as DaemonSet on all nodes.
- Exposes /metrics for system metrics.

#### 🔐 Step 2: Configure Prometheus scrape
```yaml
scrape_configs:
  - job_name: 'node'
    kubernetes_sd_configs:
      - role: node
    metrics_path: /metrics
```
#### 🧠 Step 3: Metrics collected

- node_cpu_seconds_total → CPU usage
- node_memory_MemAvailable_bytes → Memory
- node_filesystem_avail_bytes → Disk space

#### ⚖️ Step 4: Visualize & alert

- Grafana dashboards show node CPU, memory, disk usage.
- Example alert: CPU > 90% for 5 min.

### Real-world Insight

CPU spikes on worker nodes were missed until node_exporter was deployed as DaemonSet, highlighting the importance of node-level monitoring.

### Key takeaway

Node metrics give visibility into the underlying cluster resources, crucial for capacity planning and alerting.
