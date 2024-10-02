---
description: >-
  New users (projects) of MDPS need to have their AWS account setup, users
  setup, and linked together with a MDPS shared services account.  This
  procedure outlines the steps needed to accomplish this.
---

# Project Onboarding Procedure

{% hint style="info" %}
**PREREQUISITE**:  The [Shared Services account must be setup](../developer-docs/common-services/docs/users-guide/deployment/shared-services-deployment.md) before following the below procedure.
{% endhint %}

### Venue Account Onboarding Procedure:

1. <mark style="color:orange;">**Project**</mark> **obtains their own AWS account** (Bring your own Account).\
   &#x20;
2. <mark style="color:orange;">**Project**</mark> **agrees to EC2 conditions** (EULA / FIPS) on their new account\
   &#x20;&#x20;
3.  <mark style="color:orange;">**Project**</mark> **notifies **<mark style="color:purple;">**MDPS Team**</mark> that they want to onboard to MDPS by sending an email to `mdps-support@jpl.nasa.gov` The email should include the following information:

    * Project Name (e.g. `Sounder SIPS`)
    * <mark style="color:blue;">Project Identifier</mark> short string (e.g. <mark style="color:blue;">`ssips`</mark>)
    * Project Description
    * Venue Name (e.g. `Sounder SIPS Development Venue`)
    * <mark style="color:green;">Venue Identifier</mark> (e.g. <mark style="color:green;">`DEV`</mark>, `TEST`, `UAT`, `PROD`, etc..)
    * Venue Purpose
    * Existing AWS Account ID
    * NASA ID (Agency User ID), of initial MDPS user
    * Email address of initial MDPS user
    * Set of other users, if known (NASA AUID & email addresses for all)

    &#x20; &#x20;
4. <mark style="color:orange;">**Project**</mark> **waits for notification from the **<mark style="color:purple;">**MDPS Team**</mark> that everything is setup and ready to use (see step 14 below)\
   &#x20;
5.  Per the information (email address & AUID) in the email sent in step 3, the <mark style="color:purple;">**MDPS Team**</mark>** sets up initial set of users in the Shared Services AWS account Cognito user pool**:

    * Each user should be assigned these Groups at a minimum:
      * `Unity_Viewer`
    * The Cognito user naming convention is available at [Cognito User Standards](../developer-docs/common-services/docs/users-guide/security/cognito-user-standards.md)
    * After creating the user in the Cognito user pool, the <mark style="color:orange;">**Project User**</mark> receives a temporary password through email with instructions to change the password

    &#x20;&#x20;
6.  _**\[NOTE THIS STEP IS OPTIONAL FOR NOW, AND CAN BE SKIPPED]**_ \
    In the Shared Services account, the <mark style="color:purple;">**MDPS Team**</mark> needs to create a set of **Cognito project/venue-specific user groups** (roles):

    * `Unity-<PROJECT>-<VENUE>-ManagementConsole-ReadOnly`
    * `Unity-<PROJECT>-<VENUE>-ManagementConsole-Admin`
    * `Unity-<PROJECT>-<VENUE>-viewer`
    * NOTE: The Cognito user group naming conventions can be viewed in [Cognito User Group Standards](../developer-docs/common-services/docs/users-guide/security/cognito-user-group-standards.md)

    &#x20;&#x20;
7. <mark style="color:purple;">**MDPS Team**</mark>** adds project AWS account to shared service Resource Access Manager** (RAM) to enable sharing of SSM parameters. See [shared-services-deployment.md](../developer-docs/common-services/docs/users-guide/deployment/shared-services-deployment.md "mention") for more instructions.\
   &#x20;
8. <mark style="color:purple;">**MDPS Team**</mark>** requests wildcard cert In Project Venue Account.**
   * must add the cname record/value to the SHARED SERVICES DNS to approve its creation. [See instructions here](https://app.gitbook.com/s/cUYkPD7kBe7iT1LABkPZ/tips-and-tricks/speed-up-with-quick-find).\
     &#x20;&#x20;
9.  <mark style="color:purple;">**MDPS Team**</mark>** sets up roles on account**:

    * Export short-term access keys for account on command-line
    * execute the [create roles script](https://github.com/unity-sds/unity-cs-infra/blob/main/aws\_role\_create/create\_roles\_and\_policies.sh)


10. <mark style="color:purple;">**MDPS Team**</mark>** sets up the venue bastion host**

    * Follow the [instructions here](https://unity-sds.gitbook.io/docs/developer-docs/common-services/docs/users-guide/deployment/creating-a-bastion-host) to create a bastion host, if one does not already exist.

    &#x20;
11. <mark style="color:purple;">**MDPS Team**</mark>** deploys the Venue Infrastructure (Networking stack, Management Console, and more)**
    * connect to instance via AWS SSM connection
    * `sudo su - ubuntu`
    * `cd unity-cs-infra/nightly_tests ; git pull`
    * `./run.sh --destroy false --run-tests false --project-name PROJECT --venue-name VENUE`
    * NOTE: If this is the first time deploying to this AWS account, you may have to manually enter a few SSM parameters such as:
      * `/unity/cs/github/username`
      * `/unity/cs/github/useremail`
      * `/unity/cs/githubtoken`
      * `/unity/ci/slack-web-hook-url`
      * PLEASE CONSULT THE MDPS U-CS Team to determine what values to use, if you are unsure.
    * Make sure to copy the URL of the Management Console that gets printed to the console, as part of running the above command.  If any issues or errors encountered, see below "Debugging Management Console" section.
    * **OPTIONAL STEPS IF YOU NEED TO DESTROY the Venue Infrastructure:**
      * Run the following on the bastion host:
      * `./destroy.sh --project-name`` `<mark style="color:blue;">`<PROJECT>`</mark>` ``--venue-name`` `<mark style="color:green;">`<VENUE>`</mark>
      * NOTE: the S3 bucket holding the terraform state files will not be deleted via the `destroy.sh` script.  It would be available for re-use next time around, if you deploy using the same \<PROJECT> and \<VENUE>. \
        &#x20;
12. <mark style="color:purple;">**MDPS Team**</mark>** configures Shared Services HTTPD server to route to Venue "entry" ALB.**
    * Perform steps 4 & 5 [here](https://unity-sds.gitbook.io/docs/developer-docs/common-services/docs/users-guide/deployment/updating-venue-deployment).\
      &#x20;
13. <mark style="color:purple;">**MDPS Team**</mark>** runs Core Setup actions in Management Console**\
    &#x20;&#x20;
14. <mark style="color:purple;">**MDPS Team**</mark> reaches out to <mark style="color:orange;">**Project**</mark>, to notify them that their account is ready for use.
    * URL(s) and instructions to log into services is provided to Project Team.\
      &#x20;  &#x20;
15. <mark style="color:orange;">**Project Users**</mark> [log into MDPS](https://unity-sds.gitbook.io/docs/mdps-overview/unity-account-and-login) using the URLs provided, and do work.  For example:
    * <mark style="color:orange;">**Project Algorithm Developer**</mark> logs into JuptyerHub and creates/tests algorithms
    * <mark style="color:orange;">**Project Administrator**</mark> logs into Management Console, and installs/updates MDPS services
    * <mark style="color:orange;">**Project Workflow Engineer**</mark> logs into SPS/Airflow and edits DAG code
    * etc..

&#x20;



## Debugging Issues with the Management Console

1\) SSH into Management Console EC2 instance

2\) `cd /var/log/`

3\) `cat managementconsole.log`
