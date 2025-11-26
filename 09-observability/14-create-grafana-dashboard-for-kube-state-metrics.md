## 14. Creating dashboards for kube-state metrics
### Question

How do you monitor Kubernetes object state using kube-state-metrics in Grafana?

### Short explanation

This checks your understanding of monitoring the cluster state beyond node metrics.

### Answer

Kube-state-metrics exposes metrics about Pods, Deployments, StatefulSets, and other objects. Use these metrics to create Grafana dashboards showing object health, pod counts, and resource usage.

### Detailed Explanation
#### 🧾 Step 1: Deploy kube-state-metrics
- helm install kube-state-metrics prometheus-community/kube-state-metrics -n observability

#### 🔐 Step 2: Key metrics
- kube_deployment_status_replicas → number of replicas
- kube_pod_status_phase → pod phase (Running, Pending, Failed)
- kube_pod_container_status_restarts_total → restarts

#### 🧠 Step 3: Create Grafana panels
- Display pod counts by namespace
- Show deployments with unavailable replicas
- Track container restarts

### Real-world Insight

Dashboards showing deployment availability helped us detect failed rollout issues before they affected services.

### Key takeaway

Kube-state-metrics dashboards provide visibility into Kubernetes object health and cluster stability.
