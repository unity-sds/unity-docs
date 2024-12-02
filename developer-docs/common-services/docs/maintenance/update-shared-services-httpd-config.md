---
description: >-
  Guide for manually updating the shared services Apache configuration file
  through the bastion host and S3 bucket, with automatic validation and
  deployment.
---

# Update Shared Services HTTPD Config

&#x20;**Steps to Update Configuration:**

1. Connect to Bastion Host:
   * Log into appropriate AWS account (unity-venue-dev, unity-venue-test, or unity-venue-ops)
   * Connect to management\_console-bastion EC2 instance via AWS Console
     * `sudo su - ubuntu`
2. Download Current Config:
   1. <pre><code><strong>aws s3 cp s3://ucs-shared-services-apache-config-&#x3C;ENV>/unity-cs.conf .
      </strong></code></pre>
   2. _Note: Replace \<ENV>with: dev, test, or ops based on your environment_
3. Edit Configuration:
   1. <pre class="language-bash"><code class="lang-bash"><strong>vim unity-cs.conf
      </strong></code></pre>
4. Upload Modified Config:
   1. ```
      aws s3 cp unity-cs.conf s3://ucs-shared-services-apache-config-dev/unity-cs.conf
      ```
   2. _Note: Use same \<ENV>  as download step_

* Verification:
  * Changes are automatically applied within 1 minute
  * Check `#unity-cs-notification` Slack channel for:
    * Success confirmation
      * Any syntax errors in your changes
      * Apache reload status
* Note:
  * Always verify your changes are syntactically correct before uploading
  * The cron job will report any configuration errors in Slack
  * Wait at least one minute for changes to take effect
