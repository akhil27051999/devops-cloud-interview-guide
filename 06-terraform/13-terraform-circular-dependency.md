## 13. Terraform Circular Dependency
### Question

How do you resolve circular dependency errors?

### Short explanation

Tests understanding of dependency graphs in Terraform.

### Answer

- Terraform detects cycles when resources reference each other in a loop.
- Resolve using depends_on to explicitly order resource creation.

### Detailed explanation
#### 🧾 Step 1: Example
```yaml
resource "aws_vpc" "main" { ... }
resource "aws_subnet" "subnet1" {
  vpc_id = aws_vpc.main.id
  depends_on = [aws_vpc.main]
}
```
#### 🔐 Step 2: Benefits
- Prevents Terraform from failing due to implicit cyclic dependencies

### Real-world Insight

Adding explicit depends_on resolved errors when creating VPC → Subnets → Route Tables with interdependencies.

### Key takeaway

Explicit dependencies prevent Terraform cycles and ensure correct resource creation order.
