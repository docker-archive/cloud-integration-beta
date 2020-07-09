# Changelog

<!-- TEMPLATE
## x.y.z - YYYY-MM-DD

Release headlines

### Added
*

### Changed
*

### Removed
*

### Fixed
*

### Known issues
*

[Release diff](https://github.com/docker/api/compare/<LAST TAG>...<THIS TAG>)
-->

## 0.1.7 - 2020-07-09

### New features

* Support for docker logs -- follow to follow logs
* docker run ... will attach to logs by default, if the user does not specify -d
* Support for environment variables (#11)
* Support for CPU/Memory limits (#7)

### Bug fixes

* Login with azure multi-tenant accounts (#8)
* Bug fixed deploying ACR images on Linux (#10)

## 0.1.4 - 2020-06-26

First public beta release of the Docker CLI with
[ACI](https://azure.microsoft.com/en-us/services/container-instances/)
integration!

This release includes:

* Initial support for deploying containers and Compose applications to Azure Container Instances (ACI)
* A gRPC API for managing contexts and Azure containers

### Known issues

* Mapping a container port to a different host port is not supported in ACI (i.e.: `docker run -p 80:8080`). You can only expose the container port to the same port on the host.
* Exec currently only allows interactive sessions with a terminal (`exec -t`), not specify commands in the command line.
* `docker run` detaches from the container by default, even if `-d` is not specified. Logs can be seen later on with command `docker log <CONTAINER_ID>`.
* Replicas are not supported when deploying Compose application. One container will be run for each Compose service. Several services cannot expose the same port.
* Name resolution between Compose services is done using the `/etc/hosts` file. If container code ignores `/etc/hosts`, the container will not be able to resolve other Compose services in the same application.
* With an ACI context, Compose applications can be deployed with the new command `docker compose up`. The `docker-compose` command will fail if used with ACI contexts. The error message is not explicit and needs to be improved.
* Windows containers are not supported on ACI in multi-container Compose applications.
