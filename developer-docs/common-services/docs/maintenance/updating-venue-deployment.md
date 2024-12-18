---
description: Procedure for updating a venue deployment
---

# Updating Venue Deployment

1. Prerequsite:  Have a bastion host.  See [instructions here](https://unity-sds.gitbook.io/docs/developer-docs/common-services/docs/users-guide/deployment/deployment-concepts-and-infrastructure/detailed-breakdown-of-project-onboarding-steps), to create one, if one doesn't exist already.
2. Undeploy SPS
3.  Disconnect the U-DS marketplace-deployed module, for example:

    ```sh
    $ terraform state rm module.Unity-DS-Application-tdGjK
    Removed module.Unity-DS-Application-tdGjK.aws_s3_bucket.market_bucket
    Successfully removed 1 resource instance(s).
    ```
4. Destroy Management Console via bastion host.  See [instructions here](https://unity-sds.gitbook.io/docs/developer-docs/common-services/docs/users-guide/deployment/deployment-concepts-and-infrastructure/detailed-breakdown-of-project-onboarding-steps).
5. Deploy new Management Console via bastion host.   See [instructions here](https://unity-sds.gitbook.io/docs/developer-docs/common-services/docs/users-guide/deployment/deployment-concepts-and-infrastructure/detailed-breakdown-of-project-onboarding-steps).
6.  Re-add U-DS module to terraform state:

    ```sh
    terraform import <bucket resource> <bucket name>
    ```
7.  Management Console Access

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



