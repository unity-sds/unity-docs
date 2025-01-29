# OGC Processes API Overview

The OGC ("Open Spatial Consortium") Processes API ("Abstract Program Interface") is an international specification for executing remote processing of Earth Science data. The client (for example a program, or a user using a web user interface) makes HTTP(s) requests to execute data processing on a remote server, passing all required information in JSON ("JavaScript Object Notation") format. The client-server interaction through the API is agnostic to the specific data processing engine implemented by the server.

This page provides a high-level overview of the functionality exposed by the API. Other pages contain specific examples of interaction using CURL (...) and Python (...). More detailed documentation on the API can be found on the [OGC web page](https://ogcapi.ogc.org/processes/).

The API includes the following functionality for managing _processes_ (aka available science algorithms):

* **List all processes** available for execution:
  * HTTP request:  GET https://${OGC\_PROCESSES\_API}/processes
  * HTTP response: JSON payload containing a summary description of all registered processes.
* **Register a new process** for execution:
  * HTTP request: POST https://${OGC\_PROCESSES\_API}/processes
    * JSON Payload: contains all details of the process to be registered, including:
      * The algorithm to be executed, in the form of a Docker image
      * Required and optional input parameters, their type and possible default value.
      * The expected output values and their type
  * HTTP response: result of the registration request (success or failure)
* **Describe a process**:
  * HTTP request: GET https://${OGC\_PROCESSES\_API}/processes/${process\_id}
  * HTTP response: JSON document containing all details of the specified process
* **Unregister a process** so that it cannot be executed any more:
  * HTTP request: DELETE https://${OGC\_PROCESSES\_API}/processes/${process\_id}
  * HTTP response: result of the unregister action (success or failure)

The API contains the following functionality for managing _jobs_ (i.e. actual executions of processes):

* **List all  jobs**:
  * HTTP request:  GET https://${OGC\_PROCESSES\_API}/jobs
  * HTTP response: JSON document containing the list of all jobs that were executed on the server (combined for all times, all processes, and all statuses)
* **Execute a job**:
  * HTTP request: POST https://${OGC\_PROCESSES\_API}/${process\_id}/execution
    * JSON payload: contains the details of the specific execution request, i.e. the values of all required and optional input parameters
  * HTTP response:  result of the execution request (success or failure) and (if successful) job identifier that can be used for monitoring the execution
* **Monitor a job**:
  * HTTP request: GET https://${OGC\_PROCESSES\_API}/jobs/${job\_id}
  * HTTP response: JSON document containing the job details, including its current status
* **Delete a job** (whatever its status is: running, finished, etc.):
  * HTTP request: DELETE https://${OGC\_PROCESSES\_API}/jobs/${job\_id}
  * HTTP response: JSON document containing the status of the deletion request
* **Retrieve the results of a job** (if available):
  * HTTP request: GET https://${OGC\_PROCESSES\_API}/jobs/${job\_id}/results
  * HTTP response: JSON document containing references to the job output files



