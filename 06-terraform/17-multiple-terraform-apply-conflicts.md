## 17. Multiple terraform apply Conflicts
### Question

How do you prevent conflicts when multiple users run Terraform concurrently?

### Short explanation

Tests collaborative workflow practices.

### Answer
- Use remote backend with state locking (e.g., S3 + DynamoDB)
- Prevents simultaneous apply operations that may overwrite state

### Detailed explanation
#### 🧾 Step 1: Configure remote backend
```yaml
terraform {
  backend "s3" {
    bucket         = "terraform-state-bucket"
    key            = "prod/terraform.tfstate"
    region         = "us-east-1"
    dynamodb_table = "terraform-lock"
  }
}
```

#### 🔐 Step 2: Apply safely
- Terraform automatically locks the state during apply
- Other users will be blocked until the lock is released

#### 🧠 Step 3: Benefits
- Prevents accidental deletion or duplication of resources
- Supports safe team collaboration

### Real-world Insight

Without locking, two engineers applied the same configuration simultaneously, causing duplicate EC2 instances. Enabling locking resolved the problem.

### Key takeaway

State locking is essential for multi-user Terraform workflows.
