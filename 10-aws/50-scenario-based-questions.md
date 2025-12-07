# AWS Troubleshooting Runbook — Top 50 Scenarios

Tip: replace `i-0123456`, `bucket-name`, `db-instance-endpoint`, `sg-0123`, `cluster-name`, etc. with real IDs from the customer environment.

This README contains practical, repeatable, step-by-step procedures for 50 common AWS issues. Each item contains:
- Quick checks
- Step-by-step actions (commands / console checks)
- Verify
- Preventive measures

Use these as runbook snippets during interviews, projects, or real tickets.

---

## Table of Contents
- EC2 (Compute)
  1. EC2 instance won’t start
  2. Can’t SSH into EC2
  3. EC2 instance terminating immediately
  4. High CPU on EC2
  5. Elastic IP assigned but unreachable
- S3 (Storage)
  6. S3 “Access Denied”
  7. S3 static website 403 / 404
  8. S3 Cross-Region Replication not working
  9. Multipart upload stuck / incomplete
  10. S3 performance bottlenecks (hot prefix)
- RDS (Databases)
  11. Can’t connect to RDS
  12. RDS replication lag
  13. RDS high CPU
  14. RDS backup/restore failures
  15. Failover takes too long
- VPC / Networking
  16. EC2 not reachable over internet (timeout)
  17. Private subnet instance can’t access internet
  18. VPC peering not working
  19. DNS resolution failing in VPC
  20. VPN tunnel down / BGP problems
- IAM (Identity & Access)
  21. IAM “Access Denied”
  22. Root account access lost (MFA lost)
  23. Service can’t assume IAM role
  24. Overly permissive IAM policies
  25. STS token / temporary credentials expired
- Lambda (Serverless)
  26. Lambda function times out
  27. Deployment package too large
  28. Lambda permissions denied
  29. Cold-start latency
  30. API Gateway returns 502/504 for Lambda integration
- CloudFormation
  31. Stack rollback on create
  32. Stack stuck in UPDATE_ROLLBACK_FAILED
  33. Resource limit exceeded when creating resources
  34. Circular dependency in template
  35. Drift detected (stack resources differ from template)
- CloudWatch / Monitoring
  36. CloudWatch alarm not triggering
  37. Log stream not appearing in CloudWatch
  38. High CloudWatch charges
  39. Logs delayed / missing
  40. EventBridge rule not firing
- ECS / EKS (Containers)
  41. ECS task not starting
  42. ECS service unhealthy (ALB health checks)
  43. EKS Pod CrashLoopBackOff
  44. Kubeconfig not working for EKS
  45. EKS node not joining cluster
- Billing & Cost Management
  46. Unexpected high bill
  47. Savings Plan not applying
  48. Free Tier unexpectedly used/expired
  49. Reserved Instance (RI) not applied
  50. Budgets not alerting

---

# EC2 (Compute)

### 1) EC2 instance won’t start
Quick checks
- Console → EC2 → Instance status checks.
- CLI:
```
aws ec2 describe-instance-status --instance-ids i-0123456 --include-all-instances
```
Steps
1. Check System/Instance status checks in console or CLI.
2. Get console output:
```
aws ec2 get-console-output --instance-id i-0123456 --output text
```
Look for kernel errors/boot failure.
3. Check attached volumes:
```
aws ec2 describe-volumes --filters Name=attachment.instance-id,Values=i-0123456
```
4. If EBS suspect, stop instance and detach the volume:
```
aws ec2 detach-volume --volume-id vol-xxxx
```
Attach to a helper instance to inspect filesystem.
5. If limits/quota likely (e.g., EBS quota), open Service Quotas console or call support to raise limit.
6. If AMI corrupted, launch new instance from a known-good AMI and attach original volume for data recovery.
Verify
- Instance moves to running and passes both status checks.
- `get-console-output` shows normal boot.
Prevent
- Monitor Service Quotas, snapshot/AMI sanity checks, automated health checks & Auto Recovery.

---

