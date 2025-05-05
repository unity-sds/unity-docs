---
description: >-
  This page covers how to fetch Amazon Cognito authentication tokens for
  interaction with the SPS OGC and Airflow APIs.
---

# Tutorial: Fetching Cognito Authentication Tokens

These tokens are venue-specific, and have an expiration time of about an hour (3600s).

#### Requirements

* Unity username and password
* The appropriate Cognito Client ID for the venue
  * This can be fetched from (in the AWS Console): **Amazon Cognito** -> User Pools -> `unity-user-pool` -> Applications (App Clients), and finding an App Client that doesn't have a _client secret_
    * Ex. in the Unity-Dev venue, use the `unity-human-to-app-client`'s client id

#### Objectives

* Fetch a Cognito Access token (and store it in an environment variable)

#### Step 1:  Fetch Cognito Token

```bash
$ python unity-sps/utils/cognito-token-fetch.py -u <UNITY_USERNAME> -c <CLIENT_ID>
```

This will prompt for a password or a password can be provided on the command line with the `-p/--password` argument.

Upon execution, the script should return a string, resembling:

```
Authorization: Bearer eyJ[...]
```

Which is the Authentication access token. You can instead save this to an environment variable for limited reuse (though the token will eventually expire).

```
export token="$(python unity-sps/utils/cognito-token-fetch.py -u <UNITY_USERNAME> -c <CLIENT_ID>)"
```

#### Step 2: Using the Access Token

The token fetched in step 1 can be added on as an authorization header to most `curl` commands using the `-H` flag:

```
curl -X GET -H "Authorization: Bearer eyJ[...]" "${url}/ogc/api/processes"
```

```
curl -X GET -H "$(python unity-sps/utils/cognito-token-fetch.py -u <UNITY_USERNAME> -c <CLIENT_ID>)" "${url}/ogc/api/processes"
```

Though it is much easier to include it when stored in an environment variable:

```
curl -X GET -H "${token}" "${url}/ogc/api/processes"
```
