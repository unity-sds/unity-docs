# SPS-EKS Configuration

The SPS EKS Cluster marketplace application can be configured with the following user-defined (at install) variables:\


The following is the default configuration:

```json
"nodegroups": {
    "defaultGroup": {
        "block_device_mappings": {
            "xvda": {
                "device_name": "/dev/xvda",
                "ebs": {
                    "delete_on_termination": true,
                    "encrypted": true,
                    "volume_size": 100,
                    "volume_type": "gp2"
                }
            }
        },
        "desired_size": 1,
        "instance_types": [
            "t3.xlarge"
        ],
        "max_size": 1,
        "metadata_options": {
            "http_endpoint": "enabled",
            "http_put_response_hop_limit": 3
        },
        "min_size": 1
    }
}
```
