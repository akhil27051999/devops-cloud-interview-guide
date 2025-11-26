## 9. Ansible Vault
### Question

What is Ansible Vault and how do you use it?

### Short explanation

Tests knowledge of securely managing secrets in playbooks.

### Answer

Ansible Vault allows you to encrypt sensitive data like passwords, keys, or configuration files in your playbooks, keeping them secure in version control.

### Detailed explanation
#### 🧾 Step 1: Create an encrypted file
```sh
ansible-vault create secrets.yml
```

#### 🔐 Step 2: Use the vault file in playbooks
```sh
vars_files:
  - secrets.yml
```

#### 🧠 Step 3: Edit and decrypt
```sh
ansible-vault edit secrets.yml
ansible-playbook playbook.yml --ask-vault-pass
```
#### Step 4: Benefits
- Secure storage of credentials
- Integrates with CI/CD pipelines securely
- Prevents secrets exposure in Git

### Real-world Insight

We encrypted database passwords and API keys, preventing accidental leaks in GitHub repositories.

### Key takeaway

Vault is essential for secure management of secrets in Ansible playbooks.