### 2) Can’t SSH into EC2
Quick checks
- Public IP present? `describe-instances`
- Security Group inbound rule for 22?
- Local key permission: `ls -l mykey.pem` (must be 400/600)
Steps
1. Test connectivity from your machine:
```
ssh -vvv -i mykey.pem ec2-user@X.X.X.X
```
Observe errors.
2. Port test:
```
nc -zv X.X.X.X 22
# or
telnet X.X.X.X 22
```
3. Check SG:
```
aws ec2 describe-security-groups --group-ids sg-0123
```
Ensure inbound allows your IP on port 22.
4. Check NACL for the subnet:
```
aws ec2 describe-network-acls --filters Name=vpc-id,Values=vpc-xxx
```
5. If no SSH allowed, use Session Manager (SSM):
```
aws ssm start-session --target i-0123456
```
(SSM agent & role required)
6. If key missing, add a new key by creating an AMI or using EC2 Instance Connect / user-data to inject a key.
Verify
- Successful SSH session or SSM shell.
Prevent
- Use SSM for bastionless access; restrict SGs to known IPs; rotate keys.

---

### 3) EC2 instance terminating immediately
Quick checks
- CloudTrail for RunInstances/TerminateInstances events.
- Check Auto Scaling Group activities.
Steps
1. Check CloudTrail: look for `TerminateInstances` / `RunInstances` events referencing the instance.
2. Check Auto Scaling:
```
aws autoscaling describe-scaling-activities --auto-scaling-group-name MyASG
```
3. If Spot:
```
aws ec2 describe-spot-instance-requests --filters Name=instance-id,Values=i-0123456
```
4. Inspect system logs:
```
aws ec2 get-console-output --instance-id i-0123456
```
5. If AMI incompatibility suspected, attempt to launch with a different instance type or AMI.
Verify
- Identify the trigger (ASG scale-in, Spot interruption, system failure) and replicate fix.
Prevent
- Use lifecycle hooks and Auto Scaling termination protections for critical instances.

---

### 4) High CPU on EC2
Quick checks
- CloudWatch CPU metrics, `top` on instance.
Steps
1. Check:
```
aws cloudwatch get-metric-statistics ...
```
or use console to view CPU trend.
2. SSH into instance:
```
top
ps aux --sort=-%cpu
```
to find process.
3. If app issue: profile and fix code (memory leak, infinite loop).
4. Temporary mitigation: scale vertically (change instance type) or horizontally (add ASG).
5. Add Auto Scaling policies: scale out at CPU threshold.
Verify
- CPU returns to normal; auto-scaling triggers as expected.
Prevent
- Baseline sizing, load testing, implement autoscaling and alarms.

---

### 5) Elastic IP assigned but unreachable
Quick checks
```
aws ec2 describe-addresses --public-ips X.X.X.X
```
Steps
1. Confirm EIP associated with correct ENI: `describe-addresses`.
2. Ensure subnet’s route table has `0.0.0.0/0 -> igw-xxxx`.
3. Check Security Group inbound allows required traffic.
4. Ensure instance has a public IPv4 (or EIP attached to ENI).
5. If NAT setup involved, verify NAT/route for outbound connectivity.
Verify
- Ping/SSH to EIP succeeds (where ICMP/SSH allowed).
Prevent
- Use IaC (CloudFormation/Terraform) for consistent networking.

---

# S3 (Storage)

### 6) S3 “Access Denied”
Quick checks
- CloudTrail GetObject/PutObject AccessDenied events.
- CLI:
```
aws s3api get-bucket-policy --bucket bucket-name
```
Steps
1. Identify failing IAM principal (user/role) from CloudTrail.
2. Simulate policy:
```
aws iam simulate-principal-policy --policy-source-arn arn:aws:iam::123456789012:role/RoleName --action-names s3:PutObject
```
3. Inspect bucket policy & object ACLs:
```
aws s3api get-bucket-policy --bucket bucket-name
aws s3api get-object-acl --bucket bucket-name --key path/to/object
```
4. Fix: grant explicit `s3:GetObject` / `s3:PutObject` to the principal or adjust bucket policy to allow the principal’s ARN.
5. Re-test with AWS CLI using assumed role credentials.
Verify
- `aws s3 cp file s3://bucket-name/file` succeeds.
Prevent
- Use least-privilege roles and test policies in IAM Policy Simulator before applying.

