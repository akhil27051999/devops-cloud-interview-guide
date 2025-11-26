## 12. Ansible Loops
### Question

How do you use loops in Ansible to perform repetitive tasks?

### Short explanation

Tests ability to automate multiple resources efficiently.

### Answer

Use loop or with_items to iterate over lists or dictionaries to execute tasks multiple times dynamically.

### Detailed explanation
#### 🧾 Step 1: Loop Example
```yaml
tasks:
  - name: Install multiple packages
    apt:
      name: "{{ item }}"
      state: present
    loop:
      - nginx
      - git
      - curl
```

#### 🔐 Step 2: Loop with dictionary
```sh
tasks:
  - name: Create users
    user:
      name: "{{ item.name }}"
      state: present
    loop:
      - { name: 'user1' }
      - { name: 'user2' }
```

#### 🧠 Step 3: Benefits
- Reduces repetitive task definitions
- Supports dynamic configurations

### Real-world Insight

Looping over 50 package installations avoided writing 50 tasks, making playbooks more maintainable.

### Key takeaway

Loops simplify repetitive automation tasks and improve playbook readability.
