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

## License

MIT
