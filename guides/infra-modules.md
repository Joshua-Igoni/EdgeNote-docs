# 02 · Infrastructure-as-Code (Terraform)

### 2.1  Repository layout
modules/
network/ vpc, subnets, igw, nat
security/ sg + iam boundary
alb/ alb, listeners, target-group
ecs/ cluster, task-def, service
rds/ subnet-group, secret, pg-param
edge/ s3 bucket, oac, cloudfront

markdown
Copy
Edit

`main.tf` wires the modules; state is stored in  
`s3://tf-state-<account>-eu-central-1`, lock table `tf-lock-<account>`.

### 2.2  Key variables
| Name | Description | Example |
| ---- | ----------- | ------- |
| `project_name` | prefix for all AWS resources | `notejam` |
| `container_image` | ECR URI supplied by app pipeline | `…/cpapp:<sha>` |
| `db_password` | Injected from GH Secret | *(not in state, only Secrets Mgr)* |

### 2.3  Module highlights  
* **edge**  
  * `aws_cloudfront_distribution` with two origins: ALB & S3.  
  * OAC policy (SigV4) restricts S3 to CloudFront **only**.  
* **security**  
  * IAM boundary policy blocks wildcard `*:*`.  
  * SSM Exec boundary for ad-hoc debugging.
* **network**  
  * Route tables: public → IGW, private → NAT (*one per AZ*).  
  * Subnet tags for `kubernetes.io/role/internal-elb` future-proofing.  

### 2.4  Destroy strategy
The destroy workflow first:
1. Drains ECS service to 0 tasks.
2. Manually deletes ALB → releases EIPs.  
3. Runs `terraform destroy` (IGW detach, bucket policy delete).  
4. Purges the S3 state bucket last (guarded by `prevent_destroy = true`).

*(see `.github/workflows/destroy.yml` in CPterraform).*