---

### 7) S3 static website 403 / 404
Quick checks
```
aws s3api get-bucket-website --bucket bucket-name
```
- Check if `index.html` exists.
Steps
1. Confirm website hosting is enabled and index/error documents set.
2. Check object permissions:
```
aws s3api get-object-acl --bucket bucket-name --key index.html
```
3. If using public access block, inspect:
```
aws s3api get-bucket-policy-status --bucket bucket-name
```
4. If using CloudFront, ensure origin access is configured (OAC/OAI) and CloudFront behavior is correct.
5. Fix by uploading `index.html`, adjusting bucket policy or setting CloudFront origin and invalidation.
Verify
- Access website endpoint or CloudFront domain returns index.
Prevent
- Automate deployment (CI) and add health check for website endpoints.

---

### 8) S3 Cross-Region Replication not working
Quick checks
```
aws s3api get-bucket-replication --bucket bucket-name
aws s3api get-bucket-versioning --bucket bucket-name
```
Steps
1. Ensure versioning enabled on source and destination:
```
aws s3api put-bucket-versioning --bucket source-bucket --versioning-configuration Status=Enabled
aws s3api put-bucket-versioning --bucket dest-bucket --versioning-configuration Status=Enabled
```
2. Check replication configuration and IAM role exists and has `s3:ReplicateObject` etc.
3. Inspect replication status in CloudWatch / S3 metrics for replication.
4. If missing role permissions, update trust policy and the role with required actions.
5. Re-upload an object (with versioning) and watch replication.
Verify
- Object appears in target bucket with metadata `x-amz-replication-status`.
Prevent
- Validate replication during infra deployments; monitor replication metrics.

---

### 9) Multipart upload stuck / incomplete
Quick checks
```
aws s3api list-multipart-uploads --bucket bucket-name
```
Steps
1. List stuck uploads and identify `UploadId`.
2. Abort specific upload:
```
aws s3api abort-multipart-upload --bucket bucket-name --key file --upload-id UploadId
```
3. Implement lifecycle rule to abort incomplete uploads older than N days.
4. On client side, ensure retries with exponential backoff; adjust part size.
Verify
- `list-multipart-uploads` returns no stuck uploads for that object.
Prevent
- Configure lifecycle `AbortIncompleteMultipartUpload` and client-side robust uploader.

---

### 10) S3 performance bottlenecks (hot prefix)
Quick checks
- CloudWatch AllRequests and per-prefix metrics.
Steps
1. Identify access pattern hitting single prefix.
2. Recommend key name randomization (hash prefix) or use UUID-based prefixes.
3. Optionally use S3 Transfer Acceleration, CloudFront for reads.
4. Retest load and observe reduced per-prefix latency.
Verify
- Latency/throughput improves; CloudWatch per-prefix metrics show distribution.
Prevent
- Key naming strategy and performance testing.

---

# RDS (Databases)

### 11) Can’t connect to RDS
Quick checks
- Check endpoint, port and security group.
- `telnet db-instance-endpoint 3306` (or `nc -zv`)
Steps
1. Confirm the DB’s endpoint and `PubliclyAccessible` flag if trying from internet.
2. Security Group: ensure inbound allows your source IP on DB port.
3. Check subnet group: DB in private subnet requires NAT/ bastion.
4. Check DB parameter `max_connections`: `SHOW VARIABLES LIKE 'max_connections';`
5. If connecting from a VPC, ensure route table & NACL allow return traffic.
6. Use RDS console’s Connectivity & security tab for quick diagnostics.
Verify
- Connect with:
```
mysql -h endpoint -u user -p
```
Prevent
- Use Bastion or VPN, maintain connection pool & connection timeouts.

---

### 12) RDS replication lag
Quick checks
- CloudWatch metric: `ReplicaLag` (for replicas).
Steps
1. Enable Performance Insights / Enhanced Monitoring.
2. Identify long-running queries: `SHOW PROCESSLIST;` and slow query logs.
3. Kill runaway queries or optimize them; add indexes if needed.
4. Consider scaling replica instance or moving to larger instance class.
5. Offload heavy reporting to separate reporting replica.
Verify
- `ReplicaLag` drops to acceptable seconds.
Prevent
- Regular query tuning, monitor replica lag and set alarms.

