# Tutorial: Using the OGC Processes API with CURL

This page describes how a client (for example, a human or a program) can use the popular CURL tool to interact with the Unity SPS through the OGC Processes API to register a new science algorithm and request its execution. We will be assuming that the SPS system is already deployed and the OGC Processes API is available at some URL of the form:&#x20;

* OGC\_PROCESSES\_API=https://unity-dev-httpd-alb-XXXXXXXXXX.us-west-2.elb.amazonaws.com:1234/unity/dev/ogc/

Let's first consider the case when the desired science algorithm has already been registered with the system. For example, each SPS deployment includes making available the CWL DAG ("Common Workflow Language" "Direct Acyclic Graph"), which can be used to execute any CWL workflow to invoke a generic science algorithm packaged as a Docker container. Later we will show how such a DAG can be registered in the first place.

Step 1a (optional): List the available processes



<details>

<summary>Request</summary>

curl -k -X GET "${OGC\_PROCESSES\_API}/processes" | jq

</details>

<details>

<summary>Response</summary>

{

&#x20; "processes": \[

&#x20;   {

&#x20;     "title": "Generic CWL Process",

&#x20;     "description": "This process executes any CWL workflow.",

&#x20;     "keywords": null,

&#x20;     "metadata": null,

&#x20;     "id": "cwl\_dag",

&#x20;     "version": "1.0.0",

&#x20;     "jobControlOptions": \[

&#x20;       "async-execute"

&#x20;     ],

&#x20;     "links": null

&#x20;   },

&#x20;   {

&#x20;     "title": "Karpenter Test Process",

&#x20;     "description": "This process tests Karpenter node provisioning with different instance types.",

&#x20;     "keywords": null,

&#x20;     "metadata": null,

&#x20;     "id": "karpenter\_test",

&#x20;     "version": "1.0.0",

&#x20;     "jobControlOptions": \[

&#x20;       "async-execute"

&#x20;     ],

&#x20;     "links": null

&#x20;   },

&#x20;   {

&#x20;     "title": "SBG Preprocess CWL Workflow",

&#x20;     "description": "This process executes the SBG Preprocess Workflow using CWL.",

&#x20;     "keywords": null,

&#x20;     "metadata": null,

&#x20;     "id": "sbg\_preprocess\_cwl\_dag",

&#x20;     "version": "1.0.0",

&#x20;     "jobControlOptions": \[

&#x20;       "async-execute"

&#x20;     ],

&#x20;     "links": null

&#x20;   }

&#x20; ],

&#x20; "links": \[]

}

</details>

You will notice that the server response includes a process with identifier "cwl\_dag", which we will want to trigger next.

Step 2a: Execute a process (i.e. create a job)

<details>

<summary>Request</summary>

&#x20;curl -s -X POST "${OGC\_PROCESSES\_API}/processes/cwl\_dag/execution" \\

-H "Content-Type: application/json" \\

-H "Prefer: respond-async" \\

\--data-binary @- << EOF | jq '.'

{

&#x20; "inputs": {

&#x20;   "cwl\_workflow": "https://raw.githubusercontent.com/unity-sds/sbg-workflows/main/preprocess/sbg-preprocess-workflow.cwl",

&#x20;   "cwl\_args": "https://raw.githubusercontent.com/unity-sds/sbg-workflows/main/preprocess/sbg-preprocess-workflow.dev.yml",

&#x20;   "request\_instance\_type": "r7i.xlarge",

&#x20;   "request\_storage": "10Gi"&#x20;

&#x20; },

&#x20; "outputs": {

&#x20;   "result": {

&#x20;     "transmissionMode": "reference"

&#x20;   }

&#x20; }

}

EOF

</details>

<details>

<summary>Response</summary>

\[

&#x20; \[

&#x20;   "process\_id",

&#x20;   "cwl\_dag"

&#x20; ],

&#x20; \[

&#x20;   "type",

&#x20;   "process"

&#x20; ],

&#x20; \[

&#x20;   "job\_id",

&#x20;   "7b1173ff-137e-41fa-bfb1-bd133049b4a3"

&#x20; ],

&#x20; \[

&#x20;   "status",

&#x20;   "accepted"

&#x20; ],

&#x20; \[

&#x20;   "message",

&#x20;   null

&#x20; ],

&#x20; \[

&#x20;   "exception",

&#x20;   null

&#x20; ],

&#x20; \[

&#x20;   "created",

&#x20;   "2025-01-29T20:33:05.521710"

&#x20; ],

&#x20; \[

&#x20;   "started",

&#x20;   null

&#x20; ],

&#x20; \[

&#x20;   "finished",

&#x20;   null

&#x20; ],

&#x20; \[

&#x20;   "updated",

&#x20;   "2025-01-29T20:33:05.521715"

&#x20; ],

&#x20; \[

&#x20;   "progress",

&#x20;   null

&#x20; ],

&#x20; \[

&#x20;   "links",

&#x20;   null

&#x20; ]

]

