# aws.greengrass.labs.nodered.docker

This component deploys [Node-RED](https://nodered.org/) onto a Greengrass device using the nodered/node-red container from Docker Hub. The Node-Red environment has access to all the environment variable resources set by Greengrass which allows to interact with Greengrass services and other component such as TokenExchangeServer and StreamManager. For this it uses `host` networking mode. If you prefer running the container with `bridge` networking or dedicated networking you might not be able to access external services without additional configuration of the host.

To allow this component to use Greengrass IPC you need to add an `accessControl` section in the component configuration as explained in [Authorize components to perform IPC operations](https://docs.aws.amazon.com/greengrass/v2/developerguide/interprocess-communication.html#ipc-authorization-policies).

This component installs a specific version of Node-RED and for consistency the component version tracks the Node-RED version being deployed.

To remotely deploy flows to Greengrass Core devices running this component you can use the [Greengrass CLI for Node-RED](https://github.com/awslabs/aws-greengrass-labs-node-red-app-cli).

## Versions

This component has the following versions:

- 4.0.2
- 2.1.3

## Type

This component is a generic component. The [Greengrass nucleus](https://docs.aws.amazon.com/greengrass/v2/developerguide/greengrass-nucleus-component.html) runs the component's lifecycle scripts.

For more information, see [component types](https://docs.aws.amazon.com/greengrass/v2/developerguide/manage-components.html#component-types)

## Requirements

### Docker

This component requires Docker to be present on the device running Greengrass. Install it according to the operating system instructions. Ensure that the user running the component is also allowed to execute the `docker` command: on Linux it is normally enough to add the user to the `docker` group.

### Component RunAs

The default user in the NodeRed docker container is `1000:1000`. In order for the container to be able to access the persisted `/data` folder on the device, the component needs to run as the same user. Specify `"runAs":"1000:1000"` in the deployment configuration of this component.
For the same reason also the component `aws.greengrass.labs.nodered.auth` needs to be deployed with `"runAs":"1000:1000"`.
If you are using the console to configure the deployment options of each component, enter `1000` as **Linux System User** and **Linux System Group**.

## Dependencies

When you deploy a component, AWS IoT Greengrass also deploys compatible versions of its dependencies. This means that you must meet the requirements for the component and all of its dependencies to successfully deploy the component. This section lists the dependencies for the released versions of this component and the semantic version constraints that define the component versions for each dependency. You can also view the dependencies for each version of the component in the [AWS IoT Greengrass console](https://console.aws.amazon.com/greengrass). On the component details page, look for the Dependencies list.

### v4.0.2

| Dependency                       | Compatible versions | Dependency type |
| -------------------------------- | ------------------- | --------------- |
| Greengrass nucleus               | >=2.0.0 <2.12.0     | Soft            |
| Token exchange service           | >=0.0.0             | Hard            |
| Docker Application Manager       | >=0.0.0             | Soft            |
| aws.greengrass.labs.nodered.auth | ~1.0.0              | Hard            |

## Configuration

This component provides the following configuration parameters that you can customize when you deploy the component.

`headless`

Configure Node-RED to run in headless mode. No UI will be available.

## Local log file

This component uses the following log file:

```bash
/greengrass/v2/logs/aws.greengrass.labs.nodered.docker.log
```

## Changelog

The following table describes the changes in each version of the component.

| Version | Changes                            |
| ------- | ---------------------------------- |
| 4.0.2   | Update to Node-RED 4.0.2           |
|         | Fixes to authentication            |
|         | Fix running host in container      |
| 2.1.3   | Initial version for Node-RED 2.1.3 |

## Thank you

This component would have not been possible without the great work done by Node-RED at [nodered.org](https://nodered.org/) and the effort put in creating the official docker containers at https://hub.docker.com/r/nodered/node-red.
