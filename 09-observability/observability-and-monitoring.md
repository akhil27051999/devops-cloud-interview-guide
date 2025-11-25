# Observability Stack Interview Prep

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

## 3. How Promtail sends application logs to Loki
### Question

How does Promtail collect logs from applications and send them to Loki?

### Short explanation

Tests your knowledge of log aggregation and filtering using Loki stack.

### Answer

Promtail runs as an agent on nodes, reads log files, labels them, and pushes to Loki via HTTP. Only configured logs (e.g., application logs) are sent.

### Detailed Explanation
#### 🧾 Step 1: Deploy Promtail
helm install promtail grafana/promtail -n observability

#### 🔐 Step 2: Configure scrape_configs
```yaml
scrape_configs:
  - job_name: system
    static_configs:
      - targets:
          - localhost
        labels:
          job: myapp
          __path__: /var/log/myapp/*.log
```

- __path__ defines which logs to send.
- labels tag logs for querying in Grafana.

#### 🧠 Step 3: Logs reach Loki
- Loki stores logs as streams with labels.
- Promtail can filter out system logs and send only application logs.

#### ⚖️ Step 4: Query logs in Grafana
```yaml
{job="myapp"} |= "ERROR"
```

### Real-world Insight

Filtering only app logs avoided unnecessary volume of system logs and reduced storage costs in Loki.

### Key takeaway

Proper labeling and filtering in Promtail ensures efficient log collection and querying in Loki.

## 4. How Prometheus monitors endpoints using Blackbox Exporter
### Question

How does Prometheus monitor endpoint uptime and latency using the Blackbox Exporter?

### Short explanation

This checks if you understand active probing of services and endpoints for observability.

### Answer

Prometheus uses the Blackbox Exporter to actively probe endpoints (HTTP, HTTPS, ICMP) and expose results (success, latency) at a /probe endpoint, which Prometheus scrapes.

### Detailed Explanation
#### 🧾 Step 1: Deploy Blackbox Exporter
- helm install blackbox-exporter prometheus-community/blackbox-exporter -n observability

- Runs as Deployment/Service exposing /probe endpoint.

#### 🔐 Step 2: Configure Prometheus scrape
```yaml
scrape_configs:
  - job_name: 'blackbox'
    metrics_path: /probe
    params:
      module: [http_2xx]
    static_configs:
      - targets:
        - https://argocd.example.com
        - https://vault.example.com

```
- module defines probe type (HTTP, TCP, ICMP)
- targets are endpoints to monitor

#### 🧠 Step 3: Metrics collected
- probe_success → 1 if probe succeeded, 0 if failed
- probe_duration_seconds → Latency of endpoint
- probe_http_status_code → HTTP status code

#### ⚖️ Step 4: Alerts
- Example alert for downtime:
```yaml
- alert: EndpointDown
  expr: probe_success == 0
  for: 1m
  labels:
    severity: critical
  annotations:
    summary: "Endpoint is down"
```

### Real-world Insight

Monitoring Vault and ArgoCD endpoints using Blackbox exporter helped detect downtime caused by network misconfigurations before users noticed it.

### Key takeaway

Blackbox exporter allows active monitoring of endpoints, enabling proactive detection of service downtime or latency spikes.

## 5. How to create Grafana dashboards for DB metrics
### Question

How do you visualize database metrics in Grafana?

### Short explanation

Tests your understanding of dashboards, Prometheus queries, and visualization for observability.

### Answer

Create a dashboard in Grafana, add Prometheus as a data source, and use PromQL queries to visualize key DB metrics.

### Detailed Explanation
#### 🧾 Step 1: Add Prometheus data source
- Grafana → Configuration → Data Sources → Add Prometheus
- URL: http://prometheus.observability.svc.cluster.local:9090

#### 🔐 Step 2: Create Dashboard Panels
- Panel 1: DB up status
```sh
pg_up{job="postgres"}
```

- Panel 2: Active connections
```sh
pg_stat_activity_count{job="postgres"}
```

