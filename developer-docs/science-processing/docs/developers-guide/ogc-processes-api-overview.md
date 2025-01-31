# OGC Processes API Overview

The OGC ("Open Spatial Consortium") Processes API ("Abstract Program Interface") is an international specification for executing remote processing of Earth Science data. The client (for example a program, or a user using a web user interface) makes HTTP(s) requests to execute data processing on a remote server, passing all required information in JSON ("JavaScript Object Notation") format. The client-server interaction through the API is agnostic to the specific data processing engine implemented by the server.

This page provides a high-level overview of the functionality exposed by the API. Other pages contain specific examples of interaction using [CURL](https://unity-sds.gitbook.io/docs/developer-docs/science-processing/docs/developers-guide/tutorial-using-the-ogc-processes-api-with-curl)  and [Python](https://unity-sds.gitbook.io/docs/developer-docs/science-processing/docs/developers-guide/tutorial-using-the-ogc-processes-api-with-python). More detailed documentation on the API can be found on the [OGC web page](https://ogcapi.ogc.org/processes/).

The API includes the following functionality for managing _**processes**_ (aka available science algorithms):

(All requests start with: `https://${OGC_PROCESSES_API}`and are appended with the Request column value.)

<table><thead><tr><th width="125">Endpoint</th><th width="99">Method</th><th width="177">Request</th><th width="273">Response</th><th data-type="checkbox">Payload</th><th>Notes</th></tr></thead><tbody><tr><td><strong>List all processes</strong></td><td><code>GET</code></td><td><pre data-overflow="wrap"><code>/processes
</code></pre></td><td>JSON payload containing a summary description of all registered processes.</td><td>false</td><td></td></tr><tr><td><strong>Register a new process</strong></td><td><code>POST</code></td><td><pre data-overflow="wrap"><code>/processes
</code></pre></td><td>result of the registration request (success or failure)</td><td>true</td><td>[1]</td></tr><tr><td><strong>Describe a process</strong></td><td><code>GET</code></td><td><pre data-overflow="wrap"><code>/processes/${process_id}
</code></pre></td><td>JSON document containing all details of the specified process</td><td>false</td><td></td></tr><tr><td><strong>De-register a process</strong></td><td><code>DELETE</code></td><td><pre data-overflow="wrap"><code>/processes/${process_id}
</code></pre></td><td>result of the de-register action (success or failure)</td><td>false</td><td></td></tr></tbody></table>

**\[1] Register a new process JSON Payload** contains all details of the process to be registered, including:

* The algorithm to be executed, in the form of a Docker image
* Required and optional input parameters, their type and possible default value.
* The expected output values and their type

***

The API contains the following functionality for managing _**jobs**_ (i.e. actual executions of processes):

(All requests start with: `https://${OGC_PROCESSES_API}`and are appended with the Request column value.)

<table><thead><tr><th width="134">Endpoint</th><th width="87">Method</th><th width="154">Request</th><th width="299">Response</th><th data-type="checkbox">Payload</th><th>Notes</th></tr></thead><tbody><tr><td><strong>List all jobs</strong></td><td><code>GET</code></td><td><pre data-overflow="wrap"><code>/jobs
</code></pre></td><td>JSON document containing the list of all jobs that were executed on the server (combined for all times, all processes, and all statuses)</td><td>false</td><td></td></tr><tr><td><strong>Execute a job</strong></td><td><code>POST</code></td><td><pre data-overflow="wrap"><code>/execution
</code></pre></td><td>result of the execution request (success or failure) and (if successful) job identifier that can be used for monitoring the execution</td><td>true</td><td>[2]</td></tr><tr><td><strong>Monitor a job</strong></td><td><code>GET</code></td><td><pre data-overflow="wrap"><code>/${job_id}
</code></pre></td><td>JSON document containing the job details, including its current status</td><td>false</td><td></td></tr><tr><td><strong>Delete a job</strong></td><td><code>DELETE</code></td><td><pre data-overflow="wrap"><code>/jobs/${job_id}
</code></pre></td><td>JSON document containing the status of the deletion request</td><td>false</td><td></td></tr><tr><td><strong>Retrieve the results of a job (if available)</strong></td><td><code>GET</code></td><td><pre data-overflow="wrap"><code><strong>/jobs/${job_id}/results
</strong></code></pre></td><td>JSON document containing references to the job output files</td><td>false</td><td></td></tr></tbody></table>

**\[2] Execute a job JSON payload** contains the details of the specific execution request, i.e. the values of all required and optional input parameters.
