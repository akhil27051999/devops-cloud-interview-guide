## 19. Idempotent Custom Modules
### Question

How do you write an idempotent custom Ansible module?

### Short explanation

Tests ability to extend Ansible safely for repeated execution.

### Answer

- Implement check_mode and return changed: true/false based on actual changes
- Use module.exit_json(changed=...)

### Detailed explanation
#### 🧾 Step 1: Custom module example
```sh
#!/usr/bin/python
from ansible.module_utils.basic import AnsibleModule

def main():
    module = AnsibleModule(argument_spec={})
    changed = False
    # Logic to determine change
    module.exit_json(changed=changed)

if __name__ == '__main__':
    main()
```

#### 🔐 Step 2: Benefits

- Safe repeated execution
- Ensures predictable playbook behavior

### Real-world Insight

Custom module to configure firewall rules safely ran multiple times without creating duplicate rules.

### Key takeaway

Idempotent modules maintain safe, predictable automation, critical in production.