- Panel 3: Cache hit ratio
```sh
(pg_cache_hit_ratio{job="postgres"})
```

#### 🧠 Step 3: Customize visualization

- Use line graphs for trends
- Threshold colors for alerts (red > 80% usage)
- Group related metrics in a single row

### Real-world Insight

We created DB dashboards to monitor connection saturation and query latency. It helped us detect spikes during batch jobs and optimize queries.

### Key takeaway

Grafana dashboards turn raw metrics into actionable visualizations, simplifying observability.

## 6. How to create Grafana dashboards for application error logs
### Question

How do you monitor application errors using Grafana and Loki?

### Short explanation

This evaluates your ability to correlate logs with metrics for application observability.

### Answer

Use Loki as a data source in Grafana and query structured application logs. Apply labels for filtering (e.g., job="myapp").

### Detailed Explanation
#### 🧾 Step 1: Add Loki as data source

- Grafana → Data Sources → Add Loki
- URL: http://loki.observability.svc.cluster.local:3100

#### 🔐 Step 2: Create log panels
- Query for errors:
```sh
{job="myapp"} |= "ERROR"
```
- Filter by severity or request path using labels.

#### 🧠 Step 3: Visualize logs
- Use table or logs panel to show recent errors.
- Combine with Prometheus metrics for request rates, latency.

### Real-world Insight

By combining Loki error logs with Prometheus latency metrics, we quickly pinpointed slow endpoints causing error spikes.

### Key takeaway

Structured logs in Loki + Grafana visualization provide deep insights into application health.

## 7. Alerting for CPU & memory utilization
### Question

How do you configure alerts in Prometheus/Grafana for high CPU or memory usage?

### Short explanation

Tests ability to set up threshold-based alerts for system resource utilization.

### Answer

Define alert rules in Prometheus or Grafana based on node metrics. Alerts can be routed via Alertmanager to Slack.

### Detailed Explanation
#### 🧾 Step 1: Prometheus alert rule
```yaml
groups:
- name: NodeAlerts
  rules:
  - alert: HighCPU
    expr: 100 - avg(irate(node_cpu_seconds_total{mode="idle"}[5m])) * 100 > 90
    for: 5m
    labels:
      severity: critical
    annotations:
      summary: "CPU usage high on node"
```
#### 🔐 Step 2: Alertmanager config
```yaml
receivers:
- name: slack-notifications
  slack_configs:
  - channel: "#alerts"
    webhook_url: "<SLACK_WEBHOOK_URL>"
```
#### 🧠 Step 3: Visual verification

- Grafana panel shows CPU usage trend.
- Alerts trigger Slack messages when thresholds are exceeded.

### Real-world Insight

Alerts helped detect runaway processes that caused CPU spikes, enabling automated Slack notifications and faster response.

### Key takeaway

Threshold-based alerts allow teams to proactively address resource issues before impacting services.

## 8. Alerting for high error rate in the application
### Question

How do you configure alerts for spikes in HTTP 5xx errors in a REST API?

### Short explanation

This tests understanding of RED metrics (Rate, Errors, Duration) and alerting on application errors.

### Answer

Use Prometheus to query http_requests_total metric with a filter for 5xx status codes. Trigger an alert if the error count exceeds a threshold over a given time window.

### Detailed Explanation
#### 🧾 Step 1: Define Prometheus alert
```yaml
groups:
- name: AppAlerts
  rules:
  - alert: HighErrorRate
    expr: increase(http_requests_total{status=~"5.."}[10m]) > 10
    for: 5m
    labels:
      severity: critical
    annotations:
      summary: "Spike in 5xx errors"
      description: "More than 10 errors in last 10 minutes"
```
#### 🔐 Step 2: Route alert via Alertmanager
```yaml
receivers:
- name: slack-alerts
  slack_configs:
  - channel: "#alerts"
    webhook_url: "<SLACK_WEBHOOK_URL>"
```
#### 🧠 Step 3: Verification

