## 9. Explain Terraform Outputs and Interpolation
### Question

How do you use Terraform outputs to pass data between modules?

### Short explanation
Tests your knowledge of exposing values for other modules or automation pipelines.

### Answer

Terraform outputs allow exporting values from modules or resources for use elsewhere.

### Detailed explanation
#### 🧾 Step 1: Define output
```yaml
output "vpc_id" {
  value = aws_vpc.main.id
}
```
#### 🔐 Step 2: Use output in another module
```yaml
module "subnet" {
  source = "./modules/subnet"
  vpc_id = module.vpc.vpc_id
}
```

#### 🧠 Step 3: Benefits
- Pass resource attributes between modules
- Integrate with CI/CD pipelines
- Simplifies dependent configurations

### Real-world Insight

Outputs enabled our networking module to automatically feed VPC IDs into subnet and security group modules, reducing manual input errors.

### Key takeaway

Outputs allow seamless data flow between Terraform modules and automation workflows.
