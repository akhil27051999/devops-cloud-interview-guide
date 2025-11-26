## 14. Provider Version Conflicts
### Question

How do you resolve Terraform provider version conflicts?

### Short explanation

Tests knowledge of managing provider versions to ensure reproducibility.

### Answer

Specify an exact provider version in your configuration to avoid conflicts between Terraform versions or modules.

### Detailed explanation
#### 🧾 Step 1: Specify required provider
```yaml
terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 4.20"
    }
  }
}
```

#### 🔐 Step 2: Initialize Terraform
```sh
terraform init
```

- Installs the correct provider version
- Prevents unexpected upgrades or breaking changes

#### 🧠 Step 3: Benefits
- Ensures consistent behavior across environments
- Avoids breaking changes due to automatic provider updates

#### Real-world Insight

In a multi-module project, specifying provider versions prevented Terraform from failing when a newer AWS provider deprecated associate_public_ip_address.

#### Key takeaway

Explicit provider versioning ensures stability and reproducibility in Terraform projects.
