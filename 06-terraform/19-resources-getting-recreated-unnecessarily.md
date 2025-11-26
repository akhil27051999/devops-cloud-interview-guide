## 18. Resources Getting Recreated Unnecessarily
### Question

Why does Terraform sometimes recreate resources, and how can you prevent it?

### Short explanation

Tests understanding of immutable fields and lifecycle management.

### Answer
- Immutable fields (like subnet_id) changing will force recreation.
- Use ignore_changes or proper lifecycle configuration to avoid unnecessary destruction.

### Detailed explanation
#### 🧾 Step 1: Identify the resource
```sh
terraform plan
```
- Shows resource marked for destruction and recreation

#### 🔐 Step 2: Lifecycle block
```yaml
resource "aws_instance" "web" {
  ami           = "ami-123456"
  instance_type = "t2.micro"

  lifecycle {
    ignore_changes = [tags]
  }
}
```

#### 🧠 Step 3: Benefits
- Prevents downtime caused by unnecessary recreation
- Keeps manual changes to certain attributes safe

### Real-world Insight

A manually updated tag was causing Terraform to recreate web servers. Using ignore_changes preserved the resource.

### Key takeaway

Use lifecycle management to prevent unintentional resource recreation, critical in production.
