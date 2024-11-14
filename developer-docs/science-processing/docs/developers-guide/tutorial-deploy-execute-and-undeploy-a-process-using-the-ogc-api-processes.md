# Tutorial: Deploy, Execute, and Undeploy a Process using the OGC API - Processes

#### Step 1 (optional) - Deploy the Science Processing Service (SPS) in the MCP Dev Account

* **Note:** You can skip this step if you're using the OGC API - Processes of an existing SPS deployment. You'll just need the URL of the API.

```sh
local-machine) $ cd unity-sps/terraform-unity
(local-machine) $ terraform apply -var-file=MCP_DEV.tfvars -no-color 2>&1 --auto-approve | tee apply_output.txt
# Get the hostnames of the load balancers
(local-machine) $ terraform output
resources = {
  "buckets" = {
    "airflow_logs" = {
      "bucket" = "drew-dev-sps-airflowlogs"
      "ssm_param_id" = "/drew/dev/sps/processing/airflow/logs"
    }
  }
  "endpoints" = {
    "airflow" = {
      "rest_api" = {
        "ssm_param_id" = "/drew/dev/sps/processing/airflow/api_url"
        "url" = "http://k8s-sps-airflowi-ad5fd63e43-752497637.us-west-2.elb.amazonaws.com:5000/api/v1"
      }
      "ui" = {
        "ssm_param_id" = "/drew/dev/sps/processing/airflow/ui_url"
        "url" = "http://k8s-sps-airflowi-ad5fd63e43-752497637.us-west-2.elb.amazonaws.com:5000"
      }
    }
    "ogc_processes" = {
      "rest_api" = {
        "ssm_param_id" = "/drew/dev/sps/processing/ogc_processes/api_url"
        "url" = "http://k8s-sps-ogcproce-b59e4702b8-2062050657.us-west-2.elb.amazonaws.com:5001"
      }
      "ui" = {
        "ssm_param_id" = "/drew/dev/sps/processing/ogc_processes/ui_url"
        "url" = "http://k8s-sps-ogcproce-b59e4702b8-2062050657.us-west-2.elb.amazonaws.com:5001/redoc"
      }
    }
  }
}
# Set the the value of OGC_API for future API requests
(local-machine) OGC_API=http://k8s-sps-ogcproce-b59e4702b8-2062050657.us-west-2.elb.amazonaws.com:5001
```

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

[https://github.com/unity-sds/unity-monorepo/blob/main/libs/unity-py/notebooks/ogc\_notebook.ipynb](https://github.com/unity-sds/unity-monorepo/blob/main/libs/unity-py/notebooks/ogc\_notebook.ipynb)
