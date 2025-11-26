## 14. Ansible Facts Caching
### Question

What is fact caching in Ansible and why is it used?

### Short explanation

Tests knowledge of performance optimization in large environments.

### Answer

Fact caching stores collected facts for hosts to avoid repeated gathering, improving playbook performance.

### Detailed explanation
#### 🧾 Step 1: Enable caching
```sh
[defaults]
gathering = smart
fact_caching = jsonfile
fact_caching_connection = /tmp/ansible_cache
```

#### 🔐 Step 2: Benefits
- Reduces playbook runtime for large inventories
- Useful for repeated playbook executions

### Real-world Insight

Enabling fact caching reduced playbook runtime from 30 minutes to 10 minutes across 200 servers.

### Key takeaway

Fact caching improves performance for large-scale automation.
