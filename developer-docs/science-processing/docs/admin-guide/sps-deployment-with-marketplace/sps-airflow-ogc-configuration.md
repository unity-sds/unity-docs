# SPS Airflow/OGC Configuration

The SPS Airflow/OGC marketplace deployment is currently configured with the following default variables, which may be changed during installation

```json
"airflow_docker_images": {
	"airflow": {
		"name": "ghcr.io/unity-sds/unity-sps/sps-airflow",
		"tag": "2.2.0"
	}
},
"dag_catalog_repo": {
	"dags_directory_path": "airflow/dags",
	"ref": "develop",
	"url": "https://github.com/unity-sds/unity-sps.git"
},
"helm_charts": {
	"airflow": {
		"chart": "airflow",
		"repository": "https://airflow.apache.org",
		"version": "1.13.1"
	},
	"keda": {
		"chart": "keda",
		"repository": "https://kedacore.github.io/charts",
		"version": "v2.14.2"
	}
},
"karpenter_node_classes": {
	"airflow-kubernetes-pod-operator-high-workload": {
		"volume_size": "300Gi"
	},
	"default": {
		"volume_size": "30Gi"
	}
},
"ogc_processes_docker_images": {
	"git_sync": {
		"name": "registry.k8s.io/git-sync/git-sync",
		"tag": "v4.2.4"
	},
	"ogc_processes_api": {
		"name": "ghcr.io/unity-sds/unity-sps-ogc-processes-api/unity-sps-ogc-processes-api",
		"tag": "2.0.0"
	},
	"redis": {
		"name": "redis",
		"tag": "7.4.0"
	}
}
```

SPS Karpenter deployments **require** previous deployment of SPS-EKS and SPS-Karpenter in a management-console project/venue.
