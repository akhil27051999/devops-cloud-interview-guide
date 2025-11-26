## 18. Performance Optimization in Ansible
### Question

How do you improve Ansible performance for large environments?

### Short explanation

Tests ability to optimize playbooks for speed and efficiency.

### Answer
- Use fact caching
- Limit hosts with -l
- Use async and poll for long-running tasks
- Run tasks in parallel (forks)

### Detailed explanation
#### 🧾 Step 1: Parallel execution
```sh
ansible-playbook site.yml -f 50
```

#### 🔐 Step 2: Async tasks
```yaml
- name: Run long task
  shell: /usr/bin/long_task.sh
  async: 600
  poll: 0
```
#### 🧠 Step 3: Limit hosts
```sh
ansible-playbook site.yml -l webservers
```

### Real-world Insight

Using forks=50 and async for long-running tasks reduced deployment time from 3 hours to 45 minutes.

### Key takeaway

Parallelism, async tasks, and fact caching improve large-scale playbook performance.
