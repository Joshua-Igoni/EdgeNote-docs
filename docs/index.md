# Notejam - Platform Engineering Assignment

|            | |
|------------|---------------------------------------|
| **Code**   | [App](https://github.com/your-org/notejam-app) · [Terraform](https://github.com/your-org/notejam-infra) |
| **Region** | `eu-central-1` |
| **Edge**   | CloudFront → ALB (private sub-nets) |
| **Runtime**| Fargate (ECS) · Gunicorn + Django |
| **Data**   | Aurora PostgreSQL (Multi-AZ) |
| **Static** | S3 (+ Origin Access Control) |

> This site documents *architecture, deployment workflow, and day-2 operations* for the Notejam demo platform.
>
> **Audience:** Reviewer(s), new team members, DevOps/SREs.

**Jump to:** [CI / CD](ci-cd.md) · [Module reference](infra-modules.md)