- Grafana dashboard shows http_requests_total with error breakdown.
- Slack receives a descriptive alert if threshold is breached.

### Real-world Insight

We detected a new deployment causing intermittent 5xx errors and fixed a misconfigured API route by correlating alert notifications with logs.

### Key takeaway

RED metrics (Rate, Errors, Duration) enable effective monitoring and alerting for service reliability.

## 9. Alerting for latency spikes (p90, p95, p99)
### Question

How do you monitor and alert for high latency in APIs?

### Short explanation

Evaluates understanding of latency metrics and Prometheus histogram queries.

### Answer

Use Prometheus histogram metrics (e.g., http_request_duration_seconds_bucket) and histogram_quantile() to calculate p90, p95, p99 latencies. Trigger alerts when thresholds exceed limits.

### Detailed Explanation
#### 🧾 Step 1: Example PromQL query
```sh
histogram_quantile(0.95, sum(rate(http_request_duration_seconds_bucket[5m])) by (le)) > 0.5
```
- Calculates the 95th percentile of request duration over 5 minutes.

#### 🔐 Step 2: Define alert rule
```yaml
- alert: HighLatency
  expr: histogram_quantile(0.95, sum(rate(http_request_duration_seconds_bucket[5m])) by (le)) > 0.5
  for: 5m
  labels:
    severity: warning
  annotations:
    summary: "High latency detected"
```

#### 🧠 Step 3: Dashboard visualization
- Grafana panel for p90, p95, p99 latency over time.
- Can overlay request rate for correlation.

### Real-world Insight

Latency alerts helped identify network congestion between microservices, enabling early mitigation.

### Key takeaway

Percentile-based latency monitoring ensures users experience consistent performance even under load spikes.

## 10. Alerting on DB, Vault, and ArgoCD restarts
### Question

How do you detect and alert if critical services restart unexpectedly?

### Short explanation

Tests monitoring of pod/container lifecycle events using kube-state-metrics.

### Answer

Monitor kube_pod_container_status_restarts_total metric from kube-state-metrics. Alert if the restart count increases within a time window.

### Detailed Explanation
#### 🧾 Step 1: Prometheus alert rule
```yaml
- alert: ServiceRestart
  expr: increase(kube_pod_container_status_restarts_total{namespace="default", pod=~"vault|argocd|postgres"}[5m]) > 0
  for: 1m
  labels:
    severity: critical
  annotations:
    summary: "Critical service restarted"
    description: "Vault, ArgoCD, or DB pod restarted in last 5 minutes"
```
#### 🔐 Step 2: Route alerts to Slack
- Same as other alerts using Alertmanager webhook.

#### 🧠 Step 3: Verification
- Grafana dashboards for pod restarts.
- Slack receives descriptive alert message.

### Real-world Insight

Detected unplanned ArgoCD restarts after cluster upgrade. Early alerting reduced downtime and accelerated root cause analysis.

### Key takeaway

Monitoring container restarts provides immediate insight into stability of critical services.

## 11. Understanding RED and USE metrics
### Question

What are RED and USE metrics, and how do they help observability?

### Short explanation

Tests your understanding of key observability principles for services and systems.

### Answer

- RED metrics (Rate, Errors, Duration) → Used for service-level monitoring.
- USE metrics (Utilization, Saturation, Errors) → Used for system/resource monitoring.
- They help identify bottlenecks, failures, and performance issues.

### Detailed Explanation
#### 🧾 Step 1: RED Metrics Example
- Rate: http_requests_total → requests per second
- Errors: http_requests_total{status=~"5.."}
- Duration: http_request_duration_seconds

#### 🧠 Step 2: USE Metrics Example
- Utilization: node_cpu_seconds_total
- Saturation: node_load1 → system queue depth
- Errors: node_filesystem_errors_total

#### ⚖️ Step 3: Application
- Combine metrics in Grafana for dashboards.
- Create alerts on thresholds to maintain service reliability.