---

### 13) RDS high CPU
Quick checks
- CloudWatch `CPUUtilization` and Performance Insights.
Steps
1. Use Performance Insights to find top SQL statements.
2. Run `EXPLAIN` on heavy queries; add indexes or rewrite queries.
3. Consider increasing instance size or adding read replicas to distribute read load.
4. If sudden, check maintenance/backup tasks.
Verify
- CPU trend decreases; average query time improved.
Prevent
- Periodic index maintenance, slow query logs enabled.

---

### 14) RDS backup/restore failures
Quick checks
```
aws rds describe-events --source-identifier db-instance-id
```
Steps
1. Inspect RDS events for snapshot errors.
2. Check IAM role used by RDS has `rds:CreateDBSnapshot` or required S3 permissions for export.
3. Confirm there’s enough storage; increase allocated storage if full.
4. Re-run snapshot/restore.
Verify
- Snapshot status available.
Prevent
- Monitor storage; test restore procedures regularly.

---

### 15) Failover takes too long
Quick checks
- Confirm app uses cluster endpoint (writer) vs instance endpoint.
Steps
1. Check whether the app connects to a fixed instance endpoint — if so, change to cluster endpoint for Aurora.
2. Reduce DNS TTL in client caches or configure clients to handle DNS change.
3. Test failover (with `failover-db-cluster` for Aurora — careful in prod).
4. Evaluate RDS failover metrics and tune.
Verify
- Failover completes within target RTO and app reconnects quickly.
Prevent
- Use cluster endpoints, test failovers and reduce DNS caching in apps.

---

# VPC / Networking

### 16) EC2 not reachable over internet (timeout)
Quick checks
- Route table has IGW? `describe-route-tables`
- Instance has Public IP?
Steps
1. Check subnet association with route table and that route table has `0.0.0.0/0 -> igw-xxx`.
2. Ensure instance has public IPv4 / EIP and ENI is attached.
3. Verify SG and NACL allow inbound/outbound (for ephemeral ports).
4. If using NAT, ensure for outbound traffic NAT exists and route table points to NAT.
Verify
- Ping/SSH succeeds.
Prevent
- Validate VPC design templates in IaC.

---

### 17) Private subnet instance can’t access internet
Quick checks
- Route table for private subnet points to NAT?
Steps
1. Check NAT Gateway exists in a public subnet and has Elastic IP.
2. Ensure private subnet’s route table sets `0.0.0.0/0 -> nat-xxxx`.
3. Verify NAT’s subnet route to IGW.
4. Ensure security groups allow outbound.
Verify
- Instance can `curl https://example.com`.
Prevent
- Use NAT redundancy (>1 NAT per AZ) and monitoring.

---

### 18) VPC peering not working
Quick checks
- CIDR overlap? `describe-vpcs`
Steps
1. Confirm peering connection is active.
2. Add route entries in both VPCs’ route tables for peer CIDR via peering connection.
3. Ensure Security Groups allow traffic from peer CIDR.
4. Remember peering does not support transitive routing; ensure route architecture matches.
Verify
- `ping` or `telnet` works between instances in different VPCs.
Prevent
- Plan CIDRs centrally (IPAM), test route table propagation.

---

### 19) DNS resolution failing in VPC
Quick checks
```
aws ec2 describe-vpcs --vpc-ids vpc-xxxx
# check EnableDnsSupport & EnableDnsHostnames
```
Steps
1. Enable DNS hostnames/support in VPC if disabled.
2. Validate `/etc/resolv.conf` on instance points to VPC resolver (169.254.169.253 or .2).
3. If using Route53 private hosted zone, confirm zone is associated with the VPC.
4. For cross-account/private zones, check association and permissions.
Verify
- `nslookup internal-service` returns private IP.
Prevent
- Document DNS settings and test after VPC changes.

---

