# Tutorial: Using the OGC Processes API with CURL

This page describes how a client (for example, a human or a program) can use the popular CURL tool to interact with the Unity SPS through the OGC Processes API to register a new science algorithm and request its execution. We will be assuming that the SPS system is already deployed and the OGC Processes API is available at some URL of the form:&#x20;

* OGC\_PROCESSES\_API=https://XXXXXXXXX.execute-api.us-west-2.amazonaws.com/dev/ogc/api/

Interaction with the OGC API will require fetching a Cognito authentication token, detailed in [this tutorial](tutorial-fetching-cognito-authentication-tokens.md). Make sure to fetch and store your authentication token in an easily reference-able variable like `${token}` (used below).

Let's first consider the case when the desired science algorithm has already been registered with the system. For example, each SPS deployment includes making available the CWL DAG ("Common Workflow Language" "Direct Acyclic Graph"), which can be used to execute any CWL workflow to invoke a generic science algorithm packaged as a Docker container. Later we will show how such a DAG can be registered in the first place.

## **Step 1a (optional): List the available processes**



<details>

<summary>Request</summary>

```
curl -k -X GET -H "${token}" "${OGC_PROCESSES_API}/processes" | jq
```

</details>

<details>

<summary>Response</summary>

```
{
  "processes": [
    {
      "title": "Generic CWL Process",
      "description": "This process executes any CWL workflow.",
      "keywords": null,
      "metadata": null,
      "id": "cwl_dag",
      "version": "1.0.0",
      "jobControlOptions": [
        "async-execute"
      ],
      "links": null
    },
    {
      "title": "Karpenter Test Process",
      "description": "This process tests Karpenter node provisioning with different instance types.",
      "keywords": null,
      "metadata": null,
      "id": "karpenter_test",
      "version": "1.0.0",
      "jobControlOptions": [
        "async-execute"
      ],
      "links": null
    },
    {
      "title": "SBG Preprocess CWL Workflow",
      "description": "This process executes the SBG Preprocess Workflow using CWL.",
      "keywords": null,
      "metadata": null,
      "id": "sbg_preprocess_cwl_dag",
      "version": "1.0.0",
      "jobControlOptions": [
        "async-execute"
      ],
      "links": null
    }
  ],
  "links": []
}
```

</details>

You will notice that the server response includes a process with identifier "cwl\_dag", which we will want to trigger next.

## **Step 2a: Execute a process (i.e. create a job)**

<details>

<summary>Request</summary>

<pre><code><strong>curl -s -X POST "${OGC_PROCESSES_API}/processes/cwl_dag/execution" \
</strong>    -H "Content-Type: application/json" \
    -H "Prefer: respond-async" \
    -H "${token}" \ 
    --data-binary @- &#x3C;&#x3C; EOF | jq '.'
    {
        "inputs": {
            "cwl_workflow": "https://raw.githubusercontent.com/unity-sds/sbg-workflows/main/preprocess/sbg-preprocess-workflow.cwl",
            "cwl_args": "https://raw.githubusercontent.com/unity-sds/sbg-workflows/main/preprocess/sbg-preprocess-workflow.dev.yml",
            "request_instance_type": "r7i.xlarge",
            "request_storage": "10Gi",
            "use_ecr": false,
            "log_level": 20 
          },
          "outputs": {
            "result": {
              "transmissionMode": "reference"
            }
          }
    }
EOF
</code></pre>

</details>

<details>

<summary>Response</summary>

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
    "7b1173ff-137e-41fa-bfb1-bd133049b4a3"
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
    "2025-01-29T20:33:05.521710"
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
    "2025-01-29T20:33:05.521715"
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

Note that the response contains the job id ("7b1173ff-137e-41fa-bfb1-bd133049b4a3") and its status ("accepted").&#x20;

## **Step 3a: Monitor the job**

You can use the job id returned in the previous step to monitor the job until it completes.

<details>

