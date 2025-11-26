## 17. Rolling Updates with serial and max_fail_percentage
### Question

How do you perform rolling updates safely in Ansible?

### Short explanation

Tests orchestration skills in production environments.

### Answer

Use serial to apply playbooks in batches and max_fail_percentage to tolerate limited failures.

### Detailed explanation
#### 🧾 Step 1: Serial execution
```sh
- hosts: webservers
  serial: 5
  max_fail_percentage: 20
  tasks:
    - name: Update Nginx
      apt:
        name: nginx
        state: latest
```

#### 🔐 Step 2: Benefits
- Prevents full outage
- Allows controlled deployment

### Real-world Insight

Rolling updates with serial allowed updating 100 web servers with zero downtime.

### Key takeaway

Serial execution and failure tolerance enable safe production deployments.