</details>

Note that the response contains the job id ("7b1173ff-137e-41fa-bfb1-bd133049b4a3") and its status ("accepted").&#x20;

Step 3a: Monitor the job

You can use the job id returned in the previous step to monitor the job until it completes.

<details>

<summary>Request</summary>

JOB\_ID=7b1173ff-137e-41fa-bfb1-bd133049b4a3

curl "${OGC\_PROCESSES\_API}/jobs/${JOB\_ID}"

</details>

<details>

<summary>Response</summary>

{

&#x20; "processID": "cwl\_dag",

&#x20; "type": "process",

&#x20; "jobID": "7b1173ff-137e-41fa-bfb1-bd133049b4a3",

&#x20; "status": "running",

&#x20; "message": null,

&#x20; "exception": null,

&#x20; "created": "2025-01-29T20:33:05.521710Z",

&#x20; "started": null,

&#x20; "finished": null,

&#x20; "updated": "2025-01-29T20:41:47.598612Z",

&#x20; "progress": null,

&#x20; "links": null

}

</details>

Or to keep executing the command every few seconds:

<details>

<summary>Request</summary>

watch -n 5 "curl -s "${OGC\_PROCESSES\_API}/jobs/${JOB\_ID}" | jq"

</details>

<details>

<summary>Response</summary>

{

&#x20; "processID": "cwl\_dag",

&#x20; "type": "process",

&#x20; "jobID": "7b1173ff-137e-41fa-bfb1-bd133049b4a3",

&#x20; "status": "successful",

&#x20; "message": null,

&#x20; "exception": null,

&#x20; "created": "2025-01-29T20:33:05.521710Z",

&#x20; "started": null,

&#x20; "finished": "2025-01-29T20:48:11.943940Z",

&#x20; "updated": "2025-01-29T20:58:31.094170Z",

&#x20; "progress": null,

&#x20; "links": null

}

</details>

***

Let's now demonstrate how a client would make a request to register a new science algorithm, encoded as an Airflow DAG. The DAG (a Python program) needs to be checked in within the GitHub repository that was configured during the SPS deployment. In other words, the DAG author will check the latest version of the code into GitHub in the specified folder, and then they can make an HTTP request to the OGC API to register that process for execution.&#x20;

Let's consider the following DAG: [https://github.com/unity-sds/unity-sps/blob/develop/airflow/dags/cwl\_dag.py](https://github.com/unity-sds/unity-sps/blob/develop/airflow/dags/cwl_dag.py)

We will assume that the SPS deployment has been configured to monitor the GitHub repository [https://github.com/unity-sds/unity-sps](https://github.com/unity-sds/unity-sps) at the path "airflow/dags" in the brach "main".

**Step 1b: Register a process**

<details>

<summary>Request</summary>

curl -k -v -X POST -H "Expect:" -H "Content-Type: application/json; charset=utf-8" --data-binary @"./cwl\_dag.json" "${OGC\_PROCESSES\_API}/processes"

</details>

<details>

<summary>Response</summary>

< HTTP/1.1 201 Created

< Date: Thu, 30 Jan 2025 15:06:26 GMT

< Content-Length: 37

< Connection: keep-alive

< Server: uvicorn

Process cwl\_dag deployed successfully%       &#x20;

</details>

Note that the HTTP request contains the dag id "cwl\_dag". This id _must_ match the filename of the DAG in the GitHub repository, at the specified path and branch, without the ".py" extension.

<details>

<summary>Response</summary>

< HTTP/1.1 201 Created

< Date: Thu, 30 Jan 2025 11:55:45 GMT

< Content-Length: 37

< Connection: keep-alive

< Server: uvicorn



Process cwl\_dag deployed successfully%                &#x20;

</details>

Step 2b: Unregister a process

<details>

<summary>Request</summary>

curl -kv -X DELETE -H "Content-Type: application/json; charset=utf-8" "${OGC\_PROCESSES\_API}/processes/cwl\_dag"

</details>

<details>

<summary>Response</summary>

< HTTP/1.1 204 No Content

< Date: Thu, 30 Jan 2025 15:05:34 GMT

< Connection: keep-alive

< Server: uvicorn

</details>

Note that if the DAG author wants to test a new version of the DAG, they will need to perform the following steps:

* Check in the new version of the DAG in the GitHub repository
* First use the OGC API to unregister the DAG
* The use the OGC API to register the DAG again (which will trigger the new version to be deployed to the SPS system)
