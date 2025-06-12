# Creating an SPS Release

Follow these steps to create an SPS release (for example, SPS\_VERSION=25.3).

* Review and update/close all GitHub tickets for the current PI
* Update the SPS \[CHANGELOG]\([https://github.com/unity-sds/unity-sps/blob/develop/CHANGELOG.md](https://github.com/unity-sds/unity-sps/blob/develop/CHANGELOG.md))
* Review and update all documentation in GitBook
* Merge all approved changes from feature branches --> develop
  * For all repositories: sps, ogc, etc.
* Rebuild and tag the Docker containers using the "develop" branch.
  * ghcr.io/unity-sds/unity-sps/sps-airflow:$SPS\_VERSION
  * ghcr.io/unity-sds/unity-sps/sps-docker-cwl:$SPS\_VERSION
  * ghcr.io/unity-sds/unity-sps/sps-docker-cwl-modular:$SPS\_VERSION
* Update the versions in all source files:
  * pyproject.toml
    * version
  * terraform-unity/modules/terraform-unity-sps-eks/variables.tf
    * release
  * terraform-unity/modules/terraform-unity-sps-karpenter/variables.tf
    * release
  * terraform-unity/variables.tf
    * dag\_catalog\_repo/default/ref
    * airflow\_docker\_images
    * release
    * ogc\_processes\_apir
  * airflow/plugins/sps\_utils.py
    * SPS\_DOCKER\_CWL\_IMAGE
* Also change the version of the Docker containers in the .tfvars files before deploying, or better yet recreate new .tfvars files
* Deploy the new version to unity-venue-dev using the latest version of the Docker containers
  * But still configure dag\_catalog to sync with brach "develop" as we have not tagged the repository yet
* Deploy the DAGs to unity-venue-dev
  * This shoud be obsolete with the latest release as the DAGs will be automatically deployed
* Must make sure unity, smoke and integration tests work from branch "develop" with the latest deployment in "unity-venue'dev"
* Merge branch "develop" into branch "main"
  * For all repos - if they have changed: unity-sps, unity-sps-ogc-processes-api, unity-sps-ogc-processes-api-client-python
* Tag all repositories (if they have changed)
  * For all repos - if they have changed: unity-sps, unity-sps-ogc-processes-api, unity-sps-ogc-processes-api-client-python
* Update the Unity deployment manifest: [https://docs.google.com/spreadsheets/d/1FU0sWr726gqtaM50BHaltY29uE2724szWRvtG1dmgXI/edit?gid=0#gid=0](https://docs.google.com/spreadsheets/d/1FU0sWr726gqtaM50BHaltY29uE2724szWRvtG1dmgXI/edit?gid=0#gid=0) with the new information about all venues tat have been deployed
* Deploy the latest SDS release to "unity-venue-test" and "unity-venue-prod"
  * Coordinate with other SPS engineers to split the deployment task, and also to double check the release.