<summary>Request</summary>

```
JOB_ID=7b1173ff-137e-41fa-bfb1-bd133049b4a3
curl -H "${token}" "${OGC_PROCESSES_API}/jobs/${JOB_ID}"
```

</details>

<details>

<summary>Response</summary>

```
{
  "processID": "cwl_dag",
  "type": "process",
  "jobID": "7b1173ff-137e-41fa-bfb1-bd133049b4a3",
  "status": "running",
  "message": null,
  "exception": null,
  "created": "2025-01-29T20:33:05.521710Z",
  "started": null,
  "finished": null,
  "updated": "2025-01-29T20:41:47.598612Z",
  "progress": null,
  "links": null
}
```

</details>

Or to keep executing the command every few seconds:

<details>

<summary>Request</summary>

```
watch -n 5 "curl -s -H "${token}" "${OGC_PROCESSES_API}/jobs/${JOB_ID}" | jq"
```

</details>

<details>

<summary>Response</summary>

```
{
  "processID": "cwl_dag",
  "type": "process",
  "jobID": "7b1173ff-137e-41fa-bfb1-bd133049b4a3",
  "status": "successful",
  "message": null,
  "exception": null,
  "created": "2025-01-29T20:33:05.521710Z",
  "started": null,
  "finished": "2025-01-29T20:48:11.943940Z",
  "updated": "2025-01-29T20:58:31.094170Z",
  "progress": null,
  "links": null
}
```

</details>

***

Let's now demonstrate how a client would make a request to register a new science algorithm, encoded as an Airflow DAG. The DAG (a Python program) needs to be checked in within the GitHub repository that was configured during the SPS deployment. In other words, the DAG author will check the latest version of the code into GitHub in the specified folder, and then they can make an HTTP request to the OGC API to register that process for execution.&#x20;

Let's consider the following DAG: [https://github.com/unity-sds/unity-sps/blob/develop/airflow/dags/cwl\_dag.py](https://github.com/unity-sds/unity-sps/blob/develop/airflow/dags/cwl_dag.py)

