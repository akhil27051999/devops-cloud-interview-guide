## 13. Conditional Execution
### Question

How do you execute tasks conditionally in Ansible?

### Short explanation

Tests understanding of dynamic task execution based on variables or facts.

### Answer

Use when statements to conditionally execute tasks.

### Detailed explanation
#### 🧾 Step 1: Conditional task example
```sh
tasks:
  - name: Install nginx only on Ubuntu
    apt:
      name: nginx
      state: present
    when: ansible_distribution == "Ubuntu"
```

#### 🔐 Step 2: Benefits
- Tasks executed only when criteria are met
- Reduces errors and unnecessary actions

### Real-world Insight
Conditional execution allowed us to maintain a single playbook for both Ubuntu and CentOS servers.

### Key takeaway

Conditional execution ensures tasks run only when required, improving efficiency and reducing errors.
