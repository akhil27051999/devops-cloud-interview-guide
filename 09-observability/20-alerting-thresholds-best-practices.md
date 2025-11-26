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
