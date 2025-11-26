## 5. Handlers and Notifications
### Question

What are Ansible handlers and how do they work?

### Short explanation

Tests understanding of conditional actions triggered by changes.

### Answer

Handlers are tasks that execute only when notified by other tasks (usually to restart a service after configuration changes).

### Detailed explanation
#### 🧾 Step 1: Task notifying a handler
```sh
tasks:
  - name: Update nginx config
    template:
      src: nginx.conf.j2
      dest: /etc/nginx/nginx.conf
    notify:
      - Restart nginx
```

#### 🔐 Step 2: Define the handler
```sh
handlers:
  - name: Restart nginx
    service:
      name: nginx
      state: restarted
```

#### 🧠 Step 3: Benefits
- Avoid unnecessary service restarts
- Ensures changes trigger required actions only once

### Real-world Insight

Without handlers, nginx was restarted after every task, causing downtime. Handlers ensured minimal disruption.

### Key takeaway

Handlers enable efficient, conditional task execution in Ansible.
