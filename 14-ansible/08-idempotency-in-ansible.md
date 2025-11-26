## 8. Idempotency in Ansible
### Question

What is idempotency and why is it important in Ansible?

### Short explanation

Checks understanding of predictable automation behavior.

### Answer

Idempotency ensures running the same playbook multiple times produces the same result without side effects.

### Detailed explanation
#### 🧾 Example
```yaml
- name: Ensure nginx is installed
  apt:
    name: nginx
    state: present
```
- Running multiple times does not reinstall nginx

#### 🔐 Step 2: Benefits
- Safe repeated executions
- Reduces risk of system inconsistency

### Real-world Insight

Idempotent playbooks allowed safe nightly updates without breaking production servers.

### Key takeaway

Idempotency is crucial for reliable, repeatable automation.
