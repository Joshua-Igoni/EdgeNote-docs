# Static Assets Pipeline

| Step | Responsibility |
|------|----------------|
| 1 | `collectstatic` inside the just-built Docker image during infra job. |
| 2 | `aws s3 sync …` into private bucket (`notejam-static-<acct>`). |
| 3 | CloudFront invalidation on `/static/*`. |

Bucket policy + OAC snippet lives in **`modules/edge/`** — prevents public read.
