## 11. Remote State and Locking
### Question
Why is remote state and locking important in Terraform?

### Short explanation

Checks understanding of multi-user collaboration and avoiding conflicts.

### Answer

Remote state stores the Terraform state file centrally, while locking prevents multiple users from running terraform apply simultaneously, avoiding conflicts.

### Detailed explanation
#### 🧾 Step 1: Remote backend (AWS example)
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

#### 🔐 Step 2: Locking
- DynamoDB table locks state automatically during apply
- Prevents accidental concurrent modifications

#### 🧠 Step 3: Benefits
- Safe multi-user workflows
- Prevents accidental resource deletion or duplication

### Real-world Insight

Enabling S3 backend with DynamoDB locking allowed 3 engineers to safely collaborate on infrastructure changes without overwriting each other’s work.

### Key takeaway

Remote state + locking = safe, collaborative Terraform workflows.
