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
$ npm install @nutanix-api/aiops-js-client
$ npm install @nutanix-api/vmm-js-client
$ npm install @nutanix-api/iam-js-client

...etc..

```

These modules can be used in the CommonJS (CJS) , ECMAScript6(ES6) and Universal Module Definition(UMD) formats. While using the module in browser frameworks like React or Vue.JS , you can use import/export. Older Node.js versions can continue to use the require ().

For CommonJs:

```javascript
const sdk = require("@nutanix-api/iam-js-client/dist/lib/index");
let client = new sdk.ApiClient();
```

For ES6:

```javascript
import ApiClient from "@nutanix-api/iam-js-client/dist/es/ApiClient";
```

And consume it as...

```javascript
import ApiClient from "@nutanix-api/iam-js-client/dist/es/ApiClient";

let client = new ApiClient();
client.host = '10.19.50.27'; // IPv4/IPv6 address or FQDN of the cluster
client.port = 9440; // Port to which to connect to
client.username = 'admin'; // UserName to connect to the cluster
client.password = 'password'; // Password to connect to the cluster
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
