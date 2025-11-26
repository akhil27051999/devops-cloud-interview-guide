## 2. Difference between Playbooks, Tasks, and Roles
### Question

Explain the difference between Ansible Playbooks, Tasks, and Roles.

### Short explanation

Tests knowledge of Ansible architecture and reusability concepts.

### Answer

- Playbook: Defines a workflow across hosts
- Task: Single action executed on a host
- Role: Collection of tasks, variables, templates, and handlers for reusability

### Detailed explanation
#### 🧾 Step 1: Task Example
```yaml
tasks:
  - name: Install nginx
    apt:
      name: nginx
      state: present
```

#### 🔐 Step 2: Role Example
```sh
roles/
  webserver/
    tasks/main.yml
    templates/nginx.conf.j2
    vars/main.yml
```
- Roles make playbooks modular and reusable

#### 🧠 Step 3: Playbook Using Role
```yaml
- hosts: webservers
  roles:
    - webserver
```
### Real-world Insight

Using roles allowed us to create a standard LAMP stack deployment that could be reused across multiple projects.

### Key takeaway

Roles promote modularity and maintainability, while tasks are the building blocks and playbooks orchestrate workflows.
