## 16. Remote State Corruption
### Question

How do you recover from a corrupted Terraform remote state?

### Short explanation

Tests knowledge of state management and recovery strategies.

### Answer
- Enable versioning on remote backend (e.g., S3)
- Roll back to a previous version
- Re-run terraform plan and apply after recovery

### Detailed explanation
#### 🧾 Step 1: Check S3 version history
- Locate last known good state file

#### 🔐 Step 2: Restore state
- aws s3 cp s3://terraform-state-bucket/prod/terraform.tfstate.OLD terraform.tfstate

#### 🧠 Step 3: Re-apply
```sh
terraform plan
terraform apply
```

### Real-world Insight

Remote state corruption occurred during network failure; S3 versioning allowed quick rollback and safe resumption of infrastructure updates.

### Key takeaway

Enable remote backend versioning to recover from state corruption safely.
