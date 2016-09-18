# Vagrant IE

This project features a Vagrantfile to provision various Internet Explorer environments for debugging. It's using the free [Microsoft VMs](https://developer.microsoft.com/en-us/microsoft-edge/tools/vms) and configures them to use the host DNS resolver and proxy configuration. It additionally supports the provisioning of the guests `hostfile`, so that you don't need to fiddle with system settings if you just want to crackdown a bug.

## Features

* Using host DNS resolver, proxy
* Provision guest hostfile

## Prerequisites

There are a few things you need before you can start debugging. Please make sure you've installed and properly configured the following software:

* [VirtualBox](https://www.virtualbox.org/)
* [Vagrant](https://www.vagrantup.com/)

## Start, Stop, Destroy

Use the following command to download, create and provision your debugging environments.

```shell
$ vagrant up ${environment}
```
_Available environments: `vista-ie7`, `w7-ie8`, `w7-ie10`, `w7-ie11`, `w8-ie10`, `w8.1-ie11`, `w10-edge`_

Use the following command to stop/halt the environment.

```shell
$ vagrant halt ${environment}
```

If you want to start over, use the following command to shut down the running environment and destroy all resources created during the machine creation process.

```shell
$ vagrant destroy ${environment}
```

For a full list of commands and vagrant options, please see:
(https://docs.vagrantup.com/v2/cli/index.html[https://docs.vagrantup.com/v2/cli/index.html]

## Configuration

### `hostsfile`

You can either adjust the `hostfile.erb` to your needs or use a different template by defining the `DEBUG_IE_HOSTSFILE_TEMPLATE` environment variable.

_Note: 10.0.2.2 always points to the host environment._

#### Example: Docker Host (docker-machine)

This example illustrates how to use a custom template to point the debug environment to local running docker-machine.

1. Create a file named `dockerhost.erb` with the new template by running this command:

```shell
$ echo "<%= ENV['DOCKER_HOST'][/([\d.]+)/,1] %> example.docker" > dockerhost.erb
```

2. Define the `DEBUG_IE_HOSTSFILE_TEMPLATE environment variable and point it to the new template.

```shell
$ export DEBUG_IE_HOSTSFILE_TEMPLATE="$(pwd)/dockerhost.erb"
```

3. Use the following command creates and provisions the debugging environments.

```shell
$ vagrant up ${environment}
```

## License

MIT
