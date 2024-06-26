---
description: These are the currently "unity" owned venues.
---

# Unity Owned Venues

<div data-full-width="true">

<figure><img src="../../../.gitbook/assets/Unity Venues and SDLC (5).png" alt=""><figcaption></figcaption></figure>

</div>

| Venue            | Description                                                                                                    | Services                                                                                                                                                         | Shared Service Account                |
| ---------------- | -------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------- |
| Unity-Dev        | Development account for unity shared services                                                                  | <ul><li>data catalog</li><li>application catalog</li><li>web gateway</li><li>cognito</li><li>DAPA translator</li></ul>                                           | N/A                                   |
| Unity-Test       | Test account for Unity shared services                                                                         | <ul><li>data catalog</li><li>application catalog</li><li>web gateway</li><li>cognito</li><li>DAPA translator</li></ul>                                           | N/A                                   |
| Unity-Prod       | Production account for unity shared services                                                                   | <ul><li>data catalog</li><li>application catalog</li><li>web gateway</li><li>cognito</li><li>DAPA translator</li></ul>                                           | N/A                                   |
| Unity-Sips-Test  | Venue for SounderSIPS users developing and executing  the chirp, L1a and L1B workflows.                        | <ul><li>ADES (HySDS)</li><li>Job Database</li><li>JupyterHub</li><li>Mission Store</li><li>Dashboard</li></ul>                                                   | Unity-Test (Unity-Prod in the future) |
| Unity-CM         | Nightly test venue.                                                                                            | Management Console                                                                                                                                               | Unity-Test                            |
| Unity-Venue-Dev  | Development environment for venue deployed services                                                            | <ul><li>ADES (HySDS)</li><li>SDAP</li><li>EMS and ADES (Airflow)</li><li>JupyterHub</li><li>Mission Store</li><li>Dashboard</li><li>Management Console</li></ul> | Unity-Dev\*                           |
| Unity-Venue-Test | Test environment for venue deployed services                                                                   | <ul><li>EMS and ADES (Airflow)</li><li>JupyterHub</li><li>Mission Store</li><li>Dashboard</li><li>Management Console</li></ul>                                   | Unity-Test                            |
| Unity-Venue-Ops  | Operational environment for venue deployed services. Used primarily as a smoke test or final validation venue. | <ul><li>EMS and ADES (Airflow)</li><li>JupyterHub</li><li>Mission Store</li><li>Dashboard</li><li>Management Console</li></ul>                                   | Unity-Prod\*                          |
| Unity-SBG-Dev    | Venue to support SBD use cases                                                                                 | <ul><li>EMS and ADES (Airflow)</li><li>Mission Store</li><li>Management Console</li><li><em>JupyterHub (TBD)</em></li></ul>                                      | Unity-Prod\*                          |
|                  |                                                                                                                |                                                                                                                                                                  |                                       |
