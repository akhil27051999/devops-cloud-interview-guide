## 7. Difference between count, for_each, and dynamic blocks
### Question

When should you use count, for_each, or dynamic in Terraform?

### Short explanation

Tests knowledge of scalable resource creation patterns.

### Answer
- count → Creates N identical resources
- for_each → Creates resources based on map/set for named instances
- dynamic → Dynamically generates nested blocks based on input

### Detailed explanation
#### 🧾 Step 1: Using count
```yaml
resource "aws_instance" "web" {
  count = 3
  ami = "ami-123456"
  instance_type = "t2.micro"
}
```
- Creates 3 identical instances

#### 🔐 Step 2: Using for_each
```yaml
resource "aws_instance" "web" {
  for_each = var.servers
  ami = each.value.ami
  instance_type = each.value.type
}
```
- Creates instances with custom names or properties

#### 🧠 Step 3: Using dynamic
```yaml
dynamic "tags" {
  for_each = var.env_tags
  content {
    key   = tags.key
    value = tags.value
  }
}
```
- Generates dynamic nested blocks (e.g., tags) from input maps

### Real-world Insight

Using for_each allowed us to provision multiple EC2 instances with unique names without creating repetitive blocks.

### Key takeaway

count for simple replication, for_each for named resources, dynamic for complex nested configurations.
