## 13. Creating Grafana dashboards for node metrics
### Question

How do you monitor CPU, memory, and disk usage of nodes in Grafana?

### Short explanation

Checks your understanding of visualizing node-level system metrics for observability.

### Answer

- Use Prometheus node_exporter metrics in Grafana panels. Key metrics include node_cpu_seconds_total, node_memory_MemAvailable_bytes, and node_filesystem_avail_bytes.

### Detailed Explanation
#### 🧾 Step 1: Add Prometheus as data source
- Grafana → Configuration → Data Sources → Prometheus

#### 🔐 Step 2: Create panels
- CPU Usage:
  100 - (avg by(instance) (rate(node_cpu_seconds_total{mode="idle"}[5m])) * 100)

- Memory Usage:
  node_memory_MemTotal_bytes - node_memory_MemAvailable_bytes

- Disk Usage:
  (node_filesystem_size_bytes - node_filesystem_avail_bytes) / node_filesystem_size_bytes * 100

#### 🧠 Step 3: Dashboard layout
- Group metrics by node.
- Use threshold colors for high usage (red > 90%).

### Real-world Insight

Visualizing node metrics allowed preemptive scaling decisions and detection of resource-heavy pods.

### Key takeaway

Node metrics dashboards provide visibility into cluster health and resource usage trends.
