## 12. Terraform Lifecycle Ignore Changes
### Question

How can you prevent Terraform from updating a field that may change outside Terraform?

### Short explanation

Useful when certain attributes are managed outside Terraform (manual or external updates).

### Answer

Use ignore_changes in the lifecycle block.

### Detailed explanation
#### 🧾 Example
```yaml
resource "aws_instance" "web" {
  ami           = "ami-123456"
  instance_type = "t2.micro"

  lifecycle {
    ignore_changes = [tags["Name"]]
  }
}
```
#### 🔐 Purpose
- Terraform will ignore manual changes to the resource name tag
- Prevents unnecessary updates or conflicts

### 🧠 Best Practices

Only ignore fields that truly require manual management

### Real-world Insight

Ignoring auto-assigned ELB DNS names prevented Terraform from recreating instances unnecessarily.

### Key takeaway

ignore_changes ensures Terraform only manages the fields you intend, avoiding accidental resource recreation.
