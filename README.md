# Python Client Libraries for Nutanix APIs

This project contains steps  for installing and using Python Client Libraries for Nutanix APIs. Clients
are available for the following namespaces:

| Namespace    | Description                                                                                         |
|--------------|-----------------------------------------------------------------------------------------------------|
| vmm          | Manage the life-cycle of virtual machines hosted on Nutanix clusters.                                        |
| prism        | Manage Tasks, Category Associations, Alerts, Alert policies, Events and Audits.|
| clustermgmt  | Manage Hosts, Clusters, and other Nutanix infrastructure.                                                    |
| aiops        | Manage Nutanix infrastructure using Analysis, Reporting, Capacity Planning, What if Analysis, VM Rightsizing, Troubleshooting, App Discovery, Broad Observability, and Ops Automation through Playbooks.|
| storage      | Manage Volume Groups and Storage Containers hosted on Nutanix clusters.                                     |
| iam          | Manage User Identity and Access.                                                                     |
| lcm          | Manage Infrastructure, Software and Firmware Upgrades. |
| files        | Manage virtual file servers, create and configure shares for client access, protect them using DR and sync policies, provision storage space and administer security controls.|
| networking   | Manage networking configuration on Nutanix clusters, including AHV and advanced networking.|

## Getting Started

The libraries are distributed on [PyPi](https://pypi.org/). In order to add it as a dependency, run the following command:

```shell
$ pip install ntnx-storage-py-client
$ pip install ntnx-vmm-py-client
$ pip install ntnx-iam-py-client

...etc..

```


And consume it as...

```python
config = Configuration()
config.host = '10.19.50.27' # IPv4/IPv6 address or FQDN of the cluster
config.port = 9440 # Port to which to connect to
config.username = 'admin' # UserName to connect to the cluster
config.password = 'password' # Password to connect to the cluster
api_client = ApiClient(configuration=config)

```

For detailed instructions on installing individual clients, please refer to the README documentation for the respective clients in the namespace directories.


## Status
These are auto generated Python clients generated from Open API v3.0 yaml specification documents.
Due to the auto-generated nature of these clients, they may contain breaking changes from one release to
the next.

## API Reference
These clients have a full set of [API Reference Documentation](https://developers.nutanix.com/). This documentation is auto-generated, and the location may change.

## License
This library is licensed under Nutanix proprietary license. Full license text is available in [LICENSE](https://developers.nutanix.com/license).

## Contact us
In case of issues please reach out to us at the [mailing list](mailto:sdk@nutanix.com).
