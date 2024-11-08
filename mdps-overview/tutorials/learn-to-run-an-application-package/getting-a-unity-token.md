---
description: >-
  A description on how to retrieve an authorization token using
  unity-sds-client.
---

# Getting a Unity Token

In order to authorize some actions in the Unity system, you need to provide an access token. This is needed when using certain APIs and when authorizing the STAC Browser UI to view data files. Over time we expect these instances to reduce for most common interactions with the system.

Use the following Python code to get a token that you can paste into API calls or into STAC Browser when it requests you token. It uses the `unity-sds-client` library.

```
import requests

from unity_sds_client.unity import Unity
from unity_sds_client.unity import UnityEnvironments

s = Unity(UnityEnvironments.TEST)
token = s._session.get_auth().get_token()

print("Here's your token: " + token)
```

Also see: [https://github.com/unity-sds/unity-example-application/blob/main/retrieve\_Unity\_Token.ipynb](https://github.com/unity-sds/unity-example-application/blob/main/retrieve\_Unity\_Token.ipynb)
