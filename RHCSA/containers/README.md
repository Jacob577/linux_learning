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

To see the images on the system, we can run the command: `podman images`

To then run the container:
```bash
podman --name my_pod -dt -p 8080:80/tcp docker.io/library/httpd
```
With `podman ps` we will see all of the running containers, if we add the flag `-a` we will see the history of containers killed etc.

### Inspect container images
To inspect a container image, we need the command: `podman inspect`. A real world scenario would be if we needed to inspect our previously pulled container `docker.io/library/httpd:latest`.
```bash
podman inspect docker.io/library/httpd:latest
```


### Perform container management using commands such as podman and skopeo
### Build a container from a Containerfile
To build a container from an image, we will have to use the command:
```bash
podman build -t <container-name> <path/to/destination>
```
### Perform basic container management such as running, starting, stopping, and listing running containers
To see the logs, we can use `podman logs`
To see what is currently using capacity on podman: `podman top <container-id>`
We can offcourse use `stop, start and create` pods as well. 
To remove pods, we can use `rm`
### Run a service inside a container
To run a service within a container, we'll need to get into a container:
```bash
# Get shell
podman exec -it <pod-name> /bin/bash
```
### Configure a container to start automatically as a systemd service
We need to set a boolean in SELinux for a pod to start on start:
`setsebool -P container_manager_cgroup on`

To generate a unit file for the pod, we will run:
```bash
podman generate systemd --new --name <hello_pod> > /etc/systemd/system/container-hello_pod.service

# If the container should run rootless, we can instead move it to a users directory:
/home/name/.config/systemd/user/container-hello_pod.service

# Reload systemctl daemon
systemctl daemon-reload

# Then we will have to enable it
systemctl enable --now container-hello_pod.service
```
### Attach persistent storage to a container
To attach persistant storage, use:
```bash
podman run --privileged -it -v /home/jacob/containers/disk1:/mnt docker.io/library/httpd /bin/bash
# This way, we'll also get a terminal

# Another way of attaching persistant storage
podman run -dit --volume /container_drive:/mnt docker.io/clearlinux/httpd 

# Remember to add the z, otherwise container won't be able to access the storage
podman run -dit --volume /container_drive:/home:z docker.io/clearlinux/httpd

```

### Final task
start an apache container running on port 8080, enable it accross reboots and assign it to the user jacob and add a file in the containers persistand storage which say success! 