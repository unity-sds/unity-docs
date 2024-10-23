---
description: >-
  Periodically this server may need to be rebuilt, due to AMI deprecations, or
  other factors.
---

# Rebuild of Shared Services HTTPD Proxy

1. Create new EC2 Security group (reference ALB security group)
2. Create new shared-services-httpd EC2 instance
3. Populate proxy config from S3 bucket
4. Restart proxy service
5. Add new EC2 as a target to the ALB Target Group (group will now have 2 targets)
6. Remove old EC2 as a target from ALB Target Group (now there's only 1 target)
7. Destroy old shared-services-httpd EC2
