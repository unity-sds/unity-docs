---
description: >-
  This page explains the steps to add/remove users, user groups and add/remove
  users to/from user groups.
---

# User and Role Management

## Add and remove users from the MDPS system

The user should be added to the shared services Cognito user pool in the related MCP environment (Unity Dev, Unity Test or Unity Prod).

Before adding a user, the following information should be collected from the user.

* A user ID to be used. o It is recommended to use NASA AUID as the username. o The user can obtain their NASA Agency ID (AUID) from https://idmax.nasa.gov/ . User can click on the link called “Your Identity” and look for AUID.
* NASA/JPL email address belongs to the user.

### Steps to add a new user.

1. Login to the required MCP environment using Kion .
2. Search for Cognito or directly access the Cognito service using https://us-west-2.console.aws.amazon.com/cognito/v2/idp/user-pools?region=us-west-2.
3. Click on the Cognito User Pool (usually this is named as unity-user-pool or a similar name).
4. In the left side panel, click on Users under User Management.
5. Click on the Create User button on the right side of the page.
6. Select the option to “Send an email invitation”.
7. Enter the username collected by the user (it is recommended to use NASA AUID here).
8. Enter the email address collected by the user and select the check box “Mark email address as verified”, if that email is the actual email address of the user.
9. Under the “Temporary Password” select the option to “Generate a password”.
10. Click the ‘Create user” button to create the user.

Above steps will send an email to the user with the instructions to change the password. The user must change the password within 10 days. If not, it is required to delete the user and repeat all above steps.

### Steps to remove an excising user:

1. Login to the required MCP environment using Kion .
2. Search for Cognito or directly access the Cognito service using https://us-west-2.console.aws.amazon.com/cognito/v2/idp/user-pools?region=us-west-2.
3. Click on the Cognito User Pool (usually this is named as unity-user-pool or a similar name).
4. In the left side panel, click on Users under User Management.
5. Search for the user by entering the username or email.
6. When the username is listed, select the radio button near the user.
7. Click the ‘Delete user” button to delete the user.
8. Click “Disable user access” in the next dialog box.
9. Press “Delete” button to complete the action.

## Add and remove roles from the MDPS System

### Steps to add a new user group (user role).

1. Login to the required MCP environment using Kion .
2. Search for Cognito or directly access the Cognito service using https://us-west-2.console.aws.amazon.com/cognito/v2/idp/user-pools?region=us-west-2.
3. Click on the Cognito User Pool (usually this is named as unity-user-pool or a similar name).
4. In the left side panel, click on Groups under User Management.
5. Click on the “Create group” button on the right side of the page.
6. Enter group name and description.
7. The Precedence and IAM role are optional, but can be set based on the use cases
8. Click the ‘Create group” button to create the Cognito user group.

### Steps to remove an excising user group:

1. Login to the required MCP environment using Kion .
2. Search for Cognito or directly access the Cognito service using https://us-west-2.console.aws.amazon.com/cognito/v2/idp/user-pools?region=us-west-2.
3. Click on the Cognito User Pool (usually this is named as unity-user-pool or a similar name).
4. In the left side panel, click on “Groups” under User Management.
5. Search for the group by entering the group name to be deleted.
6. When the group is listed, select the radio button near the Cognito user group.
7. Click the ‘Delete” button to delete the user.

## Add/remove users from roles in the MDPS system

It is necessary to associate Cognito users with Cognito user groups in order to authorize users to perform actions such as accessing websites and RESTful APIs,. The web application or RESTful APIs will check the user’s access tokens to see if the user has required groups to access the application.

### Steps to add a user to a user group.

1. Login to the required MCP environment using Kion .
2. Search for Cognito or directly access the Cognito service using https://us-west-2.console.aws.amazon.com/cognito/v2/idp/user-pools?region=us-west-2.
3. Click on the Cognito User Pool (usually this is named as unity-user-pool or a similar name).
4. In the left side panel, click on Users under User Management.
5. Search for the user by entering the username or email.
6. When the username is listed, click on the username.
7. In the next screen, under the Group memberships, click the button “Add user to a group”.
8. Search for the group name to be added and select the user group by clicking on the radio button near the group name.
9. Press button “Add’ to add the user to the group.
10. Repeat steps 8 and 9 , if it is required to add more groups.

### Steps to remove a user from a user group.

1. Login to the required MCP environment using Kion .
2. Search for Cognito or directly access the Cognito service using https://us-west-2.console.aws.amazon.com/cognito/v2/idp/user-pools?region=us-west-2.
3. Click on the Cognito User Pool (usually this is named as unity-user-pool or a similar name).
4. In the left side panel, click on Users under User Management.
5. Search for the user by entering the username or email.
6. When the username is listed, click on the username.
7. In the next screen, under the Group memberships, search for the group name to be removed.
8. When the user group is listed, select the user group by clicking on the radio button near the group name.
9. Press button “Remove user from group to remove the user from the group.
10. Confirm the decision to remove the user group in next dialog box.