### 20) VPN tunnel down / BGP problems
Quick checks
- Console: VPN connection / tunnel status.
```
aws ec2 describe-vpn-connections --vpn-connection-ids vpn-xxxx
```
Steps
1. Review VPN tunnel logs and CloudWatch metrics for tunnel status.
2. Check customer gateway device for BGP session/misconfiguration (ASNs, PSK).
3. Reconfigure BGP peers or reset BGP sessions.
4. If using Direct Connect, validate virtual interface and BGP.
Verify
- BGP session established; ping across tunnel.
Prevent
- Dual tunnels, monitoring for BGP flaps, runbooks for failover.

---

# IAM (Identity & Access)

### 21) IAM “Access Denied”
Quick checks
- CloudTrail: denied action and principal.
Steps
1. Identify principal ARN in CloudTrail event.
2. Use IAM Policy Simulator:
```
aws iam simulate-principal-policy --policy-source-arn arn:aws:iam::123:role/RoleName --action-names s3:PutObject
```
3. Add/adjust policy to include the specific action/resource.
4. Test by assuming role credentials and re-running action.
Verify
- Previously denied API call now succeeds.
Prevent
- Use role-based access control (RBAC), test policies in sandbox.

---

### 22) Root account access lost (MFA lost)
Quick checks
- Confirm root user access attempts failed and MFA lost.
Steps
1. Gather account owner proof (billing info, corporate docs).
2. Open AWS Billing/Support case for root account recovery and follow AWS account recovery flow.
3. After recovery, add alternative root contact and store recovery codes.
Verify
- Root access restored; MFA reconfigured.
Prevent
- Break-glass procedures, store root credentials securely, delegate everyday tasks to IAM users.

---

### 23) Service can’t assume IAM role
Quick checks
```
aws iam get-role --role-name roleName
```
Check role trust relationship.
Steps
1. Inspect the role’s trust policy JSON for `sts:AssumeRole` and the allowed principal.
2. If missing, update trust relationship:
```
aws iam update-assume-role-policy --role-name roleName --policy-document file://trust.json
```
3. Verify that the service principal (e.g., `ec2.amazonaws.com`) or account ARN is correct.
Verify
- Service can assume role; `sts:AssumeRole` succeeds.
Prevent
- Keep standardized trust policies in IaC; test role assumption.

---

### 24) Overly permissive IAM policies
Quick checks
- Use IAM Access Analyzer to detect `*:*` or broad resources.
Steps
1. Run IAM Access Analyzer and list policies with wildcards.
2. Replace wildcard actions/resources with minimal required actions and resource ARNs.
3. Test using policy simulator.
Verify
- Functionality unaffected but permissions narrowed.
Prevent
- Policy reviews, approval workflow, least-privilege approach.

---

### 25) STS token / temporary credentials expired
Quick checks
- Expiry error messages in logs.
Steps
1. Check token duration and creation time: use `aws sts get-caller-identity` (using credentials).
2. For long-running processes, use longer session durations or refresh tokens periodically.
3. If using SDKs, implement automatic refresh (e.g., credential provider chain).
Verify
- Credentials rotate and processes continue without failing auth.
Prevent
- Use short-lived credentials with automated refresh mechanisms; avoid baked-in long-term keys.

---

# Lambda (Serverless)

### 26) Lambda function times out
Quick checks
- CloudWatch Logs show last logged timestamp and REPORT with Duration.
Steps
1. Inspect CloudWatch logs to see what the function was doing before timeout.
2. Increase timeout in function configuration:
```
aws lambda update-function-configuration --function-name MyFn --timeout 120
```
3. Optimize code: avoid blocking calls; use async where possible.
4. If long-running task, offload to Step Functions or break into multiple functions.
Verify
- Function completes within new timeout, or distributed to Step Functions.
Prevent
- Right-size function timeout/memory; use timeouts for downstream calls.

---

### 27) Deployment package too large
Quick checks
- Error during `aws lambda update-function-code` mentions package size > limit.
Steps
1. Identify large dependencies with packaging tools (pipdeptree / `pip show`).
2. Move heavy dependencies to Lambda Layers or use container image for Lambda (up to 10GB).
3. If using ZIP, reduce by excluding dev/test dependencies and using multi-stage Docker builds.
Verify
- Deployment succeeds and function works as expected.
Prevent
- Keep function packages minimal and leverage Layers.

