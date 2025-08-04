# Tutorial: Large Scale Testing

## Pre-Requisites

Follow the instructions at [SPS Deployment with Terraform](../admin-guide/sps-deployment-with-terraform.md). Note that in [Step 3: Deploy Airflow onto the EKS Cluster](../admin-guide/sps-deployment-with-terraform.md#step-3-deploy-airflow-onto-the-eks-cluster), follow the instructions to modify the "helm\_values\_template" terraform value from "values.tmpl.yaml" to "values\_high\_load.tmpl.yaml".

Once you've completed the deployment, navigate to your Airfow UI and log in using your Cognito user pool credentials:

<figure><img src="../../../../.gitbook/assets/Screenshot_2025-06-17_at_8_37_21 AM.png" alt=""><figcaption></figcaption></figure>

This will redirect you to the Airflow UI. You will have to click on the "Log In" button at the top right to authenticate into Airflow as the "admin" user. Enter the admin password you specified during deployment:

<figure><img src="../../../../.gitbook/assets/Screenshot 2025-06-17 at 8.41.06 AM.png" alt=""><figcaption></figcaption></figure>

You are now logged in as the admin user with the Admin role.

Also, take note of the airflow REST API endpoint (`rest_api`) that was created during airflow deployment. It should follow a similar convention:

```
  "venue_endpoints" = {
    "airflow" = {
      "rest_api" = {
        "url" = "HTTP://internal-gman-6-dev-httpd-alb-123456790.us-west-2.elb.amazonaws.com:8080/gman-6/dev/sps/
api/v1"
      }
      "ui" = {
        "url" = "HTTP://internal-gman-6-dev-httpd-alb-123456790.us-west-2.elb.amazonaws.com:8080/gman-6/dev/sps/"
      }
    }
```

We will need this value to configure the router in our initiator.

Finally, make sure you have refreshed the AWS credentials on your laptop to interact with the given Kubernetes cluster deployed on MCP.

## Post-Deployment Tasks

In order to run a high load on this Airflow deployment the following steps should be taken.

### Increase the default pool size

The following command will give you the current pool size set in your Airflow deployment:

```
kubectl exec -ti airflow-worker-0 -n sps -- bash -c 'airflow pools list'
```

Example output:

```
$ kubectl exec -ti airflow-worker-0 -n sps -- bash -c 'airflow pools list'                     Defaulted container "worker" out of: worker, worker-log-groomer, wait-for-airflow-migrations (init)
pool         | slots | description  | include_deferred
=============+=======+==============+=================
default_pool | 128   | Default pool | False
```

We will need to increase the pool size to `32768` by running the following command:

```
kubectl exec -ti airflow-worker-0 -n sps -- bash -c 'airflow pools set default_pool 32768 "Default pool"'
```

Rerun the `airflow pools list` command to verify the new value:

```
$ kubectl exec -ti airflow-worker-0 -n sps -- bash -c 'airflow pools list'                     Defaulted container "worker" out of: worker, worker-log-groomer, wait-for-airflow-migrations (init)
pool         | slots | description  | include_deferred
=============+=======+==============+=================
default_pool | 32768 | Default pool | False
```

### Mitigate DockerHub rate limits

By default, sidecar containers used in Airflow will use the `alpine:latest` docker image and download it from DockerHub. This may suffice for a small amount of dagRuns during development or during small production runs, however once the scale of the number of dagRuns increases, the system will quickly run into pods failing to start up because they are being throttled by DockerHub. To mitigate this issue, we will download the `alpine:latest` docker image from DockerHub, create a private ECR repo, retag the  downloaded image for import into the ECR repo, push the docker image, and finally configure the Airflow deployment to use our ECR repo for the alpine image.

#### Download the alpine:latest image

```
docker pull alpine:latest
```

#### Create the ECR repo

```
aws ecr create-repository --repository-name unity/alpine
```

This command will output information on the new repository you just created, e.g.:

```
{
    "repository": {
        "repositoryArn": "arn:aws:ecr:us-west-2:xxx:repository/unity/alpine",
        "registryId": "xxx",
        "repositoryName": "unity/alpine",
        "repositoryUri": "xxx.dkr.ecr.us-west-2.amazonaws.com/unity/alpine",
        "createdAt": "2025-06-17T09:08:53.556000-07:00",
        "imageTagMutability": "MUTABLE",
        "imageScanningConfiguration": {
            "scanOnPush": false
        },
        "encryptionConfiguration": {
            "encryptionType": "AES256"
        }
    }
}
```

Take note of the value for `repositoryUri`. In this example the actual registryId has been replaced with `xxx`.&#x20;

#### Retag the alpine:latest image

```
docker tag alpine:latest xxx.dkr.ecr.us-west-2.amazonaws.com/unity/alpine:latest
```

The new tag is the value of `repositoryUri` with the `latest` tag appended to it.

#### Push the retagged image to the ECR repo

First we must setup our docker credentials to be able to push up to the new ECR repo:

```
aws ecr get-login-password --region us-west-2 | docker login --username AWS --password-stdin xxx.dkr.ecr.us-west-2.amazonaws.com
```

Note that you only specify the DNS name portion of the `repositoryUri`. Once you've successfully logged in, you should see the command output:

```
Login Succeeded
```

Finally, push the retagged docker image up to the repo:

```
docker push xxx.dkr.ecr.us-west-2.amazonaws.com/unity/alpine:latest
```

Verify by logging into the AWS dashboard and visually inspect the repo for the tag you just pushed, e.g.:

<figure><img src="../../../../.gitbook/assets/Screenshot_2025-06-17_at_9_20_38 AM.png" alt=""><figcaption></figcaption></figure>

#### Update Airflow to use the ECR repo for its sidecar image

The following command will create a default airflow connection that uses the ECR repo for the sidecar image:

```
kubectl exec -ti airflow-worker-0 -n sps -- bash -c "airflow connections add kubernetes_default --conn-type kubernetes --conn-login admin --conn-password my_admin_password --conn-extra '{\"in_cluster\": false, \"namespace\": \"default\", \"disable_verify_ssl\": false, \"disable_tcp_keepalive\": false, \"xcom_sidecar_container_image\": \"xxx.dkr.ecr.us-west-2.amazonaws.com/unity/alpine:latest\"}'"
```

Note, replace `my_admin_password` with your admin password and replace `xxx` in the `xcom_sidecar_container_image` to point to your ECR repo. You should see output similar to the following:

```
Defaulted container "worker" out of: worker, worker-log-groomer, wait-for-airflow-migrations (init)
/home/airflow/.local/lib/python3.11/site-packages/airflow/configuration.py:859 FutureWarning: section/key [core/sql_alchemy_conn] has been deprecated, you should use[database/sql_alchemy_conn] instead. Please update your `conf.get*` call to use the new name
/home/airflow/.local/lib/python3.11/site-packages/airflow/metrics/statsd_logger.py:184 RemovedInAirflow3Warning: The basic metric validator will be deprecated in the future in favor of pattern-matching.  You can try this now by setting config option metrics_use_pattern_match to True.
[2025-06-17T16:27:14.818+0000] {providers_manager.py:287} INFO - Optional provider feature disabled when importing 'airflow.providers.google.leveldb.hooks.leveldb.LevelDBHook' from 'apache-airflow-providers-google' package
/home/airflow/.local/lib/python3.11/site-packages/snowflake/sqlalchemy/base.py:1068 SAWarning: The GenericFunction 'flatten' is already registered and is going to be overridden.
Successfully added `conn_id`=kubernetes_default : kubernetes://admin:******@:
```

Now visually verify the default connection by logging into the Airflow UI, clicking on the `Admin` tab at the top, then clicking on the `Connections` item in the dropdown list. You should see a single record with `Conn Id` of `kubernetes_default`. Click on the edit icon and the details of this connection should show that the `XCom sideare image` is pointing to your ECR repo, e.g.:

<figure><img src="../../../../.gitbook/assets/Screenshot_2025-06-17_at_9_29_42 AM.png" alt=""><figcaption></figcaption></figure>

Exit out without saving.

## Large Scale Processing of the SRL Image Processing Pipeline

In this tutorial, we will be exercising large scale processing in SPS/Airflow using the SRL pipeline use case. The following diagram depicts SRL's image processing pipeline, which chains together the following 3 PGE workflows:

* EDRgen
* vic2png
* RDRgen (marsinverter)

<figure><img src="../../../../.gitbook/assets/Demo SRL Pipeline (2).png" alt=""><figcaption></figcaption></figure>

We will also be exercising the initiation of this processing from raw data files (.DAT and .EMD pairs) that are uploaded to S3 and evaluation of requirement conditions before triggering each respective workflow.

### Push PGE docker images to ECR

We will need to create separate ECR repos for each of the 3 docker images to mitigate DockerHub throttling, but also to optimize docker image downloading. Perform the following commands to create these ECR repos:

#### Download the images

```
docker pull pymonger/srl-idps-edrgen:develop
docker pull pymonger/srl-idps-rdrgen:develop
docker pull pymonger/srl-idps-vic2png:develop
```

#### Create the ECR repos

```
aws ecr create-repository --repository-name unity/edrgen
aws ecr create-repository --repository-name unity/rdrgen
aws ecr create-repository --repository-name unity/vic2png
```



#### Retag the images

```
docker tag pymonger/srl-idps-edrgen:develop xxx.dkr.ecr.us-west-2.amazonaws.com/unity/edrgen:develop
docker tag pymonger/srl-idps-rdrgen:develop xxx.dkr.ecr.us-west-2.amazonaws.com/unity/rdrgen:develop
docker tag pymonger/srl-idps-vic2png:develop xxx.dkr.ecr.us-west-2.amazonaws.com/unity/vic2png:develop
```

#### Push the retagged images to their ECR repo

First we must setup our docker credentials to be able to push up to the new ECR repo:

```
aws ecr get-login-password --region us-west-2 | docker login --username AWS --password-stdin xxx.dkr.ecr.us-west-2.amazonaws.com
```

Note that you only specify the DNS name portion of the `repositoryUri`. Once you've successfully logged in, you should see the command output:

```
Login Succeeded
```

Finally, push the retagged docker image up to the repo:

```
docker push xxx.dkr.ecr.us-west-2.amazonaws.com/unity/edrgen:develop
docker push xxx.dkr.ecr.us-west-2.amazonaws.com/unity/rdrgen:develop
docker push xxx.dkr.ecr.us-west-2.amazonaws.com/unity/vic2png:develop
```

Verify by logging into the AWS dashboard and visually inspect the repos for the tags you just pushed.

### Install SRL DAGs into airflow

Next we will install the DAGs needed to exercise our large scale testing.

#### Checkout the scaling-test branch of the unity-sps repository

We will first clone the unity-sps repo and checkout the `scaling-test` branch:

```
git clone https://github.com/unity-sds/unity-sps.git
cd unity-sps
git checkout scaling-test
```

#### Copy the DAGs into airflow

```
cd airflow/dags
for i in edrgen.py rdrgen.py vic2png.py eval_srl_* router.py; do kubectl cp $i $(kubectl get pods -n sps | grep airflow-dag-processor | grep Running | awk '{print $1}' | head -1):/opt/airflow/dags/. -c dag-processor -n sps; done
```

Go back to the airflow UI and you should see the new set of installed DAGs. Click on the toggle buttons on the left to enable the newly installed DAGs:

<figure><img src="../../../../.gitbook/assets/Screenshot 2025-07-16 at 11.40.39 AM.png" alt=""><figcaption></figcaption></figure>

### Configure the router

Create a file named `srl_router.yaml` with the following content:

```
initiator_config:

  name: SRL Example

  payload_type:
    url:
      - regexes:
          - '(?<=/)(?P<apid>0980|0990)_(?P<sclk_seconds>\d{10})-(?P<sclk_subseconds>\d{5})-(?P<version>\d{1,3})(?P<extension>\.dat|\.emd)$'
        evaluators:
          - name: eval_srl_edrgen_readiness
            actions:
              - name: submit_dag_by_id
                params:
                  dag_id: eval_srl_edrgen_readiness
                  airflow_base_api_endpoint: http://internal-gman-6-dev-httpd-alb-1234567890.us-west-2.elb.amazonaws.com:8080/gman-6/dev/sps/api/v1
                  airflow_username: admin
                  airflow_password: admin
                  on_success:
                    actions:
                      - name: submit_dag_by_id
                        params:
                          dag_id: edrgen

      - regexes:
          - '(?<=/)(?P<apid>0980|0990)_(?P<sclk_seconds>\d{10})-(?P<sclk_subseconds>\d{5})-(?P<version>\d{1,3})(?P<extension>\.png)$'
        evaluators:
          - name: submit_vic2png_dag
            actions:
              - name: submit_dag_by_id
                params:
                  dag_id: vic2png
                  airflow_base_api_endpoint: http://internal-gman-6-dev-httpd-alb-1234567890.us-west-2.elb.amazonaws.com:8080/gman-6/dev/sps/api/v1
                  airflow_username: admin
                  airflow_password: admin

      - regexes:
          - '(?<=/)(?P<filename>hello_world\.txt)$'
        evaluators:
          - name: eval_hello_world_readiness
            actions:
              - name: submit_ogc_process_execution
                params:
                  process_id: eval_hello_world_readiness
                  #ogc_processes_base_api_endpoint: https://www.dev.mdps.mcp.nasa.gov:4443/gmanipon/dev/ogc/
                  airflow_base_api_endpoint: http://internal-gman-6-dev-httpd-alb-1234567890.us-west-2.elb.amazonaws.com:8080/gman-6/dev/sps/api/v1
                  on_success:
                    actions:
                      - name: submit_ogc_process_execution
                        params:
                          process_id: hello_world
```

Replace `http://internal-gman-6-dev-httpd-alb-1234567890.us-west-2.elb.amazonaws.com:8080/gman-6/dev/sps/api/v1` with the value that was returned after the terraform deployment of airflow above.

Next, let's find the initiator lambda that was installed when the venue was initially deployed. In this example, we deployed the venue using `unity-monorepo` with `PROJECT=gman-6` and `VENUE=dev`. Navigate to the Lambda page on the AWS dashboard and search for the initiator lambda by the function name `${PROJECT}-${VENUE}-initiator` . For example:

\
![](<../../../../.gitbook/assets/Screenshot 2025-07-16 at 11.46.43 AM.png>)

Click on that lambda then navigate to the `Configuration` tab and select the `Environment variables` item on the left menu. You should see the environment variable for `ROUTER_CFG_URL` defined there. Take note of the S3 url as we will overwrite that object with the `srl_router.yaml` we created above:

<figure><img src="../../../../.gitbook/assets/Screenshot 2025-07-16 at 11.48.39 AM.png" alt=""><figcaption></figcaption></figure>

Let's overwrite the old router configuration with the one we created:

```
aws s3 cp srl_router.yaml <value of ROUTER_CFG_URL>
```

## Demo Video of Large Scaling Test

[https://drive.google.com/file/d/1CZ1U0AOUy8bzm9HDWlq6YUVH75InzyEG/view?usp=sharing](https://drive.google.com/file/d/1CZ1U0AOUy8bzm9HDWlq6YUVH75InzyEG/view?usp=sharing)
