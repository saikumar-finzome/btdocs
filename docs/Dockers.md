# Dockers

## List of Docker Commands

### Docker Container Management Commands

This section features the essential commands related to the lifecycle of Docker containers. Learn how to create, manage,
and remove containers from your Docker system using the below commands.
![Image title](https://phoenixnap.com/kb/wp-content/uploads/2022/12/container-management-cheat-sheet-pnap.png)

See the containers currently running on the system:

```console
docker ps
```

See all the containers, both running and non-running:

```console
docker ps -a
```

Create a container (without starting it):

``` console
docker create [image]
```

Create an interactive container with pseudo-TTY:

```console
docker create -it [image]
```

Rename an existing container:

```console
docker rename [container] [new-name]
```

Delete a container (if it is not running):

```console
docker rm [container]
```

Forcefully remove a container, even if it is running:

```console
docker rm -f [container]
```

View logs for a running container:

```console
docker logs [container]
```

Retrieve logs created before a specific point in time:

```console
docker logs -f --until=[interval] [container]
```

View real-time events for a container:

```console
docker events [container]
```

Update the configuration of one or more containers:

```console
docker update [container]
```

View port mapping for a container:

```console
docker port [container]
```

Show running processes in a container:

```console
docker top [container]
```

View live resource usage statistics for a container:

```console
docker stats [container]
```

Show changes to files or directories on the filesystem:

```console
docker diff [container]
```

Copy a local file to a directory in a container:

```console
docker cp [file-path] CONTAINER:[path]
```

### Running a Container

The following commands show you how to start and stop processes in a container and how to manage container execution.
![Image title](https://phoenixnap.com/kb/wp-content/uploads/2022/12/running-a-container-cheat-sheet.png)

Run a command in a container based on an image:

```console
docker run [image] [command]
```

Create, start, and provide a custom name for the container:

```console
docker run --name [container-name] [image]
```

Establish a connection with a container by mapping a host port to a container port:

```console
docker run -p [host-port]:[container-port] [image]
```

Run a container and remove it after it stops:

```console
docker run --rm [image]
```

Run a detached (background) container:

```console
docker run -d [image]
```

Start an interactive process, such as a shell, in a container:

```console
docker run -it [image]
```

Start a container:

```console
docker start [container]
```

Stop a running container:

```console
docker stop [container]
```

Stop a running container and start it up again:

```console
docker restart [container]
```

Pause processes in a running container:

```console
docker pause [container]
```

Resume processes in a running container:

```console
docker unpause [container]
```

Block a container until others stop (after which it prints their exit codes):

```console
docker wait [container]
```

Kill a container by sending a SIGKILL to a running container:

```console
docker kill [container]
```

Attach local standard input, output, and error streams to a running container:

```console
docker attach [container]
```

Run a shell inside a running container:

```console
docker exec -it [container] [shell]
```

### Docker Image Commands

Below you will find all the necessary commands for working with Docker images.
![Image title](https://phoenixnap.com/kb/wp-content/uploads/2022/12/image-management-cheat-sheet.png)

Create an image from a Dockerfile:

```console
docker build [dockerfile-path]
```

Build an image from a Dockerfile located in the current directory:

```console
docker build .
```

Create an image from a Dockerfile and tag it.

```console
docker build -t [name]:[tag] [dockerfile-path]
```

Specify a file to build from:

```console
docker build -f [file-path]
```

Pull an image from a registry:

```console
docker pull [image]
```

Push an image to a registry:

```console
docker push [image]
```

Create an image from a tarball:

```console
docker import [url/file]
```

Create an image from a container:

```console
docker commit [container] [new-image]
```

Tag an image:

```console
docker tag [image] [image]:[tag]
```

Show all locally stored top-level images:

```console
docker images
```

Show history for an image:

```console
docker history [image]
```

Remove an image:

```console
docker rmi [image]
```

Load an image from a tar archive or stdin:

```console
docker load --image [tar-file]
```

Save an image to a tar archive file:

```console
docker save [image] > [tar-file]
```

Remove unused images:

```console
docker image prune
```

### Docker Networking Commands

One of the most valuable features of Docker software is the ability to connect containers to each other and to other
non-Docker workloads. This section covers network-related commands.
![Image title](https://phoenixnap.com/kb/wp-content/uploads/2022/12/networking-cheat-sheet-docker-pnap.png)

List available networks:

```console
docker network ls
```

Remove one or more networks:

```console
docker network rm [network]
```

Show information on one or more networks:

```console
docker network inspect [network]
```

Connect a container to a network:

```console
docker network connect [network] [container]
```

Disconnect a container from a network:

```console
docker network disconnect [network] [container]
```

### Docker General Management Commands

Once you set up your containers, you will need to know how to get all the important information for managing them. The
following commands will allow you to obtain general information about the system and connect to remote Docker resources.
![Image title](https://phoenixnap.com/kb/wp-content/uploads/2022/12/docker-general-management-cheat-sheet.png)

Log in to a Docker registry:

```console
docker login
```

Log out of a Docker registry:

```console
docker logout
```

List low-level information on Docker objects:

```console
docker inspect [object]
```

Show the version of the local Docker installation:

```console
docker version
```

Display information about the system:

```console
docker info
```

Remove unused images, containers, and networks:

```console
docker system prune
```

### Plugin Management Commands

Docker plugins extend Docker's functionality and help users connect with other popular services. The commands below let
you add, manage, and remove plugins on your system.
![Image title](https://phoenixnap.com/kb/wp-content/uploads/2022/12/docker-plugin-management-cheat-sheet.png)

Enable a Docker plugin:

```console
docker plugin enable [plugin]
```

Disable a Docker plugin:

```console
docker plugin disable [plugin]
```

Create a plugin from config.json and rootfs:

```console
docker plugin create [plugin] [path-to-data]
```

View details about a plugin:

```console
docker plugin inspect [plugin]
```

Remove a plugin:

```console
docker plugin rm [plugin]
```



