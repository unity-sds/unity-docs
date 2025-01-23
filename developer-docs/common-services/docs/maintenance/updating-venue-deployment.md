---
description: Procedure for updating a venue deployment
---

# Updating Venue Deployment

1. **Prerequisite**:  Have a EC2 bastion host on the venue account you wish to update.  See [instructions here](https://unity-sds.gitbook.io/docs/developer-docs/common-services/docs/users-guide/deployment/deployment-concepts-and-infrastructure/detailed-breakdown-of-project-onboarding-steps), to create one, if one doesn't exist alread&#x79;**.**
2. **Un-deploy SPS** (due to security group dependencies).
   1. Make sure SPS also un-deploys the EKS component.
3.  Log into the Management Console, and check what is deployed currently (e.g. the "Application Management" page).  If the `Unity-DS-Application`module is deployed, and present in the terraform state file, disconnect the U-DS marketplace-deployed module, for example:

    1. mkdir temp ; cd temp
    2. create a file called unity-dev.tf:

    \`\`\`\
    terraform { backend "s3" { bucket = "unity-unity-dev-bucket" key = "unity-dev-tfstate" region = "us-west-2" dynamodb\_table = "unity-dev-terraform-state" } }\
    \`\`\`

    1. run \`terraform init\`
    2. OPTIONALLY  run \`terraform state list\`
    3.  run \`terraform state rm \<MODULE>\`, for example:

        ```sh
        $ terraform state rm module.Unity-DS-Application-tdGjK
        Removed module.Unity-DS-Application-tdGjK.aws_s3_bucket.market_bucket
        Successfully removed 1 resource instance(s).
        ```
4. Destroy Management Console via bastion host.  See [instructions here](https://unity-sds.gitbook.io/docs/developer-docs/common-services/docs/users-guide/deployment/deployment-concepts-and-infrastructure/detailed-breakdown-of-project-onboarding-steps). (execute the `destroy.sh`script under step 11)
5. Deploy new Management Console via bastion host.   See [instructions here](https://unity-sds.gitbook.io/docs/developer-docs/common-services/docs/users-guide/deployment/deployment-concepts-and-infrastructure/detailed-breakdown-of-project-onboarding-steps). (execute the `run.sh` script)
6. Open the Management Console in a browser, go to the "Core Management" page, click "Save", then wait 1 minute.
7.  Manually update the shared services API Gateway Integration "Endpoint URL" to be the API Gateway URL from the venue deployment (see screeenshot below for example). <mark style="color:red;">NOTE: This may be needed until automation that does this, is fixed.</mark>\


    <figure><img src="../../../../.gitbook/assets/Screenshot 2025-01-14 at 6.57.56 PM.png" alt=""><figcaption></figcaption></figure>
8. After saving the above Endpoint URL configuration, manually re-deploy the API no shared services.
9. Go to the venue AWS console, and also redeploy the \`dev\` stage API Gateway.  Sometimes it takes a few minutes to really take effect, but at this point you should be able to see that the UI Dashboard is returning results from the Health Check API.
10. SPS Team re-deploys SPS
11. Re-add U-DS module to terraform state:

    <pre class="language-sh"><code class="lang-sh"><strong>terraform import &#x3C;bucket resource> &#x3C;bucket name>
    </strong></code></pre>



## <mark style="color:orange;">Steps to Verify a Successful Deployment:</mark>

1.  Verify the  Management Console is accessible through the shared services HTTPD:\
    `https://www.<SS_PREFIX>.mdps.mcp.nasa.gov:4443/${VENUE_PATH}/management/ui`

    **Development**: [`https://www.dev.mdps.mcp.nasa.gov:4443/unity/dev/management/ui`](https://www.dev.mdps.mcp.nasa.gov:4443/unity/dev/management/ui)&#x20;

    **Test**: [`https://www.test.mdps.mcp.nasa.gov:4443/unity/test/management/ui`](https://www.test.mdps.mcp.nasa.gov:4443/unity/test/management/ui)

    **Production**: [`https://www.mdps.mcp.nasa.gov:4443/unity/ops/management/ui`](https://www.mdps.mcp.nasa.gov:4443/unity/ops/management/ui)\

2.  Verify you can access the UI Dashboard\
    **Development**: \
    [`https://www.dev.mdps.mcp.nasa.gov:4443/unity/dev/ui/health-dashboard`](https://www.dev.mdps.mcp.nasa.gov:4443/unity/dev/ui/health-dashboard)&#x20;

    **Test**: \
    [`https://www.test.mdps.mcp.nasa.gov:4443/unity/test/ui/health-dashboard`](https://www.test.mdps.mcp.nasa.gov:4443/unity/test/ui/health-dashboard)

    **Production**: \
    [`https://www.mdps.mcp.nasa.gov:4443/unity/ops/ui/health-dashboard`](https://www.mdps.mcp.nasa.gov:4443/unity/ops/ui/health-dashboard)\

3. Verify the UI Dashboard is able to access the HealthCheck API, and is displaying statuses for each application.



