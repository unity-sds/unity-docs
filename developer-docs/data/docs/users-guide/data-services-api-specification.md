---
description: Sourced from https://api.mdps.mcp.nasa.gov/am-uds-dapa/openapi/
---

# Data Services API Specification

## API to manage collections

{% swagger src="../../../../.gitbook/assets/mdps.ds.openapi.user.2025.01.30.json" path="/am-uds-dapa/collections/" method="get" %}
[mdps.ds.openapi.user.2025.01.30.json](../../../../.gitbook/assets/mdps.ds.openapi.user.2025.01.30.json)
{% endswagger %}

{% swagger src="../../../../.gitbook/assets/mdps.ds.openapi.user.2025.01.30.json" path="/am-uds-dapa/collections/{collection_id}/" method="get" %}
[mdps.ds.openapi.user.2025.01.30.json](../../../../.gitbook/assets/mdps.ds.openapi.user.2025.01.30.json)
{% endswagger %}

{% swagger src="../../../../.gitbook/assets/mdps.ds.openapi.user.2025.01.30.json" path="/am-uds-dapa/collections/" method="post" %}
[mdps.ds.openapi.user.2025.01.30.json](../../../../.gitbook/assets/mdps.ds.openapi.user.2025.01.30.json)
{% endswagger %}

{% swagger src="../../../../.gitbook/assets/mdps.ds.openapi.user.2025.01.30.json" path="/am-uds-dapa/collections/{collection_id}/items/" method="get" %}
[mdps.ds.openapi.user.2025.01.30.json](../../../../.gitbook/assets/mdps.ds.openapi.user.2025.01.30.json)
{% endswagger %}

{% swagger src="../../../../.gitbook/assets/mdps.ds.openapi.user.2025.01.30.json" path="/am-uds-dapa/collections/{collection_id}/variables/" method="get" %}
[mdps.ds.openapi.user.2025.01.30.json](../../../../.gitbook/assets/mdps.ds.openapi.user.2025.01.30.json)
{% endswagger %}



## API to manage items&#x20;

{% swagger src="../../../../.gitbook/assets/mdps.ds.openapi.user.2025.01.30.json" path="/am-uds-dapa/collections/{collection_id}/items/{granule_id}/" method="get" %}
[mdps.ds.openapi.user.2025.01.30.json](../../../../.gitbook/assets/mdps.ds.openapi.user.2025.01.30.json)
{% endswagger %}

{% swagger src="../../../../.gitbook/assets/mdps.ds.openapi.user.2025.01.30.json" path="/am-uds-dapa/collections/{collection_id}/items/{granule_id}/" method="delete" %}
[mdps.ds.openapi.user.2025.01.30.json](../../../../.gitbook/assets/mdps.ds.openapi.user.2025.01.30.json)
{% endswagger %}

## API to manage custom metadata

{% swagger src="../../../../.gitbook/assets/mdps.ds.openapi.admin.2025.01.30.json" path="/am-uds-dapa/admin/custom_metadata/{tenant}/" method="get" %}
[mdps.ds.openapi.admin.2025.01.30.json](../../../../.gitbook/assets/mdps.ds.openapi.admin.2025.01.30.json)
{% endswagger %}

{% swagger src="../../../../.gitbook/assets/mdps.ds.openapi.admin.2025.01.30.json" path="/am-uds-dapa/admin/custom_metadata/{tenant}/" method="put" %}
[mdps.ds.openapi.admin.2025.01.30.json](../../../../.gitbook/assets/mdps.ds.openapi.admin.2025.01.30.json)
{% endswagger %}

{% swagger src="../../../../.gitbook/assets/mdps.ds.openapi.admin.2025.01.30.json" path="/am-uds-dapa/admin/custom_metadata/{tenant}/" method="delete" %}
[mdps.ds.openapi.admin.2025.01.30.json](../../../../.gitbook/assets/mdps.ds.openapi.admin.2025.01.30.json)
{% endswagger %}

{% swagger src="../../../../.gitbook/assets/mdps.ds.openapi.admin.2025.01.30.json" path="/am-uds-dapa/admin/custom_metadata/{tenant}/destroy/" method="delete" %}
[mdps.ds.openapi.admin.2025.01.30.json](../../../../.gitbook/assets/mdps.ds.openapi.admin.2025.01.30.json)
{% endswagger %}

## API to configure DAAC delivery&#x20;

{% swagger src="../../../../.gitbook/assets/mdps.ds.openapi.user.2025.01.30.json" path="/am-uds-dapa/collections/{collection_id}/archive/" method="get" %}
[mdps.ds.openapi.user.2025.01.30.json](../../../../.gitbook/assets/mdps.ds.openapi.user.2025.01.30.json)
{% endswagger %}

{% swagger src="../../../../.gitbook/assets/mdps.ds.openapi.user.2025.01.30.json" path="/am-uds-dapa/collections/{collection_id}/archive/" method="put" %}
[mdps.ds.openapi.user.2025.01.30.json](../../../../.gitbook/assets/mdps.ds.openapi.user.2025.01.30.json)
{% endswagger %}

{% swagger src="../../../../.gitbook/assets/mdps.ds.openapi.user.2025.01.30.json" path="/am-uds-dapa/collections/{collection_id}/archive/" method="post" %}
[mdps.ds.openapi.user.2025.01.30.json](../../../../.gitbook/assets/mdps.ds.openapi.user.2025.01.30.json)
{% endswagger %}

{% swagger src="../../../../.gitbook/assets/mdps.ds.openapi.user.2025.01.30.json" path="/am-uds-dapa/collections/{collection_id}/archive/" method="delete" %}
[mdps.ds.openapi.user.2025.01.30.json](../../../../.gitbook/assets/mdps.ds.openapi.user.2025.01.30.json)
{% endswagger %}

{% swagger src="../../../../.gitbook/assets/mdps.ds.openapi.user.2025.01.30.json" path="/am-uds-dapa/collections/{collection_id}/archive/{granule_id}/" method="put" %}
[mdps.ds.openapi.user.2025.01.30.json](../../../../.gitbook/assets/mdps.ds.openapi.user.2025.01.30.json)
{% endswagger %}
