## 1. What is Terraform and why is it used?
### Question
What is Terraform, and why would you choose it over manual provisioning or other tools?

### Short explanation

This checks your understanding of Terraform as an Infrastructure as Code (IaC) tool and its benefits over manual or semi-automated provisioning.

### Answer

Terraform is an open-source declarative IaC tool by HashiCorp that enables provisioning, management, and versioning of infrastructure across multiple cloud providers, such as AWS, Azure, GCP, or Kubernetes.

### Detailed explanation
#### 🧾 Step 1: Declarative Infrastructure

In Terraform, you define desired state in .tf files.

Example:
```sh
resource "aws_s3_bucket" "mybucket" {
  bucket = "my-production-bucket"
  acl    = "private"
}
```
#### 🔐 Step 2: Provider Agnostic

- Terraform uses providers to interact with cloud APIs.
- Common providers: aws, azure, google, kubernetes.

#### 🧠 Step 3: State Management
- Terraform tracks resources in a state file (terraform.tfstate), which enables incremental updates.
- Avoids recreating unchanged resources.

#### ⚙️ Step 4: Dependency Management
- Terraform automatically understands dependencies between resources, e.g., EC2 depends on VPC:
```yaml
resource "aws_vpc" "main" {
  cidr_block = "10.0.0.0/16"
}

resource "aws_instance" "web" {
  ami           = "ami-123456"
  instance_type = "t2.micro"
  subnet_id     = aws_vpc.main.id
}
```

### Real-world Insight

Using Terraform, we deployed identical dev, staging, and production environments in AWS without manual console actions, reducing setup errors by 80%.

### Key takeaway

Terraform provides declarative, version-controlled, and repeatable infrastructure provisioning across multiple platforms.
