## 10. Terraform Import
### Question

How do you manage existing resources in Terraform without recreating them?

### Short explanation

Tests ability to bring existing infrastructure under Terraform control.

### Answer

Use terraform import to link existing resources to Terraform state without recreating them.

### Detailed explanation
#### 🧾 Step 1: Import example
```sh
terraform import aws_s3_bucket.mybucket my-existing-bucket
```
- Associates existing S3 bucket with Terraform resource block

###🔐 Step 2: Update Terraform config
```yaml
resource "aws_s3_bucket" "mybucket" {
  bucket = "my-existing-bucket"
  acl    = "private"
}
```
- Ensures subsequent plan/apply operations manage the resource

#### 🧠 Step 3: Benefits
- Avoids downtime or resource duplication
- Brings legacy infra under IaC

### Real-world Insight

Importing existing VPCs allowed our team to manage previously deployed AWS resources with Terraform without disrupting production.

### Key takeaway

Terraform import is essential for transitioning manual or legacy infrastructure into code-managed workflows.
