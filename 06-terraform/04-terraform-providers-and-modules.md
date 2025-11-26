## 4. Explain Terraform Providers and Modules
### Question

What are Terraform providers and modules, and why are they important?

### Short explanation

Tests knowledge of reusable infrastructure building blocks and cloud integration.

### Answer

- Provider: Plugin to manage resources for a specific platform (AWS, Azure, GCP).
- Module: Reusable configuration that groups resources, variables, and outputs for consistent infrastructure patterns.

### Detailed explanation
#### 🧾 Step 1: Providers
```yaml
provider "aws" {
  region = "us-east-1"
}
```
- Terraform uses provider APIs to create, update, and delete resources.

#### 🔐 Step 2: Modules
- Example folder structure:
```txt
modules/
  vpc/
    main.tf
    variables.tf
    outputs.tf
```

- Usage:
```yaml
module "vpc" {
  source = "./modules/vpc"
  cidr_block = "10.0.0.0/16"
}
```

#### 🧠 Step 3: Benefits

- Code reuse and maintainability
- Standardizes infrastructure across teams

### Real-world Insight

We created a vpc module used across 5 projects, reducing code duplication and mistakes in network setup.

### Key takeaway

Providers connect Terraform to cloud APIs, while modules enforce consistency and reusability.
