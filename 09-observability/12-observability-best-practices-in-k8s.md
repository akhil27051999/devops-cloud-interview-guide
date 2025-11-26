## 12. Observability best practices in Kubernetes
### Question
 
What are best practices for setting up an observability stack in Kubernetes?

### Short explanation

Checks understanding of architecture, namespace isolation, and persistent storage for observability tools.

### Answer

- Deploy Prometheus, Loki, Grafana, and exporters in a dedicated namespace.
- Use PVCs for persistent storage (Prometheus TSDB, Loki).
- Use Kubernetes Secrets for sensitive configs (DB passwords, Slack webhook).
- Label resources appropriately for metrics collection.
- Use DaemonSets for node-level agents (node_exporter, Promtail).

### Detailed Explanation
#### 🧾 Step 1: Namespace isolation
```sh
kubectl create ns observability
```

#### 🔐 Step 2: Persistent Storage
```yaml
volumeMounts:
- mountPath: /data
  name: prometheus-pvc
```

### 🧠 Step 3: Secrets for sensitive info
```sh
kubectl create secret generic slack-webhook --from-literal=url=<WEBHOOK_URL> -n observability
```

#### ⚖️ Step 4: DaemonSets for node-level agents
- Promtail and node_exporter run as DaemonSet to collect logs and metrics from all nodes.

### Real-world Insight

Deploying observability stack in its own namespace with PVCs and Secrets prevented data loss and ensured security compliance.

### Key takeaway

Proper architecture and best practices are essential for reliable observability in Kubernetes.
