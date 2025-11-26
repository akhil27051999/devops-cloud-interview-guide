## 6. Explain Terraform Workspaces
### Question

What are Terraform workspaces and how do they help in managing multiple environments?

### Short explanation

Tests your understanding of environment separation using a single configuration.

### Answer

Terraform workspaces allow multiple instances of the same configuration to maintain separate state files. This is useful for managing dev, staging, and production environments without duplicating code.

### Detailed explanation
#### 🧾 Step 1: Default workspace
- Terraform starts with default workspace:
```sh
terraform workspace list
terraform workspace show
```

#### 🔐 Step 2: Creating a new workspace
```sh
terraform workspace new staging
terraform workspace select staging
terraform apply
```
- This creates a separate state file for staging.

#### 🧠 Step 3: Benefits
- Avoids code duplication
- Maintains isolated states
- Supports multiple environments with the same .tf files

### Real-world Insight

We used workspaces to deploy dev, staging, and prod clusters from the same configuration, preventing accidental production updates during testing.

### Key takeaway

Workspaces provide isolated state management for multiple environments, enabling safe and reusable deployments.
