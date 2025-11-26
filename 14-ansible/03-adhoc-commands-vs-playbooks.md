## 3. Ad-hoc Commands vs Playbooks
### Question

When should you use ad-hoc commands vs playbooks in Ansible?

### Short explanation

Tests your understanding of quick actions vs full automation workflows.

### Answer

- Ad-hoc commands: Quick one-off commands (e.g., ping, package install)
- Playbooks: Defined workflows for repeatable, multi-step automation

### Detailed explanation
#### 🧾 Step 1: Ad-hoc Example
```sh
ansible all -m ping
ansible webservers -m apt -a "name=nginx state=present" -b
```

#### 🔐 Step 2: Playbook Example
```yaml
- name: Setup webserver
  hosts: webservers
  become: yes
  tasks:
    - name: Install nginx
      apt:
        name: nginx
        state: present
```

#### 🧠 Step 3: When to use
- Ad-hoc: quick fixes, testing connectivity
- Playbook: production-ready automation, CI/CD integration

### Real-world Insight

We used ad-hoc commands for debugging connectivity, but production deployments were always done via playbooks.

### Key takeaway

Ad-hoc for testing, playbooks for repeatable, maintainable automation.
