## Manage containers

- Find and retrieve container images from a remote registry
- Inspect container images
- Perform container management using commands such as podman and skopeo
- Build a container from a Containerfile
- Perform basic container management such as running, starting, stopping, and listing running containers
- Run a service inside a container
- Configure a container to start automatically as a systemd service
- Attach persistent storage to a container

### Find and retrieve container images from a remote registry
Whenever we'd like to find containers in a registry, we need to search within the registry, suggestively using `podman search <service> | grep -ni <service-name>`. 

To pull the container:
```bash
podman pull docker.io/library/httpd
```
### Inspect container images
### Perform container management using commands such as podman and skopeo
### Build a container from a Containerfile
### Perform basic container management such as running, starting, stopping, and listing running containers
### Run a service inside a container
### Configure a container to start automatically as a systemd service
### Attach persistent storage to a container