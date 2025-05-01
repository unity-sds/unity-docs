# Tutorial: Using the Airflow API with CURL

This page describes how a client (for example, a human or a program) can use the popular CURL tool to interact with the Unity SPS through the Apache Airflow API to configure and execute workloads. We will be assuming that the SPS system is already deployed and the Airflow API is available at some URL of the form:

* AIRFLOW\_API=https://XXXXXXXX.execute-api.us-west-2.amazonaws.com/dev/sps/api/v1

Interaction with the Airflow API will require fetching a Cognito authentication token, detailed in [this tutorial](tutorial-fetching-cognito-authentication-tokens.md). Make sure to fetch and store your authentication token in an easily reference-able variable like `${token}` (used below).

We are also assuming the desired science algorithm has already been registered with the system, though the OGC API or by some other means. For example, each SPS deployment includes making available the CWL DAG ("Common Workflow Language" "Direct Acyclic Graph"), which can be used to execute any CWL workflow to invoke a generic science algorithm packaged as a Docker container.&#x20;

#### Step 1: **Execute a process (i.e. create a job)**

The below example assumes the pre-loaded CWL DAG was registered with the dag id of `cwl_dag`; this may change depending on your use-case.

```
curl -s -X POST "${AIRFLOW_API}/dags/cwl_dag/dagRuns" \
    -H "Content-Type: application/json" \
    -H "Prefer: respond-async" \
    -H "${token}" \ 
    --data-binary @- << EOF | jq '.'
    {
        "conf": {
            "cwl_workflow": "https://raw.githubusercontent.com/unity-sds/sbg-workflows/main/preprocess/sbg-preprocess-workflow.cwl",
            "cwl_args": "https://raw.githubusercontent.com/unity-sds/sbg-workflows/main/preprocess/sbg-preprocess-workflow.dev.yml",
            "request_instance_type": "r7i.xlarge",
            "request_storage": "10Gi",
            "use_ecr": false,
            "log_level": 20 
          }
    }
EOF
```

<details>

<summary>Example Response</summary>

```json
{
  "conf": {
     "cwl_workflow": "https://raw.githubusercontent.com/unity-sds/sbg-workflows/main/preprocess/sbg-preprocess-workflow.cwl",
     "cwl_args": "https://raw.githubusercontent.com/unity-sds/sbg-workflows/main/preprocess/sbg-preprocess-workflow.dev.yml",
     "request_instance_type": "r7i.xlarge",
     "request_storage": "10Gi",
     "use_ecr": false,
     "log_level": 20 
  },
  "dag_id": "cwl_dag",
  "dag_run_id": "manual__2025-04-29T18:26:51.157918+00:00",
  "data_interval_end": "2025-04-29T18:26:51.157918+00:00",
  "data_interval_start": "2025-04-29T18:26:51.157918+00:00",
  "end_date": null,
  "execution_date": "2025-04-29T18:26:51.157918+00:00",
  "external_trigger": true,
  "last_scheduling_decision": null,
  "logical_date": "2025-04-29T18:26:51.157918+00:00",
  "note": null,
  "run_type": "manual",
  "start_date": null,
  "state": "queued"
}
```

</details>

#### Step 2a: Fetching Results of a DAG execution

The below example will fetch the results of the job triggered in Step 1. Querying specific run resuls requires referencing the dag run id (present in the example response)

```
dag_run_id="manual__2025-04-22T17:49:02.502239+00:00"
curl -X GET -H "${token}" "${AIRFLOW_API}/dags/cwl_dag/dagRuns/${dag_run_id}"
```

<details>

<summary>Example Reponse</summary>

```json
{
  "conf": {
     "cwl_workflow": "https://raw.githubusercontent.com/unity-sds/sbg-workflows/main/preprocess/sbg-preprocess-workflow.cwl",
     "cwl_args": "https://raw.githubusercontent.com/unity-sds/sbg-workflows/main/preprocess/sbg-preprocess-workflow.dev.yml",
     "request_instance_type": "r7i.xlarge",
     "request_storage": "10Gi",
     "use_ecr": false,
     "log_level": 20 
  },
  "dag_id": "cwl_dag",
  "dag_run_id": "manual__2025-04-22T17:49:02.502239+00:00",
  "data_interval_end": "2025-04-22T17:49:02.502239+00:00",
  "data_interval_start": "2025-04-22T17:49:02.502239+00:00",
  "end_date": "2025-04-22T18:07:54.466715+00:00",
  "execution_date": "2025-04-22T17:49:02.502239+00:00",
  "external_trigger": true,
  "last_scheduling_decision": "2025-04-22T18:07:54.463268+00:00",
  "logical_date": "2025-04-22T17:49:02.502239+00:00",
  "note": null,
  "run_type": "manual",
  "start_date": "2025-04-22T17:49:03.170771+00:00",
  "state": "success"
}
```

</details>

#### Step 2b: Fetching All results from a DAG

If instead looking to query the results of _all_ runs of a specific DAG, you can simply leave the `${dag_run_id}` out of the query. This query will return a list of entries like the one in Step 2a.

```
curl -X GET -H "${token}" "${AIRFLOW_API}/dags/cwl_dag/dagRuns"
```
