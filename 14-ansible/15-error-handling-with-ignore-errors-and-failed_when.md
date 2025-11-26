## 15. Error Handling with ignore_errors and failed_when
### Question

How do you handle errors in Ansible playbooks?

### Short explanation

Tests production-grade error handling skills.

### Answer

Use ignore_errors: yes to continue playbook execution or failed_when to define custom failure conditions.

### Detailed explanation
#### 🧾 Step 1: Ignore errors
```sh
tasks:
  - command: /bin/false
    ignore_errors: yes
```

#### 🔐 Step 2: Custom failure
```sh
tasks:
  - shell: "test -f /etc/nginx/nginx.conf"
    failed_when: false
```

#### 🧠 Step 3: Benefits
- Allows playbook to continue after minor issues
- Custom failures prevent false negatives
 
### Real-world Insight

Ignoring errors for optional tasks prevented unnecessary playbook failures during updates.

### Key takeaway

Error handling improves playbook resilience in production environments.