---

### 28) Lambda permissions denied
Quick checks
- CloudWatch logs show AccessDenied when calling resource.
Steps
1. Check Lambda execution role:
```
aws lambda get-function-configuration --function-name MyFn
# -> Role
```
2. Inspect role’s attached policies:
```
aws iam list-attached-role-policies --role-name RoleName
```
3. Attach missing managed or inline policy for the service (e.g., `s3:GetObject`).
4. Re-deploy and test.
Verify
- Function can access the external resource without AccessDenied.
Prevent
- Use least-privilege roles and test end-to-end.

---

### 29) Cold-start latency
Quick checks
- Observe increased duration on first invocation after idle (CloudWatch).
Steps
1. For VPC-enabled Lambdas, consider moving out of VPC or configuring ENI warmers; or enable Provisioned Concurrency:
```
aws lambda put-provisioned-concurrency-config --function-name MyFn --qualifier 1 --provisioned-concurrent-executions 5
```
2. Reduce package size and initialization code.
3. Use lighter runtimes or keep instances warm with scheduled invocations.
Verify
- Cold-start latency reduced; warm-up invocations fast.
Prevent
- Monitor cold starts and use provisioned concurrency for critical paths.

---

### 30) API Gateway returns 502/504 for Lambda integration
Quick checks
- API Gateway execution logs and Lambda logs.
Steps
1. Check Lambda logs to see if function errored or returned invalid response format (for proxy integration: JSON with `statusCode` etc).
2. Validate API mapping templates and integration request/response settings.
3. Ensure Lambda has permission to be invoked by API Gateway:
```
aws lambda add-permission ...
```
4. Test Lambda directly; then test via API Gateway.
Verify
- API returns expected HTTP status codes and payload.
Prevent
- Standardize Lambda response schema and add integration tests.

---

# CloudFormation

### 31) Stack rollback on create
Quick checks
```
aws cloudformation describe-stack-events --stack-name MyStack
```
Look for FAILED reason.
Steps
1. Read the events and find the failing resource and error message.
2. If IAM permission issue, ensure CloudFormation execution role has necessary privileges or supply `--capabilities`.
3. Fix template or missing resources and retry create.
4. If resource limit, request quota increase.
Verify
- Stack reaches `CREATE_COMPLETE`.
Prevent
- Lint templates (`cfn-lint`), test in sandbox account.

---

### 32) Stack stuck in UPDATE_ROLLBACK_FAILED
Quick checks
- Console shows stack status; events show what failed.
Steps
1. Use:
```
aws cloudformation continue-update-rollback --stack-name MyStack
```
to recover.
2. After recovery, fix underlying root cause in template and re-run update.
3. If resources are in inconsistent state, use stack import/replace or manual remediation.
Verify
- Stack returns to `ROLLBACK_COMPLETE` or `UPDATE_ROLLBACK_COMPLETE`.
Prevent
- Create change sets and validate templates before update.

---

### 33) Resource limit exceeded when creating resources
Quick checks
- Error message mentions quota exceeded.
Steps
1. Identify which quota is hit from error (EIPs, VPCs, etc.).
2. Open Service Quotas console and request quota increase for that region and resource.
3. Retry after quota increase or rearchitect to use fewer resources.
Verify
- Resource creation succeeds post-quota increase.
Prevent
- Monitor quotas and request increases ahead of large deployments.

---

### 34) Circular dependency in template
Quick checks
- CloudFormation error mentions circular dependency.
Steps
1. Analyze resources referencing each other.
2. Break dependency by using `DependsOn` carefully or splitting stack into nested stacks.
3. Use outputs/exports and cross-stack references where appropriate.
Verify
- Template deploys without circular dependency error.
Prevent
- Modularity: separate concerns into nested stacks.

---

### 35) Drift detected (stack resources differ from template)
Quick checks
```
aws cloudformation detect-stack-drift --stack-name MyStack
```
then `describe-stack-resource-drifts`.
Steps
1. Run drift detection to list resources with drift.
2. For drifted resources, reconcile by updating resource to match template or update template to match desired state.
3. If manual changes are necessary, document them and update CFN to avoid future drift.
Verify
- Drift status becomes `IN_SYNC`.
Prevent
- Enforce changes via IaC only; restrict console access.

