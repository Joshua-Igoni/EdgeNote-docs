# Operations, Monitoring & Observability

| Domain | Implementation |
| ------ | -------------- |
| **Logs** | *ALB*: S3 access logs (`s3://notejam-alb-logs/YY/MM/DD`).<br>*ECS tasks*: CloudWatch Logs group `/ecs/notejam`. |
| **Metrics** | Default ECS service & RDS metrics + custom CloudWatch Alarms (CPU > 80 %, RDS free-storage < 10 %). |
| **Tracing** | X-Ray SDK baked into the image, disabled by env `XRAY_ENABLED=false`. |
| **Backups** | RDS automated backups 7 days, S3 versioning (+ lifecycle → GLACIER after 30 days). |
| **Patch / upgrade** | Containers are immutable → redeploy.<br>RDS minor version via `preferred_maintenance_window`. |
| **Scaling** | ECS Service Auto Scaling: CPU 50 % target (min = 3 tasks, max = 15). |