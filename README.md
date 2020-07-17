# Docker ACI Integration (Beta)

> :warning: **This CLI is in beta**: The installation process, commands, and
> flags will change in future releases.

Docker has extended its strategic collaboration with Microsoft to simplify code
to cloud application development for developers and development teams by more
closely integrating with Azure Container Instances (ACI).

The new ACI experience from local VS Code and Docker Desktop development to
remote deployment in ACI creates a tighter integration between Docker and
Microsoft developer technologies provides the following productivity benefits to
developers:

* Easily log into Azure directly from the Docker CLI
* Trigger an ACI cloud container service environment to be set up automatically
  with easy to use defaults and no infrastructure overhead
* Switch from a local context to a cloud context to quickly and easily run
  applications
* Simplifies single container and multi-container application development via
  the Compose specification allowing a developer to invoke fully Docker
  compatible commands seamlessly for the first time natively within a cloud
  container service

To learn more, take a look at the
[announcement blog post](https://www.docker.com/blog/running-a-container-in-aci-with-docker-desktop-edge/)

## FAQ

* [Where's the source code?](https://github.com/docker/aci-integration-beta/issues/1)
* When using an ACI context:
  * [Why can't I list images?](https://github.com/docker/aci-integration-beta/issues/2)
  * [How do I build images?](https://github.com/docker/aci-integration-beta/issues/3)
  * [Why aren't all the existing Docker commands and flags available?](https://github.com/docker/aci-integration-beta/issues/4)
  * [Why can't I map container ports?](https://github.com/docker/aci-integration-beta/issues/5)
  * [Is Compose v2 supported?](https://github.com/docker/aci-integration-beta/issues/24)

## macOS and Windows installation

The ACI integration is built into Docker Desktop **Edge**.
You can download it from these links:
- [macOS](https://hub.docker.com/editions/community/docker-ce-desktop-mac)
- [Windows](https://hub.docker.com/editions/community/docker-ce-desktop-windows)

## Ubuntu Linux installation

The Linux installation script and manual install instructions have been tested
with a fresh install of Ubuntu 20.04.

### Prerequisites

* [Docker 19.03 or later](https://docs.docker.com/engine/install/)

### Install script

You can install the new CLI using the install script:

```console
curl -L https://github.com/docker/aci-integration-beta/blob/main/scripts/install_linux.sh | sh
```

### Manual install

You can download the Docker ACI Integration CLI from [latest release](https://github.com/docker/aci-integration-beta/releases/latest).

You will then need to make it executable:

```console
chmod +x docker-aci
```

To enable using the local Docker Engine and to use existing Docker contexts, you
will need to have the existing Docker CLI as `com.docker.cli` somewhere in your
`PATH`. You can do this by creating a symbolic link from the existing Docker
CLI.

```console
ln -s /path/to/existing/docker /directory/in/PATH/com.docker.cli
```

> **Note**: The `PATH` environment variable is a colon separated list of
> directories with priority from left to right. You can view it using
> `echo $PATH`. You can find the path to the existing Docker CLI using
> `which docker`. You may need root permissions to make this link.

On a fresh install of Ubuntu 20.04 with Docker Engine
[already installed](https://docs.docker.com/engine/install/ubuntu/):

```console
$ echo $PATH
/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin
$ which docker
/usr/bin/docker
$ sudo ln -s /usr/bin/docker /usr/local/bin/com.docker.cli
```

You can verify that this is working by checking that the new CLI works with the
default context:

```console
$ ./docker-aci --context default ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
$ echo $?
0
```

To make this CLI with ACI integration your default Docker CLI, you must move it
to a directory in your `PATH` with higher priority than the existing Docker CLI.

Again on a fresh Ubuntu 20.04:

```console
$ which docker
/usr/bin/docker
$ echo $PATH
/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin
$ sudo mv docker-aci /usr/local/bin/docker
$ which docker
/usr/local/bin/docker
$ docker version
...
 Azure integration  0.1.4
...
```

## Uninstall

To remove this CLI, you need to remove the binary you downloaded and
`com.docker.cli` from your `PATH`. If you installed using the script, this can
be done as follows:

```console
sudo rm /usr/local/bin/docker /usr/local/bin/com.docker.cli
```

## Testing the install script

To test the install script, from a machine with docker:

```console
docker build -t testclilinux -f scripts/Dockerfile-testInstall scripts
```
