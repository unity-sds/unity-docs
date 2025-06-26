---
description: Sourced from https://api.mdps.mcp.nasa.gov/am-uds-dapa/openapi/
---

# Data Services API Specification

## API to manage collections

{% openapi src="../../../../.gitbook/assets/mdps.ds.openapi.user.2025.01.30.json" path="/am-uds-dapa/collections/" method="get" %}
[mdps.ds.openapi.user.2025.01.30.json](../../../../.gitbook/assets/mdps.ds.openapi.user.2025.01.30.json)
{% endopenapi %}

{% openapi src="../../../../.gitbook/assets/mdps.ds.openapi.user.2025.01.30.json" path="/am-uds-dapa/collections/{collection_id}/" method="get" %}
[mdps.ds.openapi.user.2025.01.30.json](../../../../.gitbook/assets/mdps.ds.openapi.user.2025.01.30.json)
{% endopenapi %}

{% openapi src="../../../../.gitbook/assets/mdps.ds.openapi.user.2025.01.30.json" path="/am-uds-dapa/collections/" method="post" %}
[mdps.ds.openapi.user.2025.01.30.json](../../../../.gitbook/assets/mdps.ds.openapi.user.2025.01.30.json)
{% endopenapi %}

{% openapi-operation spec="unity-sds-api" path="/am-uds-dapa/collections/{collection_id}/" method="delete" %}
[OpenAPI unity-sds-api](https://gitbook-x-prod-openapi.4401d86825a13bf607936cc3a9f3897a.r2.cloudflarestorage.com/raw/425adbcbbde81e093f43a179662ffa669fcb69784925ebc60290c82c939a47ac.txt?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=dce48141f43c0191a2ad043a6888781c%2F20250626%2Fauto%2Fs3%2Faws4_request&X-Amz-Date=20250626T042957Z&X-Amz-Expires=172800&X-Amz-Signature=64c034ad908e16d9754826c3304850cf9037e978159fee61aa47d2f85002f282&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)
{% endopenapi-operation %}

{% openapi src="../../../../.gitbook/assets/mdps.ds.openapi.user.2025.01.30.json" path="/am-uds-dapa/collections/{collection_id}/items/" method="get" %}
[mdps.ds.openapi.user.2025.01.30.json](../../../../.gitbook/assets/mdps.ds.openapi.user.2025.01.30.json)
{% endopenapi %}

{% openapi src="../../../../.gitbook/assets/mdps.ds.openapi.user.2025.01.30.json" path="/am-uds-dapa/collections/{collection_id}/variables/" method="get" %}
[mdps.ds.openapi.user.2025.01.30.json](../../../../.gitbook/assets/mdps.ds.openapi.user.2025.01.30.json)
{% endopenapi %}



## API to manage items&#x20;

{% openapi src="../../../../.gitbook/assets/mdps.ds.openapi.user.2025.01.30.json" path="/am-uds-dapa/collections/{collection_id}/items/{granule_id}/" method="get" %}
[mdps.ds.openapi.user.2025.01.30.json](../../../../.gitbook/assets/mdps.ds.openapi.user.2025.01.30.json)
{% endopenapi %}

{% openapi src="../../../../.gitbook/assets/mdps.ds.openapi.user.2025.01.30.json" path="/am-uds-dapa/collections/{collection_id}/items/{granule_id}/" method="delete" %}
[mdps.ds.openapi.user.2025.01.30.json](../../../../.gitbook/assets/mdps.ds.openapi.user.2025.01.30.json)
{% endopenapi %}

## API to manage custom metadata

{% openapi src="../../../../.gitbook/assets/mdps.ds.openapi.admin.2025.01.30.json" path="/am-uds-dapa/admin/custom_metadata/{tenant}/" method="get" %}
[mdps.ds.openapi.admin.2025.01.30.json](../../../../.gitbook/assets/mdps.ds.openapi.admin.2025.01.30.json)
{% endopenapi %}

{% openapi src="../../../../.gitbook/assets/mdps.ds.openapi.admin.2025.01.30.json" path="/am-uds-dapa/admin/custom_metadata/{tenant}/" method="put" %}
[mdps.ds.openapi.admin.2025.01.30.json](../../../../.gitbook/assets/mdps.ds.openapi.admin.2025.01.30.json)
{% endopenapi %}

{% openapi src="../../../../.gitbook/assets/mdps.ds.openapi.admin.2025.01.30.json" path="/am-uds-dapa/admin/custom_metadata/{tenant}/" method="delete" %}
[mdps.ds.openapi.admin.2025.01.30.json](../../../../.gitbook/assets/mdps.ds.openapi.admin.2025.01.30.json)
{% endopenapi %}

{% openapi src="../../../../.gitbook/assets/mdps.ds.openapi.admin.2025.01.30.json" path="/am-uds-dapa/admin/custom_metadata/{tenant}/destroy/" method="delete" %}
[mdps.ds.openapi.admin.2025.01.30.json](../../../../.gitbook/assets/mdps.ds.openapi.admin.2025.01.30.json)
{% endopenapi %}

## API to configure DAAC delivery&#x20;

{% openapi src="../../../../.gitbook/assets/mdps.ds.openapi.user.2025.01.30.json" path="/am-uds-dapa/collections/{collection_id}/archive/" method="get" %}
[mdps.ds.openapi.user.2025.01.30.json](../../../../.gitbook/assets/mdps.ds.openapi.user.2025.01.30.json)
{% endopenapi %}

{% openapi-operation spec="unity-sds-api" path="/am-uds-dapa/collections/{collection_id}/archive/" method="put" %}
[OpenAPI unity-sds-api](https://gitbook-x-prod-openapi.4401d86825a13bf607936cc3a9f3897a.r2.cloudflarestorage.com/raw/425adbcbbde81e093f43a179662ffa669fcb69784925ebc60290c82c939a47ac.txt?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=dce48141f43c0191a2ad043a6888781c%2F20250626%2Fauto%2Fs3%2Faws4_request&X-Amz-Date=20250626T042957Z&X-Amz-Expires=172800&X-Amz-Signature=64c034ad908e16d9754826c3304850cf9037e978159fee61aa47d2f85002f282&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)
{% endopenapi-operation %}

{% openapi-operation spec="unity-sds-api" path="/am-uds-dapa/collections/{collection_id}/archive/" method="post" %}
[OpenAPI unity-sds-api](https://gitbook-x-prod-openapi.4401d86825a13bf607936cc3a9f3897a.r2.cloudflarestorage.com/raw/425adbcbbde81e093f43a179662ffa669fcb69784925ebc60290c82c939a47ac.txt?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=dce48141f43c0191a2ad043a6888781c%2F20250626%2Fauto%2Fs3%2Faws4_request&X-Amz-Date=20250626T042957Z&X-Amz-Expires=172800&X-Amz-Signature=64c034ad908e16d9754826c3304850cf9037e978159fee61aa47d2f85002f282&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)
{% endopenapi-operation %}

{% openapi src="../../../../.gitbook/assets/mdps.ds.openapi.user.2025.01.30.json" path="/am-uds-dapa/collections/{collection_id}/archive/" method="delete" %}
[mdps.ds.openapi.user.2025.01.30.json](../../../../.gitbook/assets/mdps.ds.openapi.user.2025.01.30.json)
{% endopenapi %}

{% openapi src="../../../../.gitbook/assets/mdps.ds.openapi.user.2025.01.30.json" path="/am-uds-dapa/collections/{collection_id}/archive/{granule_id}/" method="put" %}
[mdps.ds.openapi.user.2025.01.30.json](../../../../.gitbook/assets/mdps.ds.openapi.user.2025.01.30.json)
{% endopenapi %}