We will assume that the SPS deployment has been configured to monitor the GitHub repository [https://github.com/unity-sds/unity-sps](https://github.com/unity-sds/unity-sps) at the path "airflow/dags" in the brach "main".

## **Step 1b: Register a process**

<details>

<summary>Request</summary>

```
curl -k -v -X POST -H "${token}" -H "Expect:" -H "Content-Type: application/json; charset=utf-8" --data-binary @"./cwl_dag.json" "${OGC_PROCESSES_API}/processes"
```

where:

`cat cwl_dag.json`&#x20;

```
{
  "executionUnit": {
    "image": "ghcr.io/unity-sds/unity-sps/sps-docker-cwl:2.4.0",
    "type": "docker"
  },
  "processDescription": {
    "description": "This process executes any CWL workflow.",
    "id": "cwl_dag",
    "inputs": {
      "cwl_args": {
        "description": "The URL of the CWL workflow's YAML parameters file",
        "maxOccurs": 1,
        "minOccurs": 1,
        "schema": {
          "format": "uri",
          "type": "string"
        },
        "title": "CWL Workflow Parameters URL"
      },
      "cwl_workflow": {
        "description": "The URL of the CWL workflow",
        "maxOccurs": 1,
        "minOccurs": 1,
        "schema": {
          "format": "uri",
          "type": "string"
        },
        "title": "CWL Workflow URL"
      },
      "request_instance_type": {
        "description": "The specific EC2 instance type requested for the job",
        "maxOccurs": 1,
        "minOccurs": 1,
        "schema": {
          "type": "string"
        },
        "title": "Requested EC2 Type"
      },
      "request_storage": {
        "description": "The amount of storage requested for the job",
        "maxOccurs": 1,
        "minOccurs": 1,
        "schema": {
          "type": "string"
        },
        "title": "Requested Storage"
      }
    },
    "jobControlOptions": [
      "async-execute"
    ],
    "outputs": {
      "result": {
        "description": "The result of the SBG Preprocess Workflow execution",
        "schema": {
          "$ref": "some-ref"
        },
        "title": "Process Result"
      }
    },
    "title": "Generic CWL Process",
    "version": "1.0.0"
  }
}
```

</details>

<details>

<summary>Response</summary>

```
< HTTP/1.1 201 Created
< Date: Thu, 30 Jan 2025 15:06:26 GMT
< Content-Length: 37
< Connection: keep-alive
< Server: uvicorn
Process cwl_dag deployed successfully%    
```

</details>

Note that the HTTP request contains the dag id "cwl\_dag". This id _must_ match the filename of the DAG in the GitHub repository, at the specified path and branch, without the ".py" extension.

```
< HTTP/1.1 201 Created
< Date: Thu, 30 Jan 2025 11:55:45 GMT
< Content-Length: 37
< Connection: keep-alive
< Server: uvicorn

Process cwl_dag deployed successfully%      
```

## Step 2b: Unregister a process

<details>

<summary>Request</summary>

```
curl -kv -X DELETE -H "${token}" -H "Content-Type: application/json; charset=utf-8" "${OGC_PROCESSES_API}/processes/cwl_dag"
```

</details>

<details>

<summary>Response</summary>

```
< HTTP/1.1 204 No Content
< Date: Thu, 30 Jan 2025 15:05:34 GMT
< Connection: keep-alive
< Server: uvicorn
```

</details>

Note that this removes the DAG from the SPS system.

## Step 1c: Update a process

<details>

<summary><strong>Request</strong> </summary>

```
curl --location "${OGC_PROCESSES_API}/processes" \
--header "Expect;" \
--header "Content-Type: application/json; charset=utf-8" \
--header "${token}" \
--data-binary @"./cwl_dag.json"
```

where:

`cat cwl_dag.json`&#x20;

```
{
  "executionUnit": {
    "image": "ghcr.io/unity-sds/unity-sps/sps-docker-cwl:2.4.0",
    "type": "docker"
  },
  "processDescription": {
    "description": "This process executes any CWL workflow.",
    "id": "cwl_dag",
    "inputs": {
      "cwl_args": {
        "description": "The URL of the CWL workflow's YAML parameters file",
        "maxOccurs": 1,
        "minOccurs": 1,
        "schema": {
          "format": "uri",
          "type": "string"
        },
        "title": "CWL Workflow Parameters URL"
      },
      "cwl_workflow": {
        "description": "The URL of the CWL workflow",
        "maxOccurs": 1,
        "minOccurs": 1,
        "schema": {
          "format": "uri",
          "type": "string"
        },
        "title": "CWL Workflow URL"
      },
      "request_instance_type": {
        "description": "The specific EC2 instance type requested for the job",
        "maxOccurs": 1,
        "minOccurs": 1,
        "schema": {
          "type": "string"
        },
        "title": "Requested EC2 Type"
      },
      "request_storage": {
        "description": "The amount of storage requested for the job",
        "maxOccurs": 1,
        "minOccurs": 1,
        "schema": {
          "type": "string"
        },
        "title": "Requested Storage"
      }
    },
    "jobControlOptions": [
      "async-execute"
    ],
    "outputs": {
      "result": {
        "description": "The result of the SBG Preprocess Workflow execution",
        "schema": {
          "$ref": "some-ref"
        },
        "title": "Process Result"
      }
    },
    "title": "Generic CWL Process",
    "version": "1.0.0"
  }
}
```

</details>

<details>

<summary><strong>Response</strong></summary>

```
 HTTP/1.1 204 No Content
```

</details>

The steps to develop and update a DAG include:

1. Modify the DAG and check in the new version to the GitHub repository
2. Send a `PUT` request to the OGC Processes API to update the DAG which deploys the updated DAG code to the SPS system
