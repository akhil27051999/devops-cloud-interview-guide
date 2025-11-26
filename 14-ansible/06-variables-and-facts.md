## 6. Variables and Facts
### Question

What are variables and facts in Ansible?

### Short explanation

Tests knowledge of dynamic configuration and system information collection.

### Answer

- Variables: User-defined values for playbooks, roles, and tasks
- Facts: Auto-collected system information like OS, IP, CPU

### Detailed explanation
#### 🧾 Step 1: Variables example
```sh
vars:
  web_port: 80

tasks:
  - name: Install nginx
    apt:
      name: nginx
      state: present
```

#### 🔐 Step 2: Facts usage
```sh
- debug:
    msg: "This server has {{ ansible_processor_cores }} CPU cores"
```
#### 🧠 Step 3: Benefits
- Variables: parameterize playbooks for flexibility
- Facts: dynamic decisions based on host properties

### Real-world Insight

Using facts, we installed packages conditionally based on OS type (Debian vs RedHat).

### Key takeaway

Variables parameterize, facts inform dynamic decision-making in Ansible automation.
