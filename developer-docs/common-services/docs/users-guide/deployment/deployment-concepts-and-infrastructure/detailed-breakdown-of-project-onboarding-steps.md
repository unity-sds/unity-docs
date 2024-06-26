# Detailed Breakdown of Project Onboarding Steps

1\) Project **obtains their own AWS account** (Bring your own Account).

2\) Project **agrees to EC2 conditions** (EULA / FIPS) on their new account

3\) Project **notifies Unity team** that they want to onboard to Unity

4\) **Project lets Unity know what the set of "starter" users are** (name, email address, etc..)

5a) **Unity Team sets up initial set of users/roles** (manually for now):

* Unity team creates Cognito users
  * &#x20;Each user should be created in the Shared Services Cognito user pool
  * The Cognito user naming convention is available at [Cognito User Standards](../../security/cognito-user-standards.md)
  * Unity team collects user names (preferably the NASA AUIDs) and a valid email addresses for  users to create Cognito users
  * After creating the user in the Cognito user pool, the user receives a temporary password through email with instructions to change the password
* Unity team creates Cognito user groups (roles)
  * Each user gets a `Unity-<PROJECT>-<VENUE>-ManagementConsole-ReadOnly` role
  * One or more users get the `Unity-<PROJECT>-<VENUE>-ManagementConsole-Admin` role
  * One user can have more than one Cognito user group
  * The Cognito user group naming convention is available at [Cognito User Group Standards](../../security/cognito-user-group-standards.md)

5b) **Unity team adds project AWS account to shared service Resource Access Manager** (RAM) to enable sharing of SSM parameters. See [shared-services-deployment.md](../shared-services-deployment.md "mention") for more instructions.

5c) **In Project Venue Account:** **Unity team requests** wildcard cert for the venue, must add the cname record/value to the SHARED SERVICES DNS to approve its creation.

6\) **Unity Team sets up roles on account**:

* Export short-term access keys for account on command-line
* execute the [create roles script](https://github.com/unity-sds/unity-cs-infra/blob/main/aws\_role\_create/create\_roles\_and\_policies.sh)

7\) **Unity Teams sets up the venue bastion host**

* Creates an EC2 bastion host in project AWS account, which is able to deploy Management Console EC2.
  * Create EC2 instance with the following configuration:
    * a `t2.micro` instance with the AMI specified in the `/mcp/amis/ubuntu2004-cset` SSM param
    * select keypair to use (create a new one and save it for future use)
    * select a standard security group that gives access on port 22.  Lock down to JPL source IPs
    * Make sure to put it in a public subnet (under the VPC setting)
    * Under Advanced, select an IAM Instance Profile of `Unity-CS_Service_Role-instance-profile`
    * launch instance
  * Connect to instance
  * `sudo su - ubuntu`
  * `git clone https://github.com/unity-sds/unity-cs-infra.git`
  * Back in the AWS console, create an image (AMI) from the EC2, to have as a backup.

8\) **Unity Team (or Project Team) deploys the Management Console**

* connect to instance via SSM connection (or SSH via pem file)
* `sudo su - ubuntu`
* `cd unity-cs-infra/nightly_tests`
* `git pull`
* `./run.sh --destroy false --run-tests false --project-name <PROJECT> --venue-name <VENUE>`
* Make sure to copy the URL of the Management Console that gets printed to the console, as part of running the above command.  If any issues or errors encountered, see below "Debugging Management Console" section.

OPTIONAL STEPS IF YOU NEED TO DESTROY MANAGEMENT CONSOLE:

* Run the following on the bastion host:
  * `./destroy.sh --project-name <PROJECT> --venue-name <VENUE>`

9\) **Project User** (the one that has the `Unity-<PROJECT>-<VENUE>-ManagementConsole-Admin` role) **logs into Management Console** via CloudFront.   Use the URL that was copied above.

10\) **Project user runs Core Setup actions in Management Console**

11\) **Project User deploys further marketplace items**

**11-1) Project User deploys Venue HTTPD**

**11-2) Project User deploys Venue API Gateway**

**11-3) Project User notifies U-CS team of HTTP location and API Gateway URL for inclusion in shared service API  and HTTPD proxies**



## Debugging Issues with the Management Console

1\) SSH into Management Console EC2 instance

2\) `cd /var/log/`

3\) `cat managementconsole.log`
