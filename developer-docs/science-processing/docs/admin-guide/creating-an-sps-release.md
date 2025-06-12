# Creating an SPS Release

Follow these steps to create an SPS release (for example, SPS\_VERSION=25.3).

* Review and upadte or move all GitHub tickets for the current PI
* Update the SPS CHANGELOG ([https://github.com/unity-sds/unity-sps/blob/develop/CHANGELOG.md](https://github.com/unity-sds/unity-sps/blob/develop/CHANGELOG.md))
* Review and update all documentation in GitBook
* Merge all approved changes from feature branches into "develop"
  * For all repositories that have changed: sps, ogc, etc.
* Rebuild and tag the SPS Docker containers using the "develop" branch (again, if the source code has changed):
  * ghcr.io/unity-sds/unity-sps/sps-airflow:$SPS\_VERSION
  * ghcr.io/unity-sds/unity-sps/sps-docker-cwl:$SPS\_VERSION
  * ghcr.io/unity-sds/unity-sps/sps-docker-cwl-modular:$SPS\_VERSION
  * ghcr.io/unity-sds/unity-sps-ogc-process-api/unity-sps-ogc-process-api:$OGC\_VERSION
* Update the versions in all source files:
  * pyproject.toml
    * version
  * terraform-unity/modules/terraform-unity-sps-eks/variables.tf
    * release
  * terraform-unity/modules/terraform-unity-sps-karpenter/variables.tf
    * release
  * terraform-unity/variables.tf
    * release
    * dag\_catalog\_repo/default/ref
    * airflow\_docker\_images
    * ogc\_processes\_api
  * airflow/plugins/sps\_utils.py
    * SPS\_DOCKER\_CWL\_IMAGE
* Also change the version of the Docker containers in the .tfvars files before deploying, or better yet recreate new .tfvars files before deploying
* Deploy the new SPS version to unity-venue-dev using the latest version of the Docker containers
  * But still configure dag\_catalog to sync with brach "develop" as we have not tagged the repository yet
* Deploy the DAGs to unity-venue-dev
  * This shoud be obsolete with the latest release as the DAGs will be automatically deployed
* Execute the unity, smoke and (above all) the integration tests using branch "develop" over the latest deployment in "unity-venue-dev"
  * If any errors are found, they must be fixed before proceding with tagging the repositories
* Merge branch "develop" into branch "main"
  * For all repos - if they have changed: unity-sps, unity-sps-ogc-processes-api, unity-sps-ogc-processes-api-client-python
* Tag all repositories (if they have changed)
  * For all repos - if they have changed: unity-sps, unity-sps-ogc-processes-api, unity-sps-ogc-processes-api-client-python
  * For example:
    * cd unity-sps
    * git checkout main
    * git pull
    * git tag -a "$SPS\_VERSION" -m "SPS Release SPS\_VERSION"
    * git push --tags
* Update the Unity deployment manifest: [https://docs.google.com/spreadsheets/d/1FU0sWr726gqtaM50BHaltY29uE2724szWRvtG1dmgXI/edit?gid=0#gid=0](https://docs.google.com/spreadsheets/d/1FU0sWr726gqtaM50BHaltY29uE2724szWRvtG1dmgXI/edit?gid=0#gid=0) with the new information about all venues tat have been deployed
* Deploy the latest SDS release to "unity-venue-test" and "unity-venue-prod"
  * Coordinate with other SPS engineers to split the deployment task, and also to double check the release.
  * Update the Unity manifest for each new deployment.
