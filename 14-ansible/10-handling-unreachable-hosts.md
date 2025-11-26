## 10. Handling Unreachable Hosts
### Question

How do you handle unreachable hosts in Ansible playbooks?

### Short explanation

Tests knowledge of error handling and retries.

### Answer

Use retry mechanisms, ignore_errors, or serial execution in playbooks to handle temporary network or SSH failures.

### Detailed explanation
#### 🧾 Step 1: Example with retries
```sh
tasks:
  - name: Ping host
    ping:
    retries: 3
    delay: 5
```

#### 🔐 Step 2: Serial execution
```sh
- hosts: webservers
  serial: 5
```
- Executes playbook in batches to isolate failures

#### 🧠 Step 3: Ignore errors when necessary
```sh
tasks:
  - name: Optional task
    command: /bin/false
    ignore_errors: yes
```

### Real-world Insight

Using serial execution prevented an entire playbook from failing when a few hosts were temporarily unreachable.

### Key takeaway

Error handling and retry mechanisms improve playbook reliability in production environments.