---

# CloudWatch / Monitoring

### 36) CloudWatch alarm not triggering
Quick checks
- Alarm configuration (metric name, namespace, period, threshold).
Steps
1. Check that the metric data is being published (`ListMetrics`).
2. Confirm alarm period and evaluation periods are correct and not too long.
3. Ensure alarm action targets (SNS topic) are configured and working (subscription confirmed).
4. Recreate alarm if misconfigured.
Verify
- Trigger test alarm or simulate metric to verify.
Prevent
- Test alarms during setup; document thresholds.

---

### 37) Log stream not appearing in CloudWatch
Quick checks
- Confirm log group exists and IAM has permission.
Steps
1. Ensure the resource has `logs:CreateLogStream` and `logs:PutLogEvents`.
2. For EC2, ensure CloudWatch Agent is configured and running.
3. For Lambda, ensure function’s role allows logging.
4. Check region mismatch (logs are region-specific).
Verify
- New log events appear under the group.
Prevent
- Automated agent config and monitoring of missing log groups.

---

### 38) High CloudWatch charges
Quick checks
- List number of custom metrics and log ingestion volumes.
Steps
1. Identify and remove noisy custom metrics.
2. Consolidate metrics and use high-cardinality metrics sparingly.
3. Use longer retention for critical logs and cheaper storage (e.g., S3 via Kinesis Firehose) for older logs.
4. Implement metric filters only where necessary.
Verify
- Monthly cost reduced after cleanup.
Prevent
- Governance on custom metrics creation and retention.

---

### 39) Logs delayed / missing
Quick checks
- Check CloudWatch agent or Firehose buffer settings and network.
Steps
1. Inspect agent logs for connectivity or throttling errors.
2. Increase buffer size or batch size settings carefully.
3. Verify network routes/NAT allow outbound to CloudWatch endpoints.
4. For Kinesis/Firehose, check for backpressure / retry logs.
Verify
- Logs appear within expected time window.
Prevent
- Monitor agent health and set alerts on ingestion delays.

---

### 40) EventBridge rule not firing
Quick checks
- Rule enabled? Target list not empty?
Steps
1. List rules and targets:
```
aws events list-rules
aws events list-targets-by-rule --rule MyRule
```
2. Confirm rule’s event pattern matches events (test with `PutEvents`).
3. Ensure target permissions: e.g., for Lambda target, run `aws lambda add-permission --statement-id ... --action lambda:InvokeFunction ...`
4. Review CloudWatch logs for invocation failures.
Verify
- Test event triggers target and check logs.
Prevent
- Unit tests for event patterns and target permissions.

---

# ECS / EKS (Containers)

### 41) ECS task not starting
Quick checks
- Task status `PENDING` / error message from service events.
Steps
1. Describe the task:
```
aws ecs describe-tasks --cluster MyCluster --tasks taskId
```
to view last status/reason.
2. If insufficient resources, increase cluster capacity or adjust task CPU/memory.
3. If image pull failure, check ECR auth and ImagePullBackOff.
4. Inspect container logs for startup errors.
Verify
- Task enters `RUNNING` and passes health checks.
Prevent
- Right-size task defs and autoscaling container instances / Fargate concurrency.

---

### 42) ECS service unhealthy (ALB health checks)
Quick checks
- Check target group health in EC2 console.
Steps
1. Confirm health check path/port and the container actually listens there.
2. Check container app logs for startup or binding errors.
3. Ensure security groups allow ALB → container traffic.
4. Update health check grace period or path as appropriate.
Verify
- Targets in target group show healthy.
Prevent
- Proper health check endpoints, readiness probes.

---

### 43) EKS Pod CrashLoopBackOff
Quick checks
```
kubectl describe pod <pod>
kubectl logs <pod> --previous
```
Steps
1. Get events:
```
kubectl describe pod podname -n namespace
```
→ check `CrashLoopBackOff` reason.
2. Inspect logs:
```
kubectl logs podname -n namespace
```
3. If image pull fail: ensure image exists and pull secrets are correct.
4. If `OOMKilled`, increase resource limits/requests or fix memory leak.
5. Fix liveness/readiness probes if misconfigured.
Verify
- Pod moves to `Running` with stable restarts.
Prevent
- Resource requests/limits and staging tests.

