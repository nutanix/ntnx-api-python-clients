# Python Client For Nutanix Files Versioned APIs

The Python client for Nutanix Files Versioned APIs is designed for Python client application developers offering them simple and flexible access to APIs that manage virtual file servers, create and configure shares for client access, protect them using DR and sync policies, provision storage space and administer security controls.
## Features
- Invoke Nutanix APIs with a simple interface.
- Handle Authentication seamlessly.
- Reduce boilerplate code implementation.
- Use standard methods for installation.

## Version
- API version: v4.0.a2
- Package version: 4.0.1a2

## Requirements.
Python 3.6, 3.7, and 3.8 are fully supported and tested.


## Installation & Usage

### Installing in a virtual environment
[virtualenv](https://virtualenv.pypa.io/en/latest/) is a tool to create isolated Python environments. The basic problem it addresses is one of dependencies and versions, and indirectly permissions. virtualenv can help you install this client without needing system install permissions. It creates an environment that has its own installation directories without sharing libraries with other virtualenv environments or the system installation.

#### Mac/Linux
To install virtualenv via pip run:
```sh
$ pip3 install virtualenv
```
Create the virtualenv and activate it
```sh
$ virtualenv -p python3 <my-env>
$ source <my-env>/bin/activate
```
Install the Nutanix client into the virtualenv
```sh
<my-env>/bin/pip install ntnx-files-py-client
```

#### Windows
To install virtualenv via pip run:
```sh
> pip install virtualenv
```
Create the virtualenv and activate it
```sh
> virtualenv <my-env>
> myenv\Scripts\activate
```
Install the Nutanix SDK into the virtualenv
```sh
<your-env>\Scripts\pip.exe install ntnx-files-py-client
```

Then import the package:
```python
import ntnx_files_py_client
```

## Getting Started

## Configuration
The python client for Nutanix Files Versioned APIs can be configured with the following parameters

| Parameter | Description                                                                      | Required | Default Value|
|-----------|----------------------------------------------------------------------------------|----------|--------------|
| host      | IPv4/IPv6 address or FQDN of the cluster to which the client will connect to     | Yes      | N/A          |
| port      | Port on the cluster to which the client will connect to                          | No       | 9440         |
| username  | Username to connect to a cluster                                                 | Yes      | N/A          |
| password  | Password to connect to a cluster                                                 | Yes      | N/A          |
| debug     | Runs the client in debug mode if specified                                       | No       | False        |
| verify_ssl| Verify SSL certificate of cluster the client will connect to                     | No       | True         |
| max_retry_attempts| Maximum number of retry attempts while connecting to the cluster         | No       | 5            |
| backoff_factor| A backoff factor to apply between attempts after the second try.             | No       | 3            |
| logger_file | File location to which debug logs are written to                               | No       | N/A          |
| timeout   | Global timeout in milliseconds for all operations                                | No       | 30000        |


### Sample Configuration
```python
config = Configuration()
config.host = '10.19.50.27' # IPv4/IPv6 address or FQDN of the cluster
config.port = 9440 # Port to which to connect to
config.username = 'admin' # UserName to connect to the cluster
config.password = 'password' # Password to connect to the cluster
api_client = ApiClient(configuration=config)
```

### Authentication
Nutanix APIs currently support HTTP Basic Authentication only, and the Python client can be configured using the username and password parameters to send Basic headers along with every request.

### Additional Headers
The python client can be configured to send additional headers on each request.

```python
client = ApiClient(configuration=config)
client.add_default_header(header_name='Accept-Encoding', header_value='gzip, deflate, br')
```

### Retry Mechanism
The python client can be configured to retry requests that fail with the following status codes. The numbers of seconds before which the next retry is attempted is determined using the following formula:

```{backoff factor} * (2 * ({number of retries so far} - 1))```

- [408 - Request Timeout](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/408)
- [502 - Bad Gateway](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/502)
- [503 - Service Unavailable](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/503)

```python
config = Configuration()
config.max_retry_attempts = 3 # Max retry attempts while reconnecting on a loss of connection
config.backoff_factor = 3 # Backoff factor to use during retry attempts
client = ApiClient(configuration=config)
```

## Usage

### Invoking an operation
```python
# Initialize the API
admin_users_api_instance = AdminUsersApi(api_client=client) # client configured in previous step
extId = 'extId_example' # UUID.

# Get admin user by extId
try:
    api_response = admin_users_api_instance.get_admin_user_by_ext_id(extId)
except ApiException as e:
```

### Setting headers for individual operations
Headers can be configured globally on the python client using the [method to set default headers](#additional-headers). However, sometimes headers need to be set on an individual operation basis. Nutanix APIs require that concurrent updates are protected using [ETag headers](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/ETag).

```python
# Initialize the API
admin_users_api_instance = AdminUsersApi(api_client=client) # client configured in previous step
extId = 'extId_example' # UUID.

# Get admin user by extId
try:
    api_response = admin_users_api_instance.get_admin_user_by_ext_id(extId)
except ApiException as e:

# Extract E-Tag Header
etag_value = ApiClient.get_etag(api_response)

# Update admin user
try:
    # The body parameter in the following operation is received from the previous GET request's response which needs to be updated.
    api_response = admin_users_api_instance.update_admin_user(body, extId, if_match=etag_value) # Use the extracted etag value
except ApiException as e:
```

### List Operations
List Operations for Nutanix APIs support pagination, filtering, sorting and projections. The table below details the parameters that can be used to set the options for pagination etc.

| Parameter | Description
|-----------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| _page     | specifies the page number of the result set. Must be a positive integer between 0 and the maximum number of pages that are available for that resource. Any number out of this range will be set to its nearest bound. In other words, a page number of less than 0 would be set to 0 and a page number greater than the total available pages would be set to the last page.|
| _limit    | specifies the total number of records returned in the result set. Must be a positive integer between 0 and 100. Any number out of this range will be set to the default maximum number of records, which is 100. |
| _filter   | allows clients to filter a collection of resources. The expression specified with $filter is evaluated for each resource in the collection, and only items where the expression evaluates to true are included in the response. Expression specified with the $filter must conform to the [OData V4.01 URL](https://docs.oasis-open.org/odata/odata/v4.01/odata-v4.01-part2-url-conventions.html#sec_SystemQueryOptionfilter) conventions. |
| _orderby  | allows clients to specify the sort criteria for the returned list of objects. Resources can be sorted in ascending order using asc or descending order using desc. If asc or desc are not specified the resources will be sorted in ascending order by default. For example, 'orderby=templateName desc' would get all templates sorted by templateName in desc order. |
| _select   | allows clients to request a specific set of properties for each entity or complex type. Expression specified with the $select must conform to the OData V4.01 URL conventions. If a $select expression consists of a single select item that is an asterisk (i.e. *), then all properties on the matching resource will be returned. |
| _expand   | allows clients to request related resources when a resource that satisfies a particular request is retrieved. Each expand item is evaluated relative to the entity containing the property being expanded. Other query options can be applied to an expanded property by appending a semicolon-separated list of query options, enclosed in parentheses, to the property name. Allowed system query options are $filter,$select, $orderby. |

```python
# Initialize the API
admin_users_api_instance = AdminUsersApi(api_client=client) # client configured in previous step
extId = 'extId_example' # UUID.

# List admin users
try:
    api_response = admin_users_api_instance.get_admin_users(
	                   _page=page, # if page parameter is present
	                   _limit=limit, # if limit parameter is present
	                   _filter=_filter, # if filter parameter is present
	                   _orderby=_orderby, # if orderby parameter is present
	                   _select=select, # if select parameter is present
	                   _expand=expand) # if expand parameter is present
except ApiException as e:

```
The list of filterable and sortable fields with expansion keys can be found in the documentation [here](https://developers.nutanix.com/).

## API Reference

This library has a full set of [API Reference Documentation](https://developers.nutanix.com/). This documentation is auto-generated, and the location may change.

## License
This library is licensed under Nutanix proprietary license. Full license text is available in [LICENSE](https://developers.nutanix.com/license).

## Contact us
In case of issues please reach out to us at the [mailing list](@sdk@nutanix.com)