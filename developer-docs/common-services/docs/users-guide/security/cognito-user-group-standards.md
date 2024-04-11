# Cognito User Group Standards

Cognito User Groups can be created in a Cognito user pool. These Cognito User Groups are also used as User Roles, to conduct authorization.

* A single User Group can contain multiple Cognito Users.
* A single Cognito User can be in multiple Cognito User Groups

<figure><img src="../../../../../.gitbook/assets/Screenshot 2023-09-06 at 10.14.40 AM.png" alt=""><figcaption><p>Example Cognito User Groups and Users</p></figcaption></figure>

To create a Cognito User Group, at least following attributes are required. A User Group can be created in AWS Console (\[Amazon Cognito] -> \[User pools] -> \[unity-user-pool]-> Create group).

* **Group name**
  * The name of the group. Must be unique within the Cognito User Pool.
  * A string that contains between 1 and 128 non-space characters.
  *   Must follow the below naming convention (The words between hyphens such as ProjectName should be camel cased)

      ```
         Unity-<ProjectName>-<Venue>-<ApplicationName>-<Role>
      ```

      E.g.:

      ```
         Unity-ProjectA-Dev-App01-Admin

         Unity-ProjectB-Test-App02-ReadOnly
      ```
* **Description**
  * The group description must be 2048 characters or fewer.

**Example Scenario (UI Navbar):**

<figure><img src="../../../../../.gitbook/assets/Screenshot 2024-04-11 at 3.12.03 PM.png" alt="" width="375"><figcaption></figcaption></figure>

* User : Peter, on the SIPS Team
* Access : Peter has access to the Airflow and Jupyter
* So the Cognito User "Peter"  cloud have groups:
  * ```
    Unity-SIPS-Dev-Airflow-Admin
    Unity-SIPS-Dev-Jupyter-Access
    ```
