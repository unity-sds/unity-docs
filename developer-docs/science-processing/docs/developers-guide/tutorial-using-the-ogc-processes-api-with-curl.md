# Tutorial: Using the OGC Processes API with CURL

This page describes how a client (for example, a human or a program) can use the popular CURL tool to interact with the Unity SPS through the OGC Processes API to register a new science algorithm and request its execution. The SPS system is already deployed and the OGC Processes API is available at some URL of the form:&#x20;

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

You can use the job id returned in the previous step to keep monitoring the job until it completes.

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

Or to execute the command every 5 seconds:

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

Let's now demonstrate how a client would make a request to register a new science algorithm, encoded as an Airflow DAG. The DAG (a Python program) needs to be checked in within the GitHub repository that was configured during the SPS deployment. In other words, the DAG author will check the latest version of the code into GitHub in the specified folder, and then they can make an HTTP request to the OGC API to register that process for execution.&#x20;

Step 1b: Register a process

We will register the following CWL DAG:

* [https://github.com/unity-sds/unity-sps/blob/develop/airflow/dags/cwl\_dag.py](https://github.com/unity-sds/unity-sps/blob/develop/airflow/dags/cwl_dag.py)

<details>

<summary>Request</summary>



</details>





At this time, for this to work the following conditions must be satisfied:

* The Airflow DAG is publicly available in the GitHub repository and at the path that the SPS installation was configured to look for. For example, we will demonstrate how to register the following Airflow DAG:
  * [https://github.com/unity-sds/unity-sps/blob/develop/airflow/dags/cwl\_dag.py](https://github.com/unity-sds/unity-sps/blob/develop/airflow/dags/cwl_dag.py)
  *

#### Step 2 - Deploy a Process Named `cwl_dag` with the OGC API

<details>

<summary>Request</summary>

```sh
curl -s -0 -X POST "${OGC_API}/processes" \
-H "Expect:" \
-H 'Content-Type: application/json; charset=utf-8' \
--data-binary @- << EOF
{
    "processDescription": {
        "title": "Generic CWL Process",
        "description": "This process executes any CWL workflow.",
        "id": "cwl_dag",
        "version": "1.0.0",
        "jobControlOptions": [
            "async-execute"
        ],

        "inputs": {
            "cwl_workflow": {
                "title": "CWL Workflow URL",
                "description": "The URL of the CWL workflow",
                "schema": {
                    "type": "string",
                    "format": "uri"
                },
                "minOccurs": 1,
                "maxOccurs": 1
            },
            "cwl_args": {
                "title": "CWL Workflow Parameters URL",
                "description": "The URL of the CWL workflow's YAML parameters file",
                "schema": {
                    "type": "string",
                    "format": "uri"
                },
                "minOccurs": 1,
                "maxOccurs": 1
            },
            "request_memory": {
                "title": "Requested Memory",
                "description": "The amount of memory requested for the job",
                "schema": {
                    "type": "string"
                },
                "minOccurs": 1,
                "maxOccurs": 1,
                "default": "8Gi"
            },
            "request_cpu": {
                "title": "Requested CPU",
                "description": "The number of CPU cores requested for the job",
                "schema": {
                    "type": "string"
                },
                "minOccurs": 1,
                "maxOccurs": 1
            },
            "request_storage": {
                "title": "Requested Storage",
                "description": "The amount of storage requested for the job",
                "schema": {
                    "type": "string"
                },
                "minOccurs": 1,
                "maxOccurs": 1
            }
        },
        "outputs": {
            "result": {
                "title": "Process Result",
                "description": "The result of the SBG Preprocess Workflow execution",
                "schema": {
                    "$ref": "some-ref"
                }
            }
        }
    },
    "executionUnit": {
        "type": "docker",
        "image": "ghcr.io/unity-sds/unity-sps/sps-docker-cwl:2.1.0"
    }
}
EOF
```



</details>

<details>

<summary>Expected Response</summary>

```
Process cwl_dag deployed successfully                                                                                                                                                                                        
```



</details>

#### Step 3 - Trigger Execution of the `cwl_dag` Process through the OGC API

<details>

<summary>Request</summary>

```sh
curl -s -X POST "${OGC_API}/processes/cwl_dag/execution" \
-H "Content-Type: application/json" \
-H "Prefer: respond-async" \
--data-binary @- << EOF | jq '.'
{
  "inputs": {
    "cwl_workflow": "https://raw.githubusercontent.com/unity-sds/sbg-workflows/main/preprocess/sbg-preprocess-workflow.cwl",
    "cwl_args": "https://raw.githubusercontent.com/unity-sds/sbg-workflows/main/preprocess/sbg-preprocess-workflow.dev.yml",
    "request_memory": "8Gi",
    "request_cpu": "4",
    "request_storage": "10Gi"
  },
  "outputs": {
    "result": {
      "transmissionMode": "reference"
    }
  }
}
EOF
```

</details>

<details>

<summary>Expected Response</summary>

```
[
  [
    "process_id",
    "cwl_dag"
  ],
  [
    "type",
    "process"
  ],
  [
    "job_id",
    "7b23d796-96ae-4d6b-b1c1-15181752aa43"
  ],
  [
    "status",
    "accepted"
  ],
  [
    "message",
    null
  ],
  [
    "exception",
    null
  ],
  [
    "created",
    "2024-09-18T18:03:38.299715"
  ],
  [
    "started",
    null
  ],
  [
    "finished",
    null
  ],
  [
    "updated",
    "2024-09-18T18:03:38.299718"
  ],
  [
    "progress",
    null
  ],
  [
    "links",
    null
  ]
]
```

</details>

#### Step 5a - Check Job Status through the OGC API

* Check the status of the job by it's ID:

```shell
JOB_ID={Find in the `job_id` field in above response}
watch -n 5 "curl -s "${OGC_API}/jobs/${JOB_ID}" | jq"
```

#### Step 5b - Check Job Status through the Airflow UI

* Verify the execution of the process started a `cwl_dag` DAG run.

#### Step 6 - After the Job Completes, Undeploy the Process

```sh
curl -s -X DELETE "${OGC_API}/processes/cwl_dag?force=true"
```

The following Jupyter Notebook contains an example on how to use the Unity OGC Python client to request and monitor an OGC process:

[https://github.com/unity-sds/unity-monorepo/blob/main/libs/unity-py/notebooks/ogc\_notebook.ipynb](https://github.com/unity-sds/unity-monorepo/blob/main/libs/unity-py/notebooks/ogc_notebook.ipynb)
