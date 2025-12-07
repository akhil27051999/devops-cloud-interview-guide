# AWS Troubleshooting Cheat Sheet

A concise troubleshooting matrix for common AWS services. Each row lists a typical issue, the recommended response (how to fix it), and best practices to avoid recurrence.

| Component | Issue | Response (How You’d Fix It) | Best Practice |
|---|---|---|---|
| EC2 (Elastic Compute Cloud) | Cannot SSH/RDP into instance | Check instance state → verify Security Groups (SGs) / NACLs → confirm route to IGW/NAT → use EC2 Instance Connect or SSM. | Use Elastic IPs, least-privilege SGs, CloudWatch connectivity alarms. |
| EC2 (Elastic Compute Cloud) | Instance stopped/terminated | Review CloudTrail logs for user or system events → check Auto Scaling, Spot interruptions. | Enable recovery options, regular AMI + snapshot backups. |
| EC2 (Elastic Compute Cloud) | High CPU/memory utilization | Analyze CloudWatch metrics → inspect processes via SSM → recommend right-sizing or Auto Scaling. | Implement alarms + Auto Scaling for elasticity. |
| S3 (Simple Storage Service) | Access Denied error | Verify IAM and bucket policy → check "Block Public Access" → review KMS permissions. | Least-privilege IAM roles, use Access Analyzer + logging. |
| S3 (Simple Storage Service) | Accidental data deletion | Check versioning and replication → restore previous versions if enabled. | Enable Versioning + MFA Delete for protection. |
| S3 (Simple Storage Service) | Slow uploads/downloads | Review upload method → use multipart upload or Transfer Acceleration. | Use multipart + nearest region for large objects. |
| VPC (Virtual Private Cloud) | No Internet access | Verify subnet route to IGW/NAT → check SG/NACL rules. | Separate route tables for public/private, use Flow Logs. |
| VPC (Virtual Private Cloud) | Instances can’t communicate | Check SG/NACL rules → ensure correct subnet/route table association. | Use SG references for internal traffic, clear tagging. |
| VPC (Virtual Private Cloud) | VPN not connecting | Review tunnel status, phase 1/2 logs, and encryption parameters. | Monitor VPN health via CloudWatch, consider Direct Connect. |
| IAM (Identity and Access Management) | User can’t access resources | Use IAM Policy Simulator → check for denies or missing actions. | Apply least privilege, enforce MFA. |
| IAM (Identity and Access Management) | Cross-account access fails | Check trust policies + STS AssumeRole permissions. | Test roles via simulator, document trust relationships. |
| RDS (Relational Database Service) | Connection timeout | Verify endpoint, SGs, subnet groups, and DB accessibility. | Use private subnets + bastion host, secure with IAM auth. |
| RDS (Relational Database Service) | High CPU/slow queries | Analyze Performance Insights + query tuning, scale vertically or use read replicas. | Monitor regularly, use replicas for read scaling. |
| RDS (Relational Database Service) | Failover not working | Review Multi-AZ config + failover logs. | Test failover periodically, monitor replication lag. |
| CloudWatch | Metrics missing | Check if metrics published, verify IAM role + namespace. | Enable detailed monitoring, consistent naming. |
| CloudWatch | Alarms not triggering | Review thresholds + metric data → test alarm actions. | Use anomaly detection + validate SNS subscriptions. |
| CloudWatch | High log ingestion costs | Review retention + filters, export to S3 for archive. | Use retention policies, Logs Insights for queries. |
| CloudFormation (IaC) | Stack creation failed (ROLLBACK) | Check Events tab → fix invalid parameters or permissions. | Use Change Sets before deployment. |
| CloudFormation (IaC) | Stack drift detected | Run drift detection, align stack or resource manually. | Apply stack policies, version templates. |
| Lambda (Serverless) | Function timeout | Review logs → increase timeout or optimize code logic. | Use async invocation + tune memory/time. |
| Lambda (Serverless) | Permission denied to AWS service | Verify execution role permissions (s3:GetObject, etc.). | Use role-per-function + least privilege. |
| Lambda (Serverless) | No logs in CloudWatch | Check role permissions for logs: CreateLogGroup / PutLogEvents. | Standardize logging + centralize with subscriptions. |

Notes
- Keep entries concise in the table; link to runbooks or KB articles from each row for longer procedures.
- Consider splitting very large tables by service into separate files (e.g., README-EC2.md) for readability.

What I did: created a single README.md table version of your supplied matrix.

What's next: I can (a) split this into per-service markdown files, (b) add links to runbooks or playbooks for each issue, or (c convert this into a CSV/HTML table for automation — tell me which you prefer and I’ll generate it. 
