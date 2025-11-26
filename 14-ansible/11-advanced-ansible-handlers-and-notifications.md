## 11. Ansible Handlers and Notifications (Advanced)
### Question

How do handlers improve playbook efficiency and minimize service restarts?

### Short explanation

Tests advanced use of handlers in production.

### Answer

Handlers execute only when notified by other tasks, ensuring services are restarted only if configuration changes, reducing downtime.

### Detailed explanation
#### 🧾 Step 1: Notify a handler
```sh
tasks:
  - name: Update nginx config
    template:
      src: nginx.conf.j2
      dest: /etc/nginx/nginx.conf
    notify: Restart nginx
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
- Prevents repeated service restarts
- Ensures idempotent and efficient playbook execution

### Real-world Insight

Using handlers for all web services reduced downtime during mass configuration updates from 20 minutes to 2 minutes.

### Key takeaway

Handlers are crucial for minimizing disruption during configuration changes.