### Real-world Insight

Using RED/USE metrics allowed us to differentiate between service issues (API latency) and infrastructure issues (node overload).

### Key takeaway

RED/USE metrics are foundational observability pillars for proactive monitoring.

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

## 15. Creating dashboards for Blackbox metrics
### Question

How do you visualize endpoint availability and latency using Grafana?

### Short explanation

Checks understanding of combining Prometheus and Blackbox Exporter metrics for external monitoring.

### Answer

Use metrics like probe_success and probe_duration_seconds from Blackbox Exporter. Grafana panels can show uptime and latency for critical services.

### Detailed Explanation
#### 🧾 Step 1: Create panels
- Endpoint uptime:
```sh
probe_success{job="blackbox"}
```
- Latency:
```sh
probe_duration_seconds{job="blackbox"}
```

#### 🔐 Step 2: Visualization
- Use gauges for success/failure.
- Use line graphs for latency trends.

### Real-world Insight

Visualizing latency trends allowed early detection of network congestion affecting API performance.

### Key takeaway

Blackbox dashboards complement internal metrics by providing an external view of service health.

## 16. Alerting for request rate spikes
### Question

How do you alert if the number of requests suddenly increases?

### Short explanation

Evaluates monitoring traffic patterns and detecting abnormal load.

### Answer

Use Prometheus metric http_requests_total. Trigger an alert if request rate exceeds a threshold over a given time window.

### Detailed Explanation
#### 🧾 Step 1: Define Prometheus alert
```yaml
- alert: HighRequestRate
  expr: increase(http_requests_total[5m]) > 1000
  for: 5m
  labels:
    severity: warning
  annotations:
    summary: "High request rate detected"
```

#### 🔐 Step 2: Notification
- Send alert via Alertmanager to Slack or email.

#### 🧠 Step 3: Verification
- Grafana shows request trends, correlate with latency and error metrics.

### Real-world Insight

Early detection of traffic spikes helped prevent server overload and auto-scale pods to maintain SLAs.

### Key takeaway

Monitoring request rate is critical for proactive scaling and preventing service degradation.

## 17. Combining logs and metrics for root cause analysis
### Question

How do you correlate application logs and Prometheus metrics for troubleshooting?

### Short explanation

Tests your ability to combine observability pillars for root cause analysis.

### Answer

Use Grafana to display metrics (Prometheus) and logs (Loki) side by side. Use labels (like pod, request_id) to correlate spikes in metrics with log entries.

### Detailed Explanation
#### 🧾 Step 1: Ensure consistent labeling

- Promtail should add labels like job="myapp", pod="myapp-xyz"

#### 🔐 Step 2: Create dashboard with mixed panels
- Metrics panel: request latency or error rate
- Logs panel: |= "ERROR" filtered by pod

#### 🧠 Step 3: Investigate issues
- Identify spikes in latency or errors
- Correlate timestamps with log entries for root cause

### Real-world Insight

Correlating high latency with application exceptions in logs helped quickly fix a misconfigured external API call.

### Key takeaway

Combining logs and metrics provides end-to-end observability, enabling faster issue resolution.

## 18. Promtail log filtering strategies
### Question

How do you ensure Promtail only sends relevant logs to Loki?

### Short explanation

Checks understanding of efficient log collection and labeling for observability.

### Answer

Promtail uses scrape_configs and pipeline_stages to filter logs based on file paths, labels, or content patterns. Only application logs are sent, ignoring system logs.

### Detailed Explanation
#### 🧾 Step 1: Filter by log path
```yaml
scrape_configs:
- job_name: myapp-logs
  static_configs:
  - targets:
      - localhost
    labels:
      job: myapp
      __path__: /var/log/myapp/*.log
```

#### 🔐 Step 2: Filter by log content
```yaml
pipeline_stages:
- match:
    selector: '{job="myapp"}'
    stages:
    - regex:
        expression: "ERROR|WARN"
```

