## 21. Persistence & storage strategies for Prometheus/Loki
### Question

How do you ensure Prometheus and Loki retain metrics/logs across pod restarts?

### Short explanation

Tests understanding of persistent storage in Kubernetes.

### Answer

Use PersistentVolumeClaims (PVCs) to mount persistent storage to Prometheus and Loki Pods.

### Detailed Explanation
#### 🧾 Step 1: PVC for Prometheus
```yaml
volumeMounts:
- mountPath: /prometheus
  name: prometheus-pvc
```

#### 🔐 Step 2: PVC for Loki
```yaml
volumeMounts:
- mountPath: /loki
  name: loki-pvc
```

#### 🧠 Step 3: Benefits
- Metrics and logs persist even if Pods are deleted or rescheduled.
- Supports long-term analysis and alerting.

### Real-world Insight

After a cluster upgrade, Prometheus Pods were rescheduled, but PVC ensured no historical metrics were lost.

### Key takeaway

Persistent storage is essential for long-term observability and incident investigation.
