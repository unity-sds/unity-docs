# SPS Post Deployment Operations

After a successful SPS deployment, the following operations must be executed. The SPS endpoints are available from the standard output of the last Terraform command, or by issuing "terraform output".

* Register the default DAGs with the new SPS installation:
  * cd unity-sps/utils
  * ./post\_deployment.sh \<OGC API URL>
* Optionally, execute each DAG once to make sure it works
* Update the endpoints in the common UNITY document: [https://docs.google.com/spreadsheets/d/1FU0sWr726gqtaM50BHaltY29uE2724szWRvtG1dmgXI/edit?gid=0#gid=0](https://docs.google.com/spreadsheets/d/1FU0sWr726gqtaM50BHaltY29uE2724szWRvtG1dmgXI/edit?gid=0#gid=0)&#x20;
* Update the values of the Action variables in the unity-sps repository:
  * [https://github.com/unity-sds/unity-sps/settings/variables/actions](https://github.com/unity-sds/unity-sps/settings/variables/actions)&#x20;
* Execute the Smote Tests and Integration Tests using the branch that was used to deploy the SPS
* Upload the .tfvars configuration files for EKS, Karpenter and Airflow to the common Google Drive: [https://drive.google.com/drive/u/0/folders/1lAZuNzFFV5ngR1LJuQjMA-uzfKvfiWyb](https://drive.google.com/drive/u/0/folders/1lAZuNzFFV5ngR1LJuQjMA-uzfKvfiWyb)
