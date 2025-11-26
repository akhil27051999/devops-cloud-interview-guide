## 7. Templates in Ansible (Jinja2)
### Question

How do you use Jinja2 templates in Ansible?

### Short explanation

Tests ability to generate dynamic configuration files per host.

### Answer

Templates allow rendering files dynamically using variables, loops, and conditionals.

### Detailed explanation
#### 🧾 Step 1: Template example
- nginx.conf.j2:
```sh
server {
    listen {{ web_port }};
    server_name {{ ansible_fqdn }};
}
```
#### 🔐 Step 2: Apply template in playbook
```yaml
- name: Configure nginx
  template:
    src: nginx.conf.j2
    dest: /etc/nginx/nginx.conf
```

#### 🧠 Step 3: Benefits
- Host-specific configs
- Reusable templates across environments

### Real-world Insight

Using templates, we deployed host-specific Apache virtual hosts across 100 servers automatically.

### Key takeaway

Templates simplify dynamic, environment-specific configuration generation.
