# 02 · Infrastructure-as-Code (Terraform)

### 2.1  Repository layout
* **modules/**
- **network/**: vpc, subnets, igw, nat
- **security/**: sg + iam boundary
- **alb/**: alb, listeners, target-group
- **ecs/**: cluster, task-def, service
- **rds/**: subnet-group, secret, pg-param
- **edge/**: s3 bucket, oac, cloudfront

`main.tf` wires the modules; state is stored in  
`s3`bucket, lock table `dynamodb`.

### 2.2  Key variables
| Name | Description | Example |
| ---- | ----------- | ------- |
| `app_name` | prefix for all AWS resources | `notejam` |
| `container_image` | ECR URI supplied by app pipeline | `…/cpapp:<sha>` |
| `db_password` | Injected from GH Secret | *(not in state, only Secrets Mgr)* |

### 2.3  Module highlights  
* **edge**  
  * `aws_cloudfront_distribution` with two origins: ALB & S3.  
  * OAC policy (SigV4) restricts S3 to CloudFront **only**.  
* **security**  
  * IAM boundary policy blocks wildcard `*:*`.
  * Governs all SG rules; **no 0.0.0.0/0** on task or RDS ports.
* **network**  
  * Route tables: public → IGW, private → NAT (*one per AZ*). 
* **alb**
  * Target group uses **IP mode** (one-task-per-ENI) → fast drain / blue-green ready. 
* **ecs**
  * Task-def imports the **image URI** at deploy time; secrets pulled from Secrets Mgr
* **rds** 
  * Password generated & stored in **AWS Secrets Manager**
  *  Parameter group enforces `rds.force_ssl = 1`; SG only allows traffic from `ecs_sg`.
* **edge**
  * **S3 bucket** is *private*; access granted only via an **Origin Access Control** (SigV4).
  * CloudFront distribution uses two origins (S3 & ALB) with path-based routing (`/static/*`)