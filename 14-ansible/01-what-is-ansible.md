## 1. What is Ansible and why is it used?

### Question
What is Ansible, and why is it widely used in DevOps and IT automation?

### Short explanation

Checks your understanding of Ansible as an agentless automation tool for configuration management, orchestration, and provisioning.

### Answer

Ansible is an agentless configuration management and automation tool that uses YAML playbooks to define infrastructure, application deployments, and operational workflows. It connects to servers via SSH and does not require any agent installation.

### Detailed explanation
#### 🧾 Step 1: How Ansible Works
- Control node: Where playbooks are executed
- Managed nodes: Servers being configured
- Connects via SSH (Linux) or WinRM (Windows)

#### 🔐 Step 2: Key Components
- Inventory: List of hosts
- Modules: Pre-built tasks (package, service, copy, etc.)
- Playbooks: YAML files defining workflows
- Roles: Reusable collection of tasks, templates, variables

#### 🧠 Step 3: Example Playbook
```yaml
- name: Install Nginx
  hosts: webservers
  become: yes
  tasks:
    - name: Install nginx package
      apt:
        name: nginx
        state: present
```
### Real-world Insight

We automated Nginx deployment across 50 servers without manually logging into each server, reducing deployment time from hours to minutes.

### Key takeaway

Ansible simplifies repeatable configuration management and automation without installing agents on servers.
