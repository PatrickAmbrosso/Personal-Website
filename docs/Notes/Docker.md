## Introduction to Docker
Containerization technology has been around for longer before Docker popularized it. But it was Docker which made the containerization technology accessible to the common people.

### Docker - The Technology vs Docker - The Company
The term Docker could refer to two things,

1. Docker, Inc. - The company behind developing the product.
2. [Docker](https://www.docker.com/) - One of the the products the company offers.

#### A brief history about Docker, Inc.
Docker, Inc is an American technology company that develops tools around the containers and technologies that enable it. The company was founded in 2008 with the name **dotCloud** by Kamel Founadi, Solomon Hykes and Sebastian Pahl in Paris and later moved to the US in 2010. The company was renamed to Docker in 2013. 

Check out more about the company through [Docker, Inc. website](https://www.docker.com/company/). The above paragraph is condensed from Wikipedia, [check here for the Wikipedia article](https://en.wikipedia.org/wiki/Docker,_Inc.)

Some of the popular products of Docker, Inc. include,
- [Docker Hub](https://hub.docker.com/) - A central repository of containers.
- [Docker Desktop](https://www.docker.com/products/docker-desktop/) - Desktop GUI for Windows and Mac platforms.

Docker also offers several pricing tiers with varied feature set. Check out [Docker's pricing page here](https://www.docker.com/pricing/).

#### A brief history about Docker 



### From Linux to everywhere
Docker traditionally was developed to work on the Linux OS, but slowly with Microsoft's contributions and close support with Docker, Inc. the platform is also available for Windows. This gave birth to 2 types of containers namely,

1. Linux Containers - Runs on Windows, Mac and Linux
2. Windows Containers - Runs on Windows

=== "Linux Containers"

    ```bash
    docker pull ubuntu:latest
    ```

=== "Windows Containers"

    ```PowerShell
    docker pull mcr.microsoft.com/powershell:lts-nanoserver-1903
    ```

As containers share the kernel of the host operating system, Windows containers cannot run on a Mac or Linux. However, thanks to Microsoft's support for Linux, Windows containers as well as Linux containers can run on Windows.

---
## Docker Installation
[Refer the Docs](https://docs.docker.com/get-docker/) for the latest information on instructions for how to install docker for the platform of your choice. Docker is available (thanks to the contribution of countless open source contributions) on Linux, Windows and Mac. However, the way the containerization is implemented varies slightly across the platforms.

---
## Docker Architecture
Docker follows a simple client-server architecture along with a central repository to store and serve the images. Thus, there are 3 main components in a docker implementation.

1. **Client**
	- It connects to the docker engine to *send and receive commands and outputs*.
	- It could be a *GUI Application* such as Docker Desktop (available for Windows and Mac) or a *CLI Tool* available for Docker CLI (Windows, Mac and Linux).
	- Client can exist on the same machine as the Docker Engine or exist on a different machine.
2. **Server**
	- This is
	- It performs all operations related to containers throughout their lifetime.
	- Often referred to as `dockerd` (pronounced as `docker-dee`).
	- It manages several container objects such as images, containers, volumes, networks and other plugins.
3. **Repository**
	-  A storage location for container images.
	- It could be the official repository from Docker Inc., [Docker Hub](https://hub.docker.com/) or from a third-party provider such as AWS, Azure or GCP or locally maintained by a company.

Communication between docker client and docker engine happens over *REST API*. Docker engine runs on *port 2376* by default.

### Client
- The docker client provides a primary way for the users to interact with docker.
- It provides an interface to manage container objects such as images, containers, volumes, networks and other plugins.
- Docker client is available as

	1. **Docker CLI** - Available in Windows, Mac and Linux.
	2. **Docker Desktop** - Available on Windows and Mac.

### Engine
The docker engine can be considered as the software that runs, manages and terminates the containers as per the user's requirements. Docker engine (currently-2023) is modular in nature and is made up of smaller specialized tools that adhere to open standards such as the Open Container Initiative or OCI.

#### Monolithic Docker Engine from the past
When docker was initially released, the docker engine had two components.

1. **Docker Daemon or `dockerd`** - a singular daemon that contained the code for client, API, container runtime and more.
2. **LXC** (Linux Containers)- An OS-level virtualization technology that allows creation and management of multiple isolated Linux virtual environments on a single control host

The docker daemon was built as a monolith that performs several functionalities and LXC was specific to Linux, thus threatening the cross-platform ambition of docker.

LSX was soon replaced with `libcontainer` developed in-house by Docker. Inc. to provide a cross-platform functionality to docker. `libcontainer ` replaced LXC as the execution driver in docker 0.9.

The next priority was to breakdown the monolithic docker daemon into microservices. This gave rise to tools such as `runc`, `containerd` and `shim` discussed below.

#### Open Container Initiative (OCI)
The [Open Container Initiative](https://opencontainers.org/) or the OCI is an open governance standard/structure for the purpose of creating industry standards around container formats and runtimes. OCI was established in 2015 by Docker and other industry leaders in the container industry.

OCI currently contains 3 specifications

1. **Runtime Specification (`runtime-spec`)**
2. **Image specification (`image-spec`)**
3. **Distribution Specification (`distribution-spec`)**

#### Docker in its present state
Currently (2023), Docker engine is made up of the following components.

1. **docker daemon or `dockerd`**
	- The docker daemon or `dockerd` provides an interface between the docker client and the core docker functionalities.
	- The client communicates to `dockerd` via REST API and `dockerd` delegates the tasks and performs the actions as directed from the client.
	- From being a monolithic implementation to a microservices focussed tool, most of the `dockerd`'s initial functionality is stripped into their own services.
	- Currently, the daemon is responsible for image management, image builds, REST API communication, authentication, security, core network
2. **`containerd`**
	- Its main functionality is to manage container lifecycle.
	- It interfaces between the daemon and `runc`
	- Initially it set out to be a lightweight container management tool, it soon took additional functionality of managing other objects such as images, volumes and networks.
	- However, as `containerd` is very modular, most of the functionality can be added only if required, thus keeping the size small.
	- [`containerd`](https://containerd.io/) is currently a graduated CNCF project.
3. **`shim`**  
	- `shim` promotes the concept of daemon-less containers.
	- It is used to decouple the container runtimes from the `containerd` service.
	- This is advantageous as the now-decoupled environment is not dependent on `containerd` for its operation, thus allowing `containerd` and thereby the daemon to be started, stopped or upgraded without killing all the running containers.
	- Every time a new container needs to be started, a new fork of the `runc` is created.
	- `runc` then starts the container and then exists.
	- `shim` then takes over the process and manages the containers by keeping the STDIN and STDOUT streams open and reporting container's exit status back to the daemon.
4. **`runc`**
	- It is the reference implementation of the OCI `runtime-spec`. 
	- `runc` is referred to operating at the OCI layer.
	- It is a lightweight CLI wrapper for `libcontainer`  interfacing between the host kernel and the container.
	- At its core, the functionality of `runc` is to start containers and it is very efficient at doing that.
	- `runc` can be replaced with any other OCI `runtime-spec` compatible runtimes.

### Image Repository/Registry
[Docker Hub](https://hub.docker.com/) is the most popular image repository/registry to upload and manage docker images (OCI compatible images). It offers features such as image versions (tags), public and paid repositories and much more. There are several [pricing options](https://www.docker.com/pricing/) that offer varied support and feature set.

There are other image repositories as well. Some of the other popular ones include

- [Amazon Elastic Container Registry](https://aws.amazon.com/ecr/) by AWS.
- [Azure Container Registry](https://azure.microsoft.com/en-in/products/container-registry/) by Azure.
- [Container Registry](https://cloud.google.com/container-registry/) by Google Cloud (GCP)

---
## Communication via REST API
Communication between the client, docker engine and the image repository occurs via REST API. This is implemented by the docker daemon.

Docker can be set up in two ways

1. Docker Client and Docker Engine on the same host - Communication over the IPC socket.
2. Docker Client and Docker Engine on different hosts - communication over the network

#### Client and Engine on Same Host
In a default installation, the docker client and the docker engine are setup in the same host. In such cases the communication is made via the local IPC socket.

!!! example IPC Socket

    Unix Domain Socket (UDS) or Inter-Process Communication (IPC) Socket is a data communications endpoint for exchanging data between processes running on the same host operating system. It is a standard component of the POSIX operating systems.

    In an IPC Socket communication, all communication occurs entirely within the operating system kernel. 


The socket can be found at `/var/run/docker.sock` on Linux and at `//./pipedocker_engine` on Windows. 

#### Client and Engine on Different Host


---
## Docker Objects
Docker comprises of several entities referred commonly as objects. These are

1. **[[Docker#Images|Images]]** - Templates that are used when containers are created and used.
2. **[[Docker#Containers|Containers]]** - Running/operational instance of an image.
3. **[[Docker#Volumes|Volumes]]** - Persistent storage for data created and stored during container runtimes.
4. **[[Docker#Networks|Networks]]** - Constructs of networking that enable containers to communicate within themselves and outside of the docker environment to the docker host and to other external networks.
5. **[[Docker#Networks|Networks]]** - Optional functionalities across domains such as volume, networking, authentication and so on that can be added to the docker engine.

### Images
- Images are one of the core parts of a docker implementation.
- They are templates from which containers are created.
- Hence images are considered as build-time constructs.
- Once a container is created from an image, they both become dependent on each other, thus the *image cannot be destroyed unless all of the containers that use the image are stopped and removed*. 
- Images are usually small in size as they contain the barebones of what is required to run the app or service or environment and nothing else. Containers use the host's operating system for lower level tasks, hence the light weightiness of the images.
- However, Microsoft's images on windows are a little bit larger in size due to the architecture of the Windows Operating System.

#### Pulling Images
The process of downloading an image from a repository is referred to as **pulling**. Docker defaults to Docker Hub as source repository, however it is possible to change the repository when pulling the images and the process remains the same nonetheless. 

```bash title="docker pull command"
docker pull ubuntu:latest
```

If the image did not exist prior locally, it is pulled as given below.

```txt title="docker pull output - new pull"
latest: Pulling from library/ubuntu
677076032cca: Pull complete
Digest: sha256:9a0bdde4188b896a372804be2384015e90e3f84906b750c1a53539b585fbbe7f
Status: Downloaded newer image for ubuntu:latest
docker.io/library/ubuntu:latest
```

However, if the image already exists locally, the following information is given by the docker engine back to the client.

```txt title="docker pull output - existing image"
latest: Pulling from library/ubuntu
Digest: sha256:9a0bdde4188b896a372804be2384015e90e3f84906b750c1a53539b585fbbe7f
Status: Image is up to date for ubuntu:latest
docker.io/library/ubuntu:latest
```

What happens if an image that does not exist is requested for a pull?

```bash
docker pull totally-fake-image
```

The action returns the following response.

```txt
Using default tag: latest
Error response from daemon: pull access denied for totally-fake-image, repository does not exist or may require 'docker login': denied: requested access to the resource is denied
```

To pull all images in a repository (usually not recommended) pass the `-a` flag to the docker pull command.

```bash
docker pull -a image
```

#### Listing the locally stored Images
To list the images that are currently available locally, the following commands can be used.

```bash
docker images
docker image ls
```

Docker then displays all the images that are available locally.

```txt
REPOSITORY   TAG       IMAGE ID       CREATED       SIZE
postgres     latest    680aba37fd0f   2 weeks ago   379MB
node         latest    272b8142e84e   2 weeks ago   998MB
mysql        latest    57da161f45ac   2 weeks ago   517MB
ubuntu       latest    58db3edaf2be   4 weeks ago   77.8MB
```

The `docker image ls` command allows to filter the output of the docker images returned based on certain criteria such as

- **dangling (Accepts Boolean)** - Untagged images which usually occur when newer images are built with the tag that exists prior. Docker removes the tag from the older version of the image and attaches the tag to the newer image, thus leaving the older image without tags.
- **before (Accepts image name/ID)** - Returns all the images created before the supplied image.
- **since (Accepts image name/ID)** - Returns all the images created after the supplied image.
- **label (Accepts string)** - Returns the images based on the presence of a label or value. However, the docker images ls command does not display labels in its output.

Docker also supports filtering using reference (additional functioning features) and the usage Go templates using the `--format` flag.

```bash
# List images that are devoid of tags
docker image ls --filter dangling=true

# List images that have tags
docker image ls --filter dangling=false

# List images before the curious-salamander 
docker image ls --filter before="curious-salamander"

# List images after the curious-salamander
docker image ls --filter since="curious-salamander"

# List images with Go Templates formatting
docker image ls --format "{{.Repository}}: {{.Tag}} - {{.Size}}"
```

#### Image Naming
Each and every image is labelled in a container registry to make it easier to identify the image. A standard image nomenclature is given below.

```txt
registry-link/repository-link/image-name:tag
```

- `registry-link`
	- DNS name of the image registry.
	- Instructs docker client of which registry to use.
	- This is optional, will resolve to Docker Hub registry if no input is provided.
- `repository-link`
	- Name of the repository that hosts the image of interest.
	- Official images are stored by Docker Hub in the top-level of the docker Hub namespace.
	- This is not used if the image under question resides on the top level domain in Docker Hub.
	- For third-party images, repository link is mandatory.
- `image-name`
	- Denotes the actual name of the image of interest.
	- This is a mandatory field.
- `tag`
	- Denotes a variant of the image within the repository.
	- it is just a string used for easy identification.
	- Usually, repositories would have an incrementing numeric progression.
	- The latest version, by convention is also tagged as latest to facilitate easy identification.
	- However, it might not be the case always and it entirely depends on the developer and maintainer of the image
	- The same image can have more than one tags as repositories do not restrict with content.
	- It is always best practice to refer the documentation of the respective images to better understand the naming convention of the tags used foe the particular image.

#### Searching the repository
To search for a particular image, the `docker search` command can be used. However it only searches the NAME field. By default, Docker displays only 25 hits, which can be increased to 100 with the limit flag.

```bash
docker search alpine --limit 5
```

This returns all the images with the text alpine in the NAME field.

```txt
NAME                               DESCRIPTION                STARS     OFFICIAL   AUTOMATED
alpine                             A minimal Docker image …   9724      [OK]
alpinelinux/docker-cli             Simple and lightweight …   7
alpinelinux/gitlab-runner          Alpine Linux gitlab-run…   4
alpinelinux/alpine-gitlab-ci       Build Alpine Linux pack…   3
alpinelinux/gitlab-runner-helper   Helper image container …   2
```

#### Made up of Layers
An image in Docker is made up of several layers of files that make up the final image. This is done to make the images modular. Docker finally stacks the layers and displays them as a single image. This is done to make docker efficient in operation and avoid constant re-download of existing layers from other images.

Suppose, if an ubuntu image has to be downloaded for the latest version, it might share the underlying layers from the previous versions of ubuntu. In that case, only the new layers are downloaded and the existing ones are used as such. 

Layers are represented using their SHA256 hashes. This is used by docker to identify and use the layers across images without downloading the same layer again for another image.

The number of layers is based upon what the image does, the core applications it runs and that base image was used to build the image.

These layers can be seen when trying to pull and image. To know the different layers that make up an image, use the `docker image inspect` command.

```bash
docker image inspect <container-name/container-ID>:<tag>
```

Behind the scenes, docker employs a storage driver that enables the layer stacking and presenting them as a single file system. However, the user experience remains identical in each case.

Inspecting images also provide a way to under stand the default process that will run upon the container start from the image. This is usually found within the CMD or the ENTRYPOINT sections of the `json` object the inspect command returns.

#### Content Hashes, Digests and Immutability
Docker 1.10 introduced the concept of content hashes, where each and every image gets a *cryptographic content hash* which is a representation of  the contents of an image. This makes it impossible to change the contents of an image 
without changing the hash. This is referred to as **digest**, which are a way to uniquely identify an image.

When an image is pulled, the docker client shows the digest associated with the image. To show all the images along with their digest, use the --digest flag to `docker images` command

```bash
docker images --digest
```

```txt
REPOSITORY   TAG       DIGEST               IMAGE ID       CREATED       SIZE
postgres     latest    sha256:901df89014…   680aba37fd0f   2 weeks ago   379MB
node         latest    sha256:33e99abf6c…   272b8142e84e   2 weeks ago   998MB
mysql        latest    sha256:8653a170e0…   57da161f45ac   2 weeks ago   517MB
ubuntu       latest    sha256:9a0bdde418…   58db3edaf2be   4 weeks ago   77.8MB

# NOTE: The digest hash is truncated for better visibility
```

Currently, there is no provision to get the digest has for images in a remote registry. To get the digest of an image, the image needs to be pulled locally.

#### Deleting Images
Deleting images can be done by using the docker `images rm` or the `docker rmi` command. Deleting an image remove the layers corresponding to the image from the host. However, if any of the layers is shared across multiple other images, the layer is not removed until all other images are also deleted.

```bash
# Delete images (command 1)
docker image rm <image-name/image-ID>

# Delete images (command 2)
docker rmi <image-name/image-ID>

# Deleting multiple images
docker rmi <image-1-name/image-1-ID> <image-2-name/image-2-ID>

# Deleting all existing images
docker rmi $(docker images -q)
```

### Containers
Docker implements containers adhering to the OCI runtime, image and specifications, thus making them compatible to be run on other containerization platforms also following the OCI standards. A container is the runtime instance of an image. It can be considered similar to a VM instance, but it is much faster and lightweight than a typical VM. Also Containers share the OS/Kernel of the host and install only the libraries and dependencies required by the application or service that the container hosts. 

#### Run Containers (basics)
Containers can be run from an image which can be either locally available or hosted in an image registry. If locally available, containers are spun up from the local copy of the image. If the images are not available locally, they are pulled from the remote registry and then the containers are spun up. Thus, containers can only be spun from a local copy of images, which makes them dependent during runtime.

```bash
docker run -it centos:latest /bin/bash
```

The following outputs results in case the image does not exist locally.

```txt
Unable to find image 'centos:latest' locally
latest: Pulling from library/centos
a1d0c7532777: Pull complete
Digest: sha256:a27fd8080b517143cbbbab9dfb7c8571c40d67d534bbdee55bd6c473f432b177
Status: Downloaded newer image for centos:latest
[root@adcd9bc897de /]#
```

In case a copy of the image is found locally, the following output can be obtained where the container is started straightaway.

```txt
[root@e5da8ec268a1 /]#
```

#### Listing Containers
The following command(s) can be used to list the containers currently managed by the docker engine.

```bash
# List running containers (command 1)
docker container ls

# List running containers (command 2)
docker ps

# List all containers (stopped and running)
docker ps -a
```

Upon executing the commands given above, the following output can be obtained.

```txt
# Listing just the running containers
CONTAINER ID   IMAGE           COMMAND        CREATED         STATUS         PORTS     NAMES
b83c2173f913   centos:latest   "sleep 1000"   3 seconds ago   Up 2 seconds             wonderful_wilson

# Listing all the containers (stopped and running)
CONTAINER ID   IMAGE           COMMAND        CREATED              STATUS                          PORTS     NAMES
b83c2173f913   centos:latest   "sleep 1000"   About a minute ago   Up About a minute                         wonderful_wilson
464420bfbaba   centos:latest   "/bin/bash"    About a minute ago   Exited (0) About a minute ago             gifted_robinson
e5da8ec268a1   centos:latest   "/bin/bash"    10 minutes ago       Exited (0) 2 minutes ago                  objective_banach
adcd9bc897de   centos:latest   "/bin/bash"    11 minutes ago       Exited (0) 10 minutes ago                 gracious_borg

```

#### Stopping Running Containers
To stop running containers, the following command(s) can be used.

```
# Stop a single Container (command 1)
docker container stop curious_mongoose

# Stop a single Container (command 2)
docker stop curious_mongoose

# Stop a multiple Containers
docker stop angry_shark 1b24b6409429

# NOTE: Both container Name and Container ID can be used interchangeably.
# NOTE: First few characters (uniqueness required) are enough when using ID
```

Docker engine then returns the name/Id that was supplied to denote that the containers have been stopped.

```txt
# Stop a single Container
curious_mongoose

# Stop a multiple Containers
angry_shark
1b24b6409429
```

An important note to take is that a container runs *as long as a main task is designated* to it. Without a process, the container terminates. Thus, here are a couple of things to keep in mind.

1. Starting a container without a main process, terminates it immediately.
2. Killing the main process of a running container, terminates the container.

To exit a container without killing the container, use the shortcut `Ctrl + P + Q`

#### Start a Stopped Container
To start a stopped container, use the following command

```bash
# Start a stopped container (command 1)
docker container start curious_mongoose

# Start a stopped container (command 2)
docker start curious_mongoose

# NOTE: Both container Name and Container ID can be used interchangeably.
```

A couple of important things to keep in mind when stopping and starting containers.

1. Stopping and Starting a container does not destroy the data within it.
2. However, if a container is terminated, the data is lost.

Containers can be configured to automatically restart based on a restart policy. These restart policies are *container scoped*. Following are the restart policies currently available

- `always` - Always restart containers
- `unless-stopped` - Restart unless explicitly stopped
- `on-failed` - Restart if the container fails and terminates

#### Removing/Deleting Containers
To remove a container completely. the `docker container rm` command can be used.

```
# Remove (delete) a container (Command 1)
docker container rm happy_otter

# Remove (delete) a container (Command 2)
docker rm sad_kangaroo

# Remove (delete) all existing containers (forcefully)
docker rm $(docker ps -aq) -f

# NOTE: Both container Name and Container ID can be used interchangeably.
```

The `docker rm` command can be used to stop and terminate (delete/remove) a container in one go. However, it abruptly terminates the container. The differences between using `docker stop` and `docker rm` to stop a container is the signal it sends to the main process running inside the container.

1. `docker stop` issues the SIGTERM command and it provides the containers to complete the existing task and terminate gracefully. If the container does not stop, a SIGKIILL command is issued.
2. `docker rm` issues the SIGKILL command straightaway, thus abruptly terminating the container.

#### Accessing the Container Logs
Logs can be an effective way to identify and troubleshoot issues. To access the logs of a container, use the following command.

```bash
docker logs keen_meninsky
```

This lists all the logs of the container named `keen_meninsky`

```txt
root@f1b513b37415:/# whoami
root
root@f1b513b37415:/# man grep
This system has been minimized by removing packages and content that are
not required on a system that users do not log into.

To restore this content, including manpages, you can run the 'unminimize'
command. You will still need to ensure the 'man-db' package is installed.
```

#### Running Containers (more options)
Containers can be given additional instructions when they are started. Following are some of the most common information that can be passed when starting containers

```bash
# Run container in detached mode (leaves current terminal free to use)
docker run -d ubuntu:latest

# Attach tthe STDIN, STDERR and STDOUT of a container to terminal
docker -a proud_jackfruit

# Run container in interactive mode (i) and leave the terminal open (t)
docker run -it ubuntu:latest

# Execute a command on a container upon start
docker run ubuntu:latest sleep 100

# Explicitly name a container (replaces the randomly generated name)
docker run --name my-demo-environment ubuntu:latest

# Map a port from host to container (Format - Host:Container)
docker run -p 8080:80 httpd:2.4.55

# Pass values to environment variables (can be checked with inspect command)
docker run -e MYSQL_ROOT_PASSWORD=my-secret-pw mysql:latest
```

Check out the list of options that can be supplied from [Docker Docs](https://docs.docker.com/engine/reference/commandline/run/#options) 

### Volumes
By default, all data stored within a container's runtime storage is deleted when the container is terminated. Volumes are a way to manage that behavior to allow to create non-persistent and persistent storage for data as required. 

1. **Non-Persistent Data**
	- Default storage mode for containers
	- The data is wiped as soon as the container is terminated.
	- Thus, the data lifecycle is coupled with the container lifecycle.
2. **Persistent Data**
	- Data persists beyond the lifecycle of the container.
	- Volumes provide a way to persist data.

#### Non-Persistent Data
Containers are considered to be immutable, meaning that they are read-only entities. However, a read-write layer is essential to run certain processes on containers. This is provided by means of storage drivers. Following are the storage drivers currently available.

- Storage Drivers available on Linux
	- `AUFS`
	- `overlay2`
	- `devicemapper`
	- `btrfs`
	- `zfs`
- Storage Drivers on Windows
	- `windowsfilter`

#### Persistent Data
Volumes are the most common way to maintain persistent data within containers. Following are three points to keep in mind as to why volumes are preferred to run persistent storage in containers.
- **Volumes are independent** - volumes are not tied to the lifecycle of the container, hence the container can be stopped, removed and restarted, but the data would persist.
- **Volumes can be mapped to external storage** - Volumes can be mapped to external storage services both on-premises and on cloud, thus allowing them to share storage with other machines.
- **Volumes can be mounted to multiple containers** - Volumes provide features to attach them to multiple containers at the same time.

!!! note Docker Volumes as First-Class Citizens
    
    Volumes in Docker are first-class citizens and have their own object in the API. Thus some sub-commands might mean different thing entirely in volumes as to what it would mean in the context of an image or a container.

##### Create a Volume
To create a new volume, use the following command

```bash
# Create Docker Volumes
docker volume create my-first-volume
```

This by default creates a docker volume with the `local driver`. Several other general and purpose built drivers are available as third party plugins providing Docker and thereby the containers running on Docker, seamless access to data across a wide variety of sources, formats and file systems.

##### Listing all Volumes
To list all the volumes available locally, use the following command

```bash
# List Docker Volumes
docker volume ls
```

Docker Engine responds with the list of all volumes locally available.

```txt
DRIVER    VOLUME NAME
local     6d406a6bfc9f2a4fd7c80f8a9f908437374ec861d2c4ca76519af2699bdb4c7f
local     3877ee1f1a0d095e20b3a67b11e33259c8adb077947c10e61a56ab983f6206db
local     a573c7afa8ec51e9970a75a49017de551790906b60512c83833c57728d999b0e
```

This shows the list of volumes with the `DRIVER` being used along with the `VOLUME NAME`. Unless specifically created, it shows the hash for the volume.

##### Inspect Volumes
To know more information about a volume in question, the `inspect` command can be used as below

```bash
# Inspect a Docker Volume
docker volume inspect my-first-volume
```

This produces an output with details about the volume.

```txt
[
    {
        "CreatedAt": "2023-02-28T01:12:49Z",
        "Driver": "local",
        "Labels": {},
        "Mountpoint": "/var/lib/docker/volumes/my-first-volume/_data",
        "Name": "my-first-volume",
        "Options": {},
        "Scope": "local"
    }
]
```

##### Attach Volumes


##### Delete Volumes
Docker Volumes can be deleted with the following commands

1. `docker volume rm` - (*SINGLE ACTION*) Deletes specific volumes supplied by their volume names.
2. `docker volume prune` - (*MASS ACTION*) Deletes all volumes that are currently not attached to a container or service.

```bash
# Delete specific volumes
docker volume rm my-first-volume

# Delete all non-attached volumes
docker volume prune
```

Docker shows the following output

```txt
# docker volume rm - for unattached volumes
my-first-volume

# docker volume rm - or attached volumes
Error response from daemon: remove important-volume: 
volume is in use - [1b77d...9c7]

# docker volume prune
WARNING! This will remove all local volumes not used by at least one container.
Are you sure you want to continue? [y/N] y
Deleted Volumes:
a573c7afa8ec51e9970a75a49017de551790906b60512c83833c57728d999b0e
3877ee1f1a0d095e20b3a67b11e33259c8adb077947c10e61a56ab983f6206db
6d406a6bfc9f2a4fd7c80f8a9f908437374ec861d2c4ca76519af2699bdb4c7f

Total reclaimed space: 1.71GB
```

##### Mount Volumes to Containers


##### Using Volume Plugins
Volume Plugins are a way to extend the persistent data functionality across multiple cloud platforms and service providers. Docker Hub allows third party volume plugins to be hosted. Installing plugins can be done with the following command

```bash
docker plugin install purestorage/docker-plugin:latest --alias pure --grant-all-permissions
```

!!! danger Data Corruptions in Shared Volumes

    When writing data to a shared volume, there might be instances where the container holds the data in local buffer before committing to the shared volume. If multiple containers perform this delayed update, it is possible that the container that updates the data at the last might overwrite the existing data.

    To avoid this, the application or service that might access the shared volume must be developed in a way to circumvent the overwrite issue. 


### Networks


#### Architecture and Implementation
At its highest level, networking in docker is comprised of 3 major components.

1. **Container Network Model (CNM)** - Provides the Specification of network implementation.
2. **Libnetwork** - Implementation of CNM for Docker.
3. **Drivers** - Offers support for network topologies.

##### Container Network Model (CNM)
Container Network Model provides the design fundamentals for networking in docker. CNM can be divided into 3 main building blocks.

1. **Sandboxes**
	- Isolated network stacks, containing ethernet interfaces, ports, routing tables and DNS config.
2. **Endpoints**
	- Virtual network interfaces.
	- Responsible for making connections. 
3. **Networks**
	- These are software implementations of a network switch acting as a 802.1d bridge.
	- They group together and isolate a collection of endpoints that need to communicate.

A connection between two containers needs to be explicitly established by means of networks connected via endpoints.

##### Libnetwork
While CNM provided the basis and guidelines for the architecture, Libnetwork is the actual implementation of the CNM for Docker. Libnetwork is written in Golang and is cross-platform.

Previously the docker daemon used to perform all the networking and routing. But moving towards a cloud native, microservices based architecture favoring the Unix ideology of modular tools that perform specific function well, the networking part of the daemon was refactored into Libnetwork.

Libnetwork also implements *native service discovery*, *ingress-based container load balancing* and the *network control plane & management plane functionality*.

##### Network Drivers
While Libnetwork implements the network control plane and the management plane, the network drivers implement the data plane. Here, connectivity, isolation and creation of networks is handled by the drivers. 

By default, docker ships with several network drivers (commonly referred to as local drivers or native drivers). In Linux, these include `bridge`, `overlay`, `macvlan`. On Windows, these could be `nat`, `overlay`, `transparent` and `l2bridge`. There are several other third party network drivers available on Docker Hub for varied use cases.

Each of these drivers are responsible for the actual creation and management of resources on the network they manage. 

At any given time, there can be multiple network drivers active, promoting a fluid environment and therefore Docker supports a wide range of heterogeneous networks. 

#### Working with Networks
:::note Docker Network as First-Class Citizens
Networks in Docker are first-class citizens and have their own object in the API. Thus some sub-commands might mean different thing entirely in networks as to what it would mean in the context of an image or a container.
:::

##### Create Networks
To create a network in Docker, use the following command

```bash
# Create networks in docker (by specifying driver)
docker network create -d bridge MyLocalNetwork
```

Upon successful creation, the Docker engine shows the hash of the thus created network.

```txt
a98dddd335c41d115e89a3fb7113f8360a1b2d8cc3d8346a05c960c6fff42959
```

##### Listing All Networks
To list all the active networks maintained by the Libnetwork associated with the local Docker engine, use the following command

```bash
# List available Docker Networks
docker network ls
```

This returns a similar list as follows based on what networks are locally configured and maintained.

```txt
NETWORK ID     NAME             DRIVER    SCOPE
a98dddd335c4   MyLocalNetwork   bridge    local
afe897982312   bridge           bridge    local
7280ae463ec3   host             host      local
```

##### Inspect Networks
One of the most powerful tools when working with networks in docker, the inspect command provides valuable insights.

```bash
# Inspect a Docker Volume
docker network inspect MyLocalNetwork
```

This produces an output with details about the volume.

```txt
[
    {
        "Name": "MyLocalNetwork",
        "Id": "ead060ef929e747f2be0bdedb7182a4047c3a8dbaeab63a6e103581dd4532abd",
        "Created": "2023-03-01T17:05:04.12407108Z",
        "Scope": "local",
        "Driver": "bridge",
        "EnableIPv6": false,
        "IPAM": {
            "Driver": "default",
            "Options": {},
            "Config": [
                {
                    "Subnet": "172.21.0.0/16",
                    "Gateway": "172.21.0.1"
                }
            ]
        },
        "Internal": false,
        "Attachable": false,
        "Ingress": false,
        "ConfigFrom": {
            "Network": ""
        },
        "ConfigOnly": false,
        "Containers": {},
        "Options": {},
        "Labels": {}
    }
]
```

##### Delete Networks
Docker networks can be deleted with the following commands
1. `docker network rm` - (*SINGLE ACTION*) Deletes specific networks supplied by their network Names or Network ID.
2. `docker network prune` - (*MASS ACTION*) Deletes all networks that are currently not attached to a container or service.

```bash
# Delete specific networks
docker network rm Secret-Salamander

# Delete all non-attached networks
docker network prune
```

Docker shows the following output

```txt
# docker network rm - for unattached volumes
my-first-volume

# docker network rm - or attached network
Error response from daemon: remove Secret-Salamander: 
Network is in use - [1b77d...9c7]

# docker network prune
WARNING! This will remove all custom networks not used by at least one container.
Are you sure you want to continue? [y/N] y
Deleted Networks:
minikube
mongo-network
MyLocalNetwork
```



### Plugins


---
## Build Images with Dockerfile
Containers are immutable infrastructure, meaning no changes are made extensively once the application is deployed. In order to run containers, images are required as templates. Images can be built as per requirement by using a *Dockerfile*.

A Dockerfile provides a way to create images. It guides the Docker engine on the requirements to build an image as per the specifications supplied. The directory in which the application and dependencies exist is referred to as the *build context*. Dockerfile usually exists at the build context directory. Dockerfile is always written as **Dockerfile** with the first letter capital and without spaces. Also, a Dockerfile does not have an extension (its just Dockerfile).

### Dockerfile Syntax Reference
The template for a typical Dockerfile follows the below given syntax.

```Dockerfile
# Comment
INSTRUCTION arguments
```

- Here, the `INSTRUCTION`'s are not case-sensitive but by convention are written in uppercase to identify instructions from arguments. 
- Dockerfile instructions are executed in order from top to bottom.

The following snippet captures some of the most commonly used `INSTRUCTION` types and an example of how to use them.

```Dockerfile
# DemoDirective=
# Dummy Dockerfile - Demonstration Purpose
FROM alpine                              


```

#### Structure and Formatting
- **Comments** *(Optional)*
	- Comments in a Dockerfile are marked with a `#` at the beginning.
	- However, parser directives can also start with a `#`
	- Unless the line is a parser directive, all comments are ignored by the Docker Daemon when processing the Dockerfile.
- **Parser Directives** *(Optional)*
	- Parser Directives affect the way the subsequent lines are handled in a Dockerfile.
	- They are structured as special kind of comment in the form of `# directive=value`. 
	- Parser Directives cannot be repeated and can appear only once.
- **Environment Variables** *(Optional)*
	- sdf
- **.dockerignore** *(Optional)*
	- It can be used to exclude files and directories from being sent to the Docker Daemon.
	- Docker CLI looks for a file named `.dockerignore` in the root of the context.
	- If found, the CLI modifies the contents of the entities to exclude the files and directories before it sends to the daemon.
	- It can be used to exclude files and folders being sent with the `ADD` and `COPY` instructions
	- A typical .`dockerignore` file is provided below for reference.

```txt
# Comment
/temp*
*/secrets*
*/*/logs*
temp?
```

| Syntax Reference | What they mean in a `.dockerignore` file                                                                         |
| ---------------- | ---------------------------------------------------------------------------------------------------------------- |
| `# comment`      | Ignored - no action taken                                                                                        |
| `*/temp*`        | Excludes files and subdirectories starting with `temp` from the root of the context.                             |
| `*/secrets`      | Excludes files and subdirectories one level down the root that start with `secrets`                              |
| `temp?`          | Excludes files and subdirectories that start with `temp` and have one more character at the root of the context. |


#### Dockerfile Instructions and Arguments
- **FROM** *(Mandatory)*
	- This instruction is mandatory and always must be the first instruction at the top of the Dockerfile. However there can be comments preceding it.
	- Specifies the base image from which the current image is to be built.
	- To build an image from scratch, use the official `scratch` image from Docker Hub. 
- **RUN** *(Optional)*

### Building the Images

### Pushing the Images to a Registry


---
## Deployment with Docker Compose


---
## Container Orchestration with Docker Swarm


---
## Networking


---
## Overlay Networking


---
## Persisting Data with Volumes


---
## App Deployment with Stacks


---
## Focussing on Security


---
[[Docker Cheat Sheet]]

## Building Images with Dockerfile
- **Pull a container image from a repository:** `docker pull image:tag`
- **Run a container:** `docker run image:tag`
- **Docker ps command:** `docker ps <flags>` --> `docker ps -a` (all containers, running or not)
- **Docker stop command:** `docker stop <container_name/container_id>`
- **Docker rm command:** `docker rm <container_name/container_id>`
- **List local images:** `docker images` or `docker images ls`
- **Execute a command on a container:** `docker exec <container_name/container_id> <command>`
- **Running in detached mode:** `docker run -d image:tag`
- **Attach a detached container:** `docker attach <container_name/container_id>`
