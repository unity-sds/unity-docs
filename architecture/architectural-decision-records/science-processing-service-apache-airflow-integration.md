## Science Processing Service: Apache Airflow Integration

### **Status**

| Status   | Date       |
| -------- | ---------- |
| Proposed | 11/10/2023 |
|          |            |

### **Context**

In recent years, Apache Airflow has emerged as one of the leading open source orchestration engines for scalable jobs processing. Additionally, it is gaining attention and traction at JPL across several projects in Earth and Planetary sciences. We are proposig to integrate the Airflow architecture in the Unity model, as such:

* The core components of Airflow (Web Server, Scheduler, Database) will compose the front-end EMS layer (which provides orchestration and monitoring across multiple back-ends)
* The Airflow Operators will be used to submit workloads to multiple pluggable ADES back-ends (Celery Workers, EKS, ECS, etc.)

Additionally, Unity may decide to provide Airflow extensions as follows:
* An OGC WPS-T interface to allow clients to submit job requests that conform to this API specificiation
* An Airflow HySDS Operator to allow projects to execute workloads on the HySDS system
* An Airflow WPS-T Operator to allow projects to submit requests to any WPS-T compliant back-end
  
### Alternatives

The following architectural options have been investigated to offer users the ability to leverage the Airflow functionality
(see the referenced presentation for details):

* Option 1: Integrate Airflow simply as a possible ADES back-end
* Option 2: Fork and maintain CWL-Airflow as a possible ADES back-end
* Option 3: Integrate Airflow as the Unity EMS layer and use Airflow operators to execute workloads on different ADES back-ends. This is the option that was chosen to provide the most functionality and long-term benefits.

### **Decision and Rationale**

We propose to choose Option 3 above for the following reasons:
* It provides Unity with an EMS (orchestration) layer out-of-the-box, which otherwise Unity would have to custom develop (lengthy and costly)
* It offers the full Airflow functionality to our users
* It provides integration paths with the previous Unity APIs and workload engines, specifically:
  * Supporting the OGC WPS-T spec
  * Offering HySDS as processing engine
* Overall, it allows users the flexibility to author their workflows in pure Python (Airflow), or CWL, and to request execution via the WPS-T or Airflow APIs

### **Impacts**

CQL will need to be documented and tested. Common filters (e.g. a single value, string, etc) might be pretty straight forward but complex metadata (e.g. nested JSON) might not be supported. This does allow us to migrate to different technologies (e.g. elastic search, databases, etc) in the future without impacting the users.

The use of CQL will require development effort within the DAPA request and we're not sure this will be supported by the process mapper functionality.&#x20;

The CQL development will also duplicate native capabilties of elastic search and this was a primary concern with the cost of development.

Lastly, the results will be returned via STAC so this should integrate with stage in requests as well where needed.

### References

(Optional) Any other references that make sense. Documentation links, other ADRs, etc.

{% embed url="https://docs.up42.com/developers/api-assets/stac-cql" %}

{% embed url="https://pystac-client.readthedocs.io/en/latest/tutorials/cql2-filter.html" %}
