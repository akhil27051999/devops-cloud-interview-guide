## 16. Ansible Modules vs Plugins
### Question

Explain the difference between modules and plugins in Ansible.

### Short explanation

Tests conceptual understanding of Ansible components.

### Answer
- Modules: Actual tasks executed on hosts (package, service, copy)
- Plugins: Extend Ansible functionality (callbacks, connection, inventory)

### Detailed explanation
#### 🧾 Step 1: Module Example
```sh
- name: Install nginx
  apt:
    name: nginx
    state: present
```

#### 🔐 Step 2: Plugin Example
```sh 
# Callback plugin logs output in JSON format
callback_whitelist = json
```

#### 🧠 Step 3: Benefits
- Modules perform actions
- Plugins customize behavior or extend capabilities

### Real-world Insight

Using callback plugins enabled logging playbook runs in JSON format for CI/CD pipelines.

### Key takeaway

Modules execute tasks, plugins extend Ansible functionality, both essential for production automation.
