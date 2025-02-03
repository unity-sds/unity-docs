# Developers Guide

Unity SPS developers are responsible for authoring, testing and executing workflows for science data processing. These workflows are encoded in Python as Airflow DAGs (Direct Acyclic Graphs) and eventually executed on Kubernetes Pods running on EC2 nodes in the AWS Cloud.

Developers may write DAGs from scratch following the well documented Airflow guide \[1], or may take advantage of the SPS built-in CWL DAG that allows them to execute any workflow written according to the CWL specification \[2]. In this latter case, the developer's effort shifts from learning how to write a DAG, to learning how to write a CWL workflow.

In either case, the Unity SPS allows developers to submit, monitor and inspect a workflow execution either via the Airflow User Interface \[3], via the Airflow REST API \[4], or via the OGC API \[5] \[6] \[5].

## Working with the CWL DAG

Because the CWL DAG is already registered in the SPS system as part of its deployment, the developer's responsibility is to author a CWL document (and associated YAML or JSON parameters file) that will execute their specific data processing workflow. Typically (but not necessarily) the CWL workflow will invoke a Docker container that packages the specific science algorithm \[8].

The CWL DAG can be executed via the Airflow UI, the Airflow API or the OGC Processes API (using the "/jobs" endpoint). The CWL and YAML/JSON file URLs will be specified as input parameters when submitting a job for execution.

## Best Practice #1

Because CWL has a pretty steep learning curve, the developer may want to author the CWL workflow using their own laptop or desktop, before uploading it and executing it on the Unity SPS (where it can be run programmatically and at scale). This can be easily accomplished by installing the _cwltool_ in the computer's main environment or (better) by creating a dedicated Python virtual environment \[9].

## Best Practice #2

It is recommended that both the CWL workflow and the invoked Docker container be explicitly versioned in their file name and Docker tag. For example:

*   http://github.com/acme\_organization/cwl\_workflows/acme\_workflow\_v1.cwl

    which internally references:
* ghcr.io/acme\_organization/acme\_algorithm:1.2.3

This practice will allow the developer to unequivocally update and execute new versions of the CWL workflow and/or Docker container.

## Authoring a generic Python DAG

When authoring a generic DAG (using Python), the developer will need to check the source code into the specific GitHub repository that is configured for that SPS installation (this repository will be chosen by the project before deploying the SPS). The developer will need to "register" the Python DAG using the OGC Processes API (specifically, the "/processes" endpoint) before it can be executed using the Airflow UI or supported REST APIs.

## Best Practice

It is recommended that the developer explicitly encode the version of the DAG in the file name. For example:

* http://github.com/acme\_organization/airflow\_dags/acme\_dag\_v1.py

This will allow the developer to unequivocally execute the new version of the DAG by following these steps:

* Check in the new version of the DAG into GitHub
* Use the OGC processes API to register the new version (with the new identifier "acme\_dag\_v1")
* Use any available method to execute the "acme\_dag\_v1" DAG.

## References

\[1] [Airflow DAGs](https://airflow.apache.org/docs/apache-airflow/stable/core-concepts/dags.html)

\[2] [Common Workflow Language (CWL)](https://www.commonwl.org/)

\[3] [Astronomer tutorial on Airflow User Interface](https://www.astronomer.io/docs/learn/airflow-ui/)

\[4] [Airflow REST API](https://airflow.apache.org/docs/apache-airflow/stable/stable-rest-api-ref.html)

\[5] [Unity OGC Processes API Overview](developers-guide/ogc-processes-api-overview.md)

\[6] [Tutorial: Using the OGC Processes API with CURL](developers-guide/tutorial-using-the-ogc-processes-api-with-curl.md)

\[7] [Tutorial: Using the OGC Processes API with Python](developers-guide/tutorial-using-the-ogc-processes-api-with-python.md)

\[8] [Using Docker containers with CWL](https://www.commonwl.org/user_guide/topics/using-containers.html)

\[9] [CWL Quick Start](https://www.commonwl.org/user_guide/introduction/quick-start.html)
