---
description: >-
  The Unity venue bastion host is an EC2 instance that serves as a starting
  point for deploying and managing things inside of a venue AWS account.
---

# Creating a Bastion Host

{% hint style="info" %}
**PREREQUISITE**:  The MDPS [venue roles and policies](https://github.com/unity-sds/unity-cs-infra/blob/main/aws_role_create/create_roles_and_policies.sh) must be created before following the below procedure.
{% endhint %}

### Procedure:

* Creates an EC2 bastion host in project AWS account, which is able to deploy Management Console EC2.
* Create EC2 instance with the following configuration:
  * **Name of instance:**
    * Use the format:
      * &#x20;`unity-`<mark style="color:blue;">`<PROJECT>`</mark>`-`<mark style="color:green;">`<VENUE>`</mark>`-cs-management_console-bastion`
  * **AMI / instance type**:&#x20;
    * Get the AMI ID to use, by opening another tab, going to "Systems Manager -> Parameter Store", searching for the `/mcp/amis/ubuntu2004-cset` parameter, and copying the value in that parameter.
    * Go to "My AMIs" --> "Shared With Me", click in the AMI box, and paste in the AMI ID in the drop-down text box
    * use a `t2.micro` instance
  * **Key Pair:**&#x20;
    * If a key pair doesn't already exist, create one in the format `unity-`<mark style="color:blue;">`<PROJECT>`</mark>`-`<mark style="color:green;">`<VENUE>`</mark>`-bastion-pem` (do this in another tab first)
    * select keypair (use "Select Existing Keypair") to use (create a new one and save it for future use)
  * **Networking:**
    * Make sure to select a public subnet (under the VPC setting)
  * **Security Group:**&#x20;
    * If an existing `mc-bastion-sg` security doesn't already exist, then create one. It should have:
      * INCOMING CONNECTIONS:
        * none
      * OUTGOING CONNECTIONS:
        * open custom TCP for 443 to anywhere, and 80 to anywhere
    * Select the `mc-bastion-sg` security group.
  * Under "Advanced Details", select an IAM Instance Profile of `Unity-CS_Service_Role-instance-profile`
  * launch instance
    * NOTE: if this is the first time deploying to this AWS account, you may need to click on the error link and subscript/accept the Ubuntu Pro FIPS 20.04 LTS agreement, then click re-try on the launch instance.
  * Connect to instance
  * `sudo su - ubuntu`
  * `git clone https://github.com/unity-sds/unity-monorepo.git`
  * \[OPTIONAL STEP]  Back in the AWS console, create an image (AMI) from the EC2, to have as a backup.
