## 4. Ansible Inventory Types
### Question

Explain static and dynamic inventory in Ansible.

### Short explanation

Tests your ability to manage host definitions efficiently.

### Answer

- Static inventory: Hosts defined manually in a file
- Dynamic inventory: Automatically fetched from cloud providers or scripts

### Detailed explanation
#### 🧾 Step 1: Static Inventory Example
```sh
[webservers]
web1.example.com
web2.example.com

[dbservers]
db1.example.com
```

#### 🔐 Step 2: Dynamic Inventory
- Use cloud plugins like AWS EC2, Azure, GCP
```sh
ansible-inventory -i aws_ec2.yaml --list
```

#### 🧠 Step 3: Benefits
- Static: Simple, easy for small environments
- Dynamic: Scales for cloud environments and auto-updating hosts

### Real-world Insight

Dynamic inventory with AWS EC2 plugin allowed automatic addition of new instances without manually updating inventory files.

### Key takeaway

Dynamic inventory is essential for large-scale, cloud-based deployments.
