## Science Processing Service: Apache Airflow Integration

### **Status**

| Status   | Date       |
| -------- | ---------- |
| Proposed | 11/10/2023 |
|          |            |

### **Context**

In recent years, Apache Airflow has emerged as one of the leading open source orchestration engines for scalable jobs processing. Additionally, it is gaining attention and traction at JPL across several projects in Earth and Planetary sciences. We are proposig to integrate the Airflow architecture in the Unity model, as such (see diagram below):

* The core components of Airflow (Web Server, Scheduler, Database) will compose the front-end EMS layer (which provides orchestration and monitoring across multiple back-ends)
* The Airflow Operators will be used to submit workloads to multiple pluggable ADES back-ends (Celery Workers, EKS, ECS, etc.)

Additionally, Unity may decide to provide Airflow extensions as follows:
* An OGC WPS-T interface to allow clients to submit job requests that conform to this API specificiation
* An Airflow HySDS Operator to allow projects to execute workloads on the HySDS system
* An Airflow WPS-T Operator to allow projects to submit requests to any WPS-T compliant back-end

<img width="982" alt="Screenshot 2023-11-10 at 09 38 20" src="https://github.com/LucaCinquini/unity-docs/assets/637059/5ada4434-cc05-4153-9b35-ca6c38a2d5df">

  
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
  * Supporting the OGC WPS-T specification
  * Offering HySDS as a possible processing engine
* Overall, it allows users the flexibility to author their workflows in pure Python (Airflow), or CWL, and to request execution via the WPS-T or Airflow APIs

### **Impacts**

The development work of offering Airflow as part of the Unity infrastructure has been scoped and should not take more than a few months. Providing interoperability with WPS-T and HySDS will take longer, perhaps another few months. This architecture improvement will provide long term benefits and longevity to the Unity project.

### References

* [Apache Airflow](https://airflow.apache.org/)

* [SPS presentation to Unity (private)](https://docs.google.com/presentation/d/1ibGSqWkvZXVBxvkm08ZHAk4bjcs03oZYgFmV1DPPxWU/edit#slide=id.g291c340cd4b_0_0)