---

### 44) Kubeconfig not working for EKS
Quick checks
```
aws eks update-kubeconfig --name cluster-name
```
Steps
1. Run the above command to refresh kubeconfig.
2. Ensure IAM user/role is mapped in `aws-auth` ConfigMap for cluster access:
```
kubectl get configmap aws-auth -n kube-system -o yaml
```
3. If missing, update `aws-auth` to include user/role.
Verify
- `kubectl get nodes` returns cluster nodes.
Prevent
- Automate kubeconfig refresh and mapping in IaC.

---

### 45) EKS node not joining cluster
Quick checks
- Node shows in EC2 but not in `kubectl get nodes`.
Steps
1. Check worker node user-data/logs for bootstrap errors (cloud-init).
2. Ensure node IAM role has `AmazonEKSWorkerNodePolicy` and CNI permissions.
3. Confirm correct cluster name in kubelet bootstrap arguments.
4. Recreate node if bootstrap irrecoverable.
Verify
- Node registers and becomes `Ready`.
Prevent
- Use managed node groups or `eksctl`/CloudFormation patterns.

---

# Billing & Cost Management

### 46) Unexpected high bill
Quick checks
- AWS Billing Console / Cost Explorer → identify top services/dates.
Steps
1. Open Cost Explorer and filter by service/region/time to find spike.
2. Inspect CloudTrail for resource creation events around spike time.
3. Identify idle/forgotten resources: unattached EBS volumes, unused snapshots, idle RDS, large S3 data transfer.
4. Stop/terminate unused resources; enable lifecycle policies and S3 Intelligent Tiering.
5. Set Budgets/Alerts.
Verify
- Monthly forecast reduces after cleanup.
Prevent
- Tagging + automated scripts to find and stop idle resources; budgets with alerts.

---

### 47) Savings Plan not applying
Quick checks
- Check which usage is covered by SP (EC2 instance family/region matching).
Steps
1. In Billing → Savings Plans, inspect covered usage.
2. If mismatch, ensure instances are of covered family or convert usage to covered type.
3. Purchase right SP type if necessary.
Verify
- Cost Explorer shows applied SP discounts.
Prevent
- Align workloads to SP family/region and monitor usage patterns.

---

### 48) Free Tier unexpectedly used/expired
Quick checks
- Billing → Free Tier usage dashboard.
Steps
1. Identify services exceeding free tier (e.g., hours of EC2).
2. Stop/terminate instances or upgrade plan.
3. Educate user on Free Tier limits and set budgets.
Verify
- Free Tier usage goes below threshold for next cycle.
Prevent
- Tag FreeTier test resources and auto-terminate after test period.

---

### 49) Reserved Instance (RI) not applied
Quick checks
- Billing/RIs list and instance attributes.
Steps
1. Check RI scope (AZ/Region) and instance attributes (size, platform).
2. If mismatch, consider modifying RIs (convertible RIs) or adjust instance attributes.
3. Ensure instance is running in covered region and family.
Verify
- Cost Explorer shows hourly usage matched to RI discounts.
Prevent
- Plan purchases based on reported usage patterns.

---

### 50) Budgets not alerting
Quick checks
- Budget notification config and SNS subscription confirmation.
Steps
1. Verify budget configuration: threshold, time unit, scope.
2. Make sure notification goes to SNS topic and that topic has confirmed subscriptions (email/HTTPS).
3. Test by lowering threshold to trigger test alert.
4. If SNS publish fails, check IAM permissions for budgets SNS publish.
Verify
- Alert is received by intended recipient.
Prevent
- Periodic test of budget notifications and ensure alternate channels (Slack/SMS).

---

End of runbook.

Notes:
- Replace placeholder IDs and names with your environment values.
- Keep this README in sync with your internal runbooks and IaC.
- Consider converting repetitive checks into small automation scripts or AWS Systems Manager documents for repeatable execution.
