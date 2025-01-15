---
description: Procedure for updating a venue deployment
---

# Updating Venue Deployment

1. Prerequsite:  Have a bastion host.  See [instructions here](https://unity-sds.gitbook.io/docs/developer-docs/common-services/docs/users-guide/deployment/deployment-concepts-and-infrastructure/detailed-breakdown-of-project-onboarding-steps), to create one, if one doesn't exist already.
2. Undeploy SPS (due to security group dependencies)
3.  If the `Unity-DW-Application`module is present in the terraform state file, disconnect the U-DS marketplace-deployed module, for example:

    ```sh
    $ terraform state rm module.Unity-DS-Application-tdGjK
    Removed module.Unity-DS-Application-tdGjK.aws_s3_bucket.market_bucket
    Successfully removed 1 resource instance(s).
    ```
4. Destroy Management Console via bastion host.  See [instructions here](https://unity-sds.gitbook.io/docs/developer-docs/common-services/docs/users-guide/deployment/deployment-concepts-and-infrastructure/detailed-breakdown-of-project-onboarding-steps). (run destroy.sh under step 11)
5. Deploy new Management Console via bastion host.   See [instructions here](https://unity-sds.gitbook.io/docs/developer-docs/common-services/docs/users-guide/deployment/deployment-concepts-and-infrastructure/detailed-breakdown-of-project-onboarding-steps).
6. Manually update the shared services API Gateway Integration "Endpoint URL" (see screeenshot below for example). <mark style="color:red;">NOTE: This may be needed until automation that does this, is fixed.</mark>\
   ![](<../../../../.gitbook/assets/Screenshot 2025-01-14 at 6.57.56 PM.png>)
7. SPS Team re-deploys SPS
8.  Re-add U-DS module to terraform state:

    <pre class="language-sh"><code class="lang-sh"><strong>terraform import &#x3C;bucket resource> &#x3C;bucket name>
    </strong></code></pre>
9.  Management Console Access

    1. The Management Console is accessible through the shared services HTTPD:
       * Deployment automatically configures the Apache settings
       * Links your venue's ALB to the shared services HTTPD
       * Access URL format: `https://www.<SS_PREFIX>.mdps.mcp.nasa.gov:4443/${VENUE_PATH}/management/ui`
    2. Environment URLs:
       1. Development: `https://www.dev.mdps.mcp.nasa.gov:4443/unity/dev/management/ui`
       2. Test: `https://www.test.mdps.mcp.nasa.gov:4443/unity/test/management/ui`
       3. Production: `https://www.mdps.mcp.nasa.gov:4443/unity/prod/management/ui`
    3. Notes:
       1. `SS_PREFIX` varies by environment (dev, test, or empty for production)
       2. Configuration is automatically removed during venue destruction
       3.  No manual Apache configuration is required



