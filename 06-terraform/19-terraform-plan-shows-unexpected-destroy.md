## 19. Terraform Plan Shows Unexpected Destroy
### Question

Why does terraform plan sometimes show resources to be destroyed unexpectedly?

### Short explanation

Tests understanding of drift, missing resources, or misconfigured provider.

### Answer

- Causes: Drift, manual deletion, state mismatch, provider misconfiguration.
- Resolution: Import missing resources, fix configuration, re-run plan.

### Detailed explanation
#### 🧾 Step 1: Identify the resource
```sh
terraform plan
```
- Compare desired state vs actual state

#### 🔐 Step 2: Import missing resources
```sh
terraform import aws_s3_bucket.mybucket my-existing-bucket
```

#### 🧠 Step 3: Re-plan and apply
```sh
terraform plan
terraform apply
```

### Real-world Insight

A production S3 bucket was deleted outside Terraform; importing the bucket avoided unnecessary destruction.

### Key takeaway

Always reconcile state before applying changes to prevent accidental resource deletion.
