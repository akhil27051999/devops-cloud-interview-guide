## 15. Secret Exposure in State Files
### Question

How do you protect sensitive data in Terraform?

### Short explanation

Tests knowledge of handling sensitive information in IaC.

### Answer

- Use sensitive = true in outputs
- Store secrets in environment variables or Vault
- Avoid hardcoding secrets in .tf files

### Detailed explanation
#### 🧾 Step 1: Mark outputs sensitive
```yaml
output "db_password" {
  value     = aws_db_instance.prod.password
  sensitive = true
}
```

#### 🔐 Step 2: Use environment variables
- export TF_VAR_db_password="mysecretpassword"

#### 🧠 Step 3: Use Terraform Vault provider
- Integrates with HashiCorp Vault to fetch secrets dynamically

### Real-world Insight

Marking RDS passwords as sensitive prevented them from appearing in logs or terminal output.

### Key takeaway

Never store secrets in plaintext in Terraform; always use secure methods and mark sensitive outputs.
