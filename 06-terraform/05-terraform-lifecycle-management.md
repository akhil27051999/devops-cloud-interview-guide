## 5. Terraform Lifecycle Management (create_before_destroy)
### Question

How do you ensure zero-downtime updates for critical resources?

### Short explanation

Tests understanding of resource lifecycle customization in Terraform.

### Answer

Use the lifecycle block with create_before_destroy to instruct Terraform to create a new resource before deleting the old one.

### Detailed explanation
#### 🧾 Step 1: Lifecycle block example
```yaml
resource "aws_instance" "web" {
  ami           = "ami-123456"
  instance_type = "t2.micro"

  lifecycle {
    create_before_destroy = true
  }
}
```

####🔐 Step 2: Use cases
- Upgrading EC2 instances without downtime
- Updating RDS instances with minimal disruption

#### 🧠 Step 3: Considerations
- Increases cost temporarily as two resources exist simultaneously
- Not all resources support create_before_destroy

### Real-world Insight

Using create_before_destroy allowed zero-downtime deployment of web servers behind an ALB.

### Key takeaway

Lifecycle rules let you control how resources are created and destroyed, crucial for production safety.
