## 8. Terraform Drift: detection and correction
### Question

What is Terraform drift and how do you correct it?

### Short explanation

Tests understanding of resource changes outside Terraform.

### Answer

- Drift occurs when a resource is changed manually outside Terraform.
- Correct using terraform plan and terraform apply to reconcile desired state, or import manually changed resources using terraform import.

### Detailed explanation
####🧾 Step 1: Detect drift
```sh
terraform plan
```
- Shows differences between state and actual resources

#### 🔐 Step 2: Correct drift
- If resource exists but state is outdated:
```sh
terraform import aws_instance.web i-0123456789abcdef0
terraform plan
terraform apply
```

#### 🧠 Step 3: Best practices
- Avoid manual changes; use Terraform for all infrastructure
- Enable remote state for team collaboration

### Real-world Insight

Manual change to an S3 bucket ACL caused Terraform to attempt unnecessary changes. Importing the bucket corrected the state.

### Key takeaway

Terraform drift detection ensures actual infrastructure matches desired state, preventing unintended changes.
