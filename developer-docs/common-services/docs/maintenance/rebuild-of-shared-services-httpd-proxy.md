---
description: >-
  Periodically this server may need to be rebuilt, due to AMI deprecations, or
  other factors.
---

# Rebuild of Shared Services HTTPD Proxy

1. TERRAFORM APPLY:  Create new EC2 Security group (reference ALB security group)
2. TERRAFORM APPLY:  Create new shared-services-httpd EC2 instance
3. SCRIPT: (USER DATA) : install Apache2 stuff, crontab, etc..
4. SCRIPT:  Update apache2 config:
   1. Populate proxy config from S3 bucket
   2. Restart proxy service
5. SCRIPT:  Add new EC2 (created in step 1) as a target to the existing ALB Target Group (group will now have 2 targets)
6. SCRIPT:  Remove old EC2 as a target from ALB Target Group (now there's only 1 target)
7. TERRAFORM DESTROY:  Destroy security group
8. TERRAFORM DESTROY:  Destroy old shared-services-httpd EC2







