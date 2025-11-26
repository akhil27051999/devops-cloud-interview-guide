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
