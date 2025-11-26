## 3. Difference between terraform plan and terraform apply
### Question

What is the difference between terraform plan and terraform apply?

### Short explanation

Checks your understanding of Terraform workflow and safe infrastructure changes.

### Answer
- terraform plan shows what changes Terraform will make without applying them.
- terraform apply executes the planned changes to reach the desired state.

### Detailed explanation
#### 🧾 Step 1: Run terraform plan
- terraform plan -out=tfplan
- Terraform compares desired state in .tf files with actual resources.
- Outputs create, update, delete actions.

#### 🔐 Step 2: Run terraform apply
- terraform apply tfplan
- Applies changes to your infrastructure.
- Updates the state file after execution.

#### 🧠 Step 3: Benefits
- Prevents accidental deletion or modification of resources.
- Provides audit visibility before changes.

### Real-world Insight

Using terraform plan saved us from accidentally destroying a production RDS instance during an upgrade.

### Key takeaway

Plan before apply ensures safe, predictable infrastructure changes.