#### 🧠 Step 3: Benefits
- Reduces volume of logs in Loki
- Focuses on actionable logs (errors, warnings)
- Improves query performance in Grafana

### Real-world Insight

Filtering only error logs reduced Loki storage usage by 70% while keeping all critical information for troubleshooting.

### Key takeaway

Proper filtering in Promtail ensures efficient and meaningful log collection.

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

## 20. Alerting thresholds best practices
### Question

How do you choose thresholds for alerts?

### Short explanation

Evaluates understanding of realistic, actionable alerting to avoid noise.

### Answer

- Use historical metric trends to define thresholds.
- Alert on sustained deviations, not short spikes.
- Combine metrics for contextual alerts (e.g., high CPU + high latency).

### Detailed Explanation
#### 🧾 Step 1: Avoid alert fatigue
- Example: CPU > 90% sustained for 5 mins instead of immediate spike.

#### 🔐 Step 2: Use relative thresholds
- Compare to baseline average: avg_over_time(node_cpu_seconds_total[1h]) * 1.5

#### 🧠 Step 3: Combine metrics for reliability
- Alert on high latency AND high error rate instead of either alone.

### Real-world Insight

Using only CPU thresholds caused multiple false alerts. Combining CPU + request latency reduced alert noise by 60%.

### Key takeaway

Well-chosen thresholds ensure actionable and reliable alerts.

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

## 22. Kubernetes observability edge case: CrashLoopBackOff
### Question

How do you detect and alert on pods in CrashLoopBackOff?

### Short explanation

Tests ability to monitor Kubernetes pod health.

### Answer

Use kube_pod_container_status_restarts_total metric from kube-state-metrics. Alert if the restart count increases rapidly.

### Detailed Explanation
#### 🧾 Step 1: Prometheus alert
```yaml
- alert: PodCrashLoop
  expr: increase(kube_pod_container_status_restarts_total[5m]) > 3
  for: 5m
  labels:
    severity: critical
```
#### 🔐 Step 2: Dashboard visualization
- Grafana panel for restart trends by namespace and pod.

### Real-world Insight

Detected a CrashLoopBackOff in a DB pod due to misconfigured secrets. Alerting enabled immediate remediation.

### Key takeaway

Monitoring container restarts helps detect unstable deployments and misconfigurations quickly.

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

## 24. Monitoring HashiCorp Vault health
### Question

How do you monitor Vault uptime and availability?

### Short explanation

Tests monitoring of critical services with external endpoints and metrics.

### Answer

Use Blackbox Exporter to probe Vault’s HTTP endpoint and Vault exporter for internal metrics. Alert on probe_success=0 or Vault unsealed status.

### Detailed Explanation
#### 🧾 Step 1: Metrics
- vault_seal_status → 0 if unsealed
- probe_success → 1 if endpoint is reachable

#### 🔐 Step 2: Alert
```yaml
- alert: VaultDown
  expr: probe_success{job="blackbox", instance="vault.example.com"} == 0
  for: 1m
```

### Real-world Insight

Alerts detected Vault unsealing failure during maintenance, enabling immediate manual intervention.

### Key takeaway

Critical services require both internal metrics and external probes for robust observability.

## 25. Monitoring ArgoCD server health
### Question

How do you monitor the health and uptime of the ArgoCD server?

### Short explanation

Checks understanding of monitoring a critical deployment with metrics and external probes.

### Answer

Use Blackbox Exporter for uptime, and Prometheus metrics from ArgoCD server. Alert if probe_success is 0 or API latency exceeds threshold.

### Detailed Explanation
#### 🧾 Step 1: Blackbox probe for ArgoCD
```yaml
static_configs:
  - targets:
      - https://argocd.example.com
params:
  module: [http_2xx]
```

#### 🔐 Step 2: Prometheus alert for uptime
```yaml
- alert: ArgoCDDown
  expr: probe_success{job="blackbox", instance="argocd.example.com"} == 0
  for: 1m
```

#### 🧠 Step 3: Prometheus alert for latency
```yaml
- alert: ArgoCDHighLatency
  expr: probe_duration_seconds{job="blackbox", instance="argocd.example.com"} > 0.5
  for: 5m
```

### Real-world Insight

Alerts on ArgoCD downtime allowed the team to react to a failed automated deployment before users were affected.

### Key takeaway

Combining uptime and latency monitoring provides a complete view of service health.

## 26. REST API latency & error dashboards
### Question

How do you monitor a REST API’s latency and errors in Grafana?

### Short explanation

Tests knowledge of combining RED metrics (Rate, Errors, Duration) for service observability.

### Answer

Use Prometheus metrics exposed by the API, such as http_requests_total and http_request_duration_seconds. Create Grafana panels for p90/p95/p99 latency and 5xx error rate.

### Detailed Explanation
#### 🧾 Step 1: Prometheus queries

- Errors:
  increase(http_requests_total{status=~"5.."}[10m])

- p95 latency:
  histogram_quantile(0.95, sum(rate(http_request_duration_seconds_bucket[5m])) by (le))

#### 🔐 Step 2: Grafana panels
- Line charts for latency over time
- Table for error counts per endpoint
- Use alerts for spikes in latency or errors

### Real-world Insight

Dashboards allowed us to detect a sudden spike in errors after a deployment, correlating with p95 latency and fixing the faulty endpoint.

### Key takeaway

Dashboards combining error rate and latency are essential for monitoring API health.

## 27. Combined RED/USE metrics dashboards
### Question

How do you combine RED (service) and USE (system) metrics for observability?

### Short explanation

Evaluates understanding of integrated monitoring.

### Answer

Create Grafana dashboards combining Prometheus metrics for service performance (RED) and system resources (USE). This helps identify whether issues are application-related or infrastructure-related.

### Detailed Explanation
#### 🧾 Step 1: RED metrics
- Rate: http_requests_total
- Errors: http_requests_total{status=~"5.."}
- Duration: http_request_duration_seconds

#### 🔐 Step 2: USE metrics
- CPU: node_cpu_seconds_total
- Memory: node_memory_MemAvailable_bytes
- Disk: node_filesystem_avail_bytes

#### 🧠 Step 3: Dashboard layout

- Rows for service metrics and system metrics
- Overlay resource usage with request rates and latency

### Real-world Insight

Correlating high API latency with high node CPU usage helped us identify that slow endpoints were caused by resource contention, not application code.

### Key takeaway

Integrated dashboards allow faster root cause analysis by combining system and application metrics.

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

## 30. Observability stack CI/CD and documentation
### Question

How do you deploy observability stack (Prometheus, Grafana, Loki) using CI/CD with Helm and document setup?

### Short explanation

Tests deployment automation and reproducibility skills.

### Answer

- Use Helm charts for Prometheus, Grafana, Loki, Promtail, and exporters.
- Automate deployment using GitHub Actions or Jenkins.
- Maintain README.md with instructions for installing and configuring the stack in Kubernetes.

### Detailed Explanation
#### 🧾 Step 1: Helm deployment example
```sh
helm install prometheus prometheus-community/prometheus -n observability
helm install grafana grafana/grafana -n observability
helm install loki grafana/loki -n observability
helm install promtail grafana/promtail -n observability
```

#### 🔐 Step 2: CI/CD automation
- GitHub Actions pipeline:
  - Checkout code
  - Run helm lint
  - Deploy to cluster using helm upgrade --install

#### 🧠 Step 3: Documentation
- README.md includes:
  - Namespace creation
  - PVC instructions
  - Secrets setup (Slack webhook)
  - Helm commands
  - Dashboard import instructions

### Real-world Insight

Using Helm charts and CI/CD allowed quick reproducible deployments in multiple clusters, reducing manual errors.

### Key takeaway

Automating deployment and documenting setup ensures reliable, repeatable observability stack setup across environments.


