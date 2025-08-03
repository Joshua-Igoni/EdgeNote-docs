# 03 · CI/CD pipelines

## 3.1  Application repo (`CPnotejam/.github/workflows/app.yml`)

| Step | Action |
| --- | --- |
| **test** | pytest +Django test runner → fail-fast |
| **build-push** | OIDC-based ECR login → `docker build` → `docker push` |
| **trigger-infra** | `gh workflow run deploy.yml … -f image_uri=$URI` |

*Branch protection:* only `CPassignment` triggers infra.

## 3.2  Infra repo (`CPterraform/.github/workflows/deploy.yml`)

1. **OIDC auth** (`aws-actions/configure-aws-credentials@v4`).  
2. `terraform init / validate / plan / apply`.  
3. **collectstatic** inside the just-built image (no host Python needed).  
4. `aws s3 sync` to bucket → `cloudfront create-invalidation`.  

## 3.3  Secrets / vars

| GitHub | Scope | Usage |
| ------ | ----- | ----- |
| `AWS_ROLE_ARN` | repo secret | assumed by both workflows via OIDC |
| `DB_PASSWORD`  | infra repo secret | passed as `TF_VAR_db_password` |
| `REPO_B_TOKEN` | app repo secret | PAT to trigger infra repo workflow |
| `AWS_REGION`   | org / env variable | `eu-central-1` |

*(No long-lived AWS keys stored anywhere).*