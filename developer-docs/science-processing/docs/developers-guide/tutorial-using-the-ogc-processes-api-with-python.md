# Tutorial: Using the OGC processes API with Python

The notebook found at this link: [https://github.com/unity-sds/unity-monorepo/blob/294-update-ogc-api-notebook/libs/unity-py/notebooks/ogc\_notebook.ipynb](https://github.com/unity-sds/unity-monorepo/blob/294-update-ogc-api-notebook/libs/unity-py/notebooks/ogc_notebook.ipynb) describes how to use the `Unity-Py` library to interact with the Unity SPS through the OGC Processes API.

**Requirements:**

* Any compute environment: Local or in the cloud
* This tutorial assumes you have deployed an OGC process to the OGC Processes API endpoint but covers briefly how to register (and deregister) a process at the end
* This notebook uses the [Unity-Py](https://github.com/unity-sds/unity-monorepo/tree/main/libs/unity-py) library to interact with the OGC API
* Unity environment (e.g., DEV, TEST, PROD) and venue (e.g., unity-sbg-dev, unity-emit-dev, unity-asips-int) names. See [this documentation](https://unity-sds.gitbook.io/docs/system-docs/architecture/deployments-projects-and-venues/unity-owned-venues) for guidance
* Unity username and password
* A URL for the OGC API processes endpoint, something like: [https://unity-dev-httpd-alb-XXXXXXXXXX.us-west-2.elb.amazonaws.com:1234/unity/dev/ogc/](https://unity-dev-httpd-alb-xxxxxxxxxx.us-west-2.elb.amazonaws.com:1234/unity/dev/ogc/)

**Objectives:**

* List all processes that have been registered at the OGC endpoint URL
* Get details on a single process
* Execute a job
* Monitor a job
* Get all jobs executing for a process
* Delete a job
