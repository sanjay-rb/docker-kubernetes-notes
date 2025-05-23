# Docker & Kubernetes Notes

This repository contains notes and commands related to Docker and Kubernetes.

> **Note**: This repository includes references to various Dockerfiles with names formatted like `Dockerfile [notes]`. When running these on your machine, be sure to remove the square brackets (`[]`) from the filename. Docker requires the file to be named exactly `Dockerfile`, without any extensions or additional characters.

## Manipulating Containers with Docker CLI

### `docker run hello-world`

Runs a test container to verify Docker installation.

```
Hello from Docker!
This message shows that your installation appears to be working correctly.
...
```

### `docker run busybox echo 'Hello World!'`

Runs BusyBox and echoes a message.

```
Hello World!
```

### `docker create hello-world`

Creates a container without starting it.

```
a1b2c3d4e5f678901234567890abcdef1234567890abcdef1234567890abcdef
```

### `docker start <container-id>`

Starts a container (no logs are shown).

```
a1b2c3d4e5f6
```

### `docker logs <container-id>`

Displays logs from a container.

```
Hello from Docker!
This message shows that your installation appears to be working correctly.
...
```

### `docker start -a <container-id>`

Starts a container and streams its logs.

```
Hello from Docker!
This message shows that your installation appears to be working correctly.
...
```

### `docker ps`

Lists currently running containers.

```
CONTAINER ID   IMAGE         COMMAND      CREATED          STATUS          PORTS     NAMES
a1b2c3d4e5f6   busybox       "echo test"  2 minutes ago    Up 2 minutes              busy_container
```

### `docker ps --all`

Lists all containers (including stopped).

```
CONTAINER ID   IMAGE         COMMAND      CREATED          STATUS                      NAMES
a1b2c3d4e5f6   hello-world   "/hello"     5 minutes ago    Exited (0) 4 minutes ago    awesome_hello
```

### `docker system prune`

Cleans up unused data.

```
WARNING! This will remove:
   - all stopped containers
   - all networks not used by at least one container
   - all dangling images
   - all build cache

Are you sure you want to continue? [y/N] y
Deleted Containers:
a1b2c3d4e5f6

Total reclaimed space: 25.3MB
```

### `docker stop <container-id>`

Gracefully stops a container.

```
a1b2c3d4e5f6
```

### `docker kill <container-id>`

Immediately kills a container.

```
a1b2c3d4e5f6
```

### `docker exec -it <container-id> <exec cmd>`

Executes a command in a running container.

```bash
bin
dev
etc
home
...
```

### `docker exec -it <container-id> sh`

Opens a shell inside the container.

```
#
```

### `docker run -it busybox sh`

Open a shell while running the new continer.

```
#
```

## Build Custom Images

1. **`docker build <working-dir-with-Dockerfile>`**

   _Create new continer with custom instructions._

   **Example Output (with Buildkit):**

   ```
   docker build .
   [+] Building 7.8s (7/7) FINISHED                                                                                                      docker:desktop-linux
   => [internal] load build definition from Dockerfile                                                                                                  0.0s
   => => transferring dockerfile: 277B                                                                                                                  0.0s
   => [internal] load metadata for docker.io/library/alpine:latest                                                                                      5.7s
   => [auth] library/alpine:pull token for registry-1.docker.io                                                                                         0.0s
   => [internal] load .dockerignore                                                                                                                     0.0s
   => => transferring context: 2B                                                                                                                       0.0s
   => [1/2] FROM docker.io/library/alpine:latest@sha256:a8560b36e8b8210634f77d9f7f9efd7ffa463e380b75e2e74aff4511df3ef88c                                0.8s
   => => resolve docker.io/library/alpine:latest@sha256:a8560b36e8b8210634f77d9f7f9efd7ffa463e380b75e2e74aff4511df3ef88c                                0.0s
   => => sha256:6e771e15690e2fabf2332d3a3b744495411d6e0b00b2aea64419b58b0066cf81 0B / 3.99MB                                                            2.0s
   => => extracting sha256:6e771e15690e2fabf2332d3a3b744495411d6e0b00b2aea64419b58b0066cf81                                                             0.1s
   => [2/2] RUN apk add --update redis                                                                                                                  1.0s
   => exporting to image                                                                                                                                0.2s
   => => exporting layers                                                                                                                               0.2s
   => => exporting manifest sha256:bbf11d4ef63d252300a29b6aedba0c8c90b3ebd63f38211ed9b5e68e6a68a860                                                     0.0s
   => => exporting config sha256:3c97933bd8c21954eedac04f31baeea99bc624f1f9bcd37489793204a3bde3c0                                                       0.0s
   => => exporting attestation manifest sha256:910ad445ce9b009cc847b2774d2e429e94f7eadb6f8d0271824340952c183eba                                         0.0s
   => => exporting manifest list sha256:c5f7ff8e437e2a4857661b11ac459769c86058822fd2230dd09f842c9719f8d4                                                0.0s
   => => naming to moby-dangling@sha256:c5f7ff8e437e2a4857661b11ac459769c86058822fd2230dd09f842c9719f8d4                                                0.0s
   => => unpacking to moby-dangling@sha256:c5f7ff8e437e2a4857661b11ac459769c86058822fd2230dd09f842c9719f8d4
   ```

   **Example Output (without Buildkit):**

   ```
   DOCKER_BUILDKIT=0 docker build .
   DEPRECATED: The legacy builder is deprecated and will be removed in a future release.
               BuildKit is currently disabled; enable it by removing the DOCKER_BUILDKIT=0
               environment-variable.

   Sending build context to Docker daemon   98.3kB
   Step 1/3 : FROM alpine
   latest: Pulling from library/alpine
   Digest: sha256:a8560b36e8b8210634f77d9f7f9efd7ffa463e380b75e2e74aff4511df3ef88c
   Status: Downloaded newer image for alpine:latest
   ---> a8560b36e8b8
   Step 2/3 : RUN apk add --update redis
   ---> Running in 7eb61f293f37
   fetch https://dl-cdn.alpinelinux.org/alpine/v3.21/main/aarch64/APKINDEX.tar.gz
   fetch https://dl-cdn.alpinelinux.org/alpine/v3.21/community/aarch64/APKINDEX.tar.gz
   (1/1) Installing redis (7.2.8-r0)
   Executing redis-7.2.8-r0.pre-install
   Executing redis-7.2.8-r0.post-install
   Executing busybox-1.37.0-r12.trigger
   OK: 11 MiB in 16 packages
   ---> Removed intermediate container 7eb61f293f37
   ---> ad87171750c2
   Step 3/3 : CMD ["redis-server"]
   ---> Running in fe284bc4dbca
   ---> Removed intermediate container fe284bc4dbca
   ---> 59801d109883
   Successfully built 59801d109883
   ```

### Introduction to Dockerfile

A Dockerfile is a script containing a series of instructions to build a Docker image. It automates the process of creating containerized environments, ensuring consistency and reproducibility.

### Basic Flow of a Dockerfile:

1. Specify the base image.
2. Run necessary boot scripts to the image.
3. Define the commands to run during the container's startup.

```
# 1. Specify the base image.
FROM alpine

# 2. Run necessary boot scripts to the image.
RUN apk add --update redis

# 3. Define the commands to run during the container's startup.
CMD ["redis-server"]
```

### Docker Build Flow

1. **FROM**
   1. Already downloaded ?
      1. Load from cache
   1. NO!
      1. Download base image
1. **RUN**
   1. Get previous image
   1. Is this instruction in cache from current image ?
      1. Load from cache
      1. Generate new image out of it.
   1. NO!
      1. Create temporary container
      1. Perform RUN instruction on that container
      1. Take a snapshot of temporary container
      1. Generate new image out of it
      1. Destroy temporary container
1. **CMD**
   1. Get previous image
   1. Create temporary container
   1. Perform CMD instruction on that container
   1. Take a snapshot of temporary container and generate new image out of it
   1. Destroy temporary container
1. if no more instruction
   1. Return newly generated latest image id

### What a Image can be created from Container !?

Yes, that's correct! You can create a new Docker image from any running container by specifying a start command (CMD).

To do this manually, use the following Docker CLI command:

```sh
docker commit -c 'CMD ["redis-server"]' <CONTAINER_ID>
```

### Can we name (or tag) Docker images?

Absolutely! You can assign a name (or tag) to your Docker images using the following methods:

1. **Tagging during build:**

   ```sh
   docker build -t sanjoke/redis:latest .
   ```

   - This command tags the image as you build it.
   - Here, `sanjoke` is your Docker Hub username, `redis` is the custom image name, and `latest` is the tag.

2. **Tagging an existing image:**
   ```sh
   docker tag 9dfadec01 sanjoke/redis:latest
   ```
   - This command tags an already built image (referenced by its image ID or SHA).
   - You can use this to add or change tags for images after they are built.

## Simple Project with Docker

### Create a Web Application

1. Build a simple web app using any language or framework you like.

### Write a Dockerfile

1. Choose a base image from [Docker Hub](https://hub.docker.com/).
1. **Copy your app files into the container:**  
   Example:

   ```dockerfile
   COPY ./ ./
   ```

### Build the Docker Image

1. Run this command to build your Docker image:

   ```
   docker build . -t sanjoke/simplewebapp:latest
   ```

### Run the Docker Container

1. Start your app in a container:

   ```
   docker run -p 8080:5000 sanjoke/simplewebapp:latest
   ```

1. **About the `-p` flag:**  
   `-p INCOMING_PORT:CONTAINER_PORT` maps a port on your computer to the app inside the container.  
   Example: `-p 8080:5000` lets you access the app at `localhost:8080`.

### View the Web Application

1. Open your browser and go to `http://localhost:8080`.

### Set the Working Directory

1. **Set a working directory in your Dockerfile:**  
   Example:

   ```dockerfile
   WORKDIR /home/simplewebapp
   ```

### Optimize Docker Builds with Caching

1.  If you copy all files before installing dependencies, Docker will reinstall dependencies every time any file changes:
    ```dockerfile
    COPY ./ ./
    RUN pip install -r requirements.txt
    ```
1.  To avoid unnecessary reinstalls, copy only `requirements.txt` first, install dependencies, then copy the rest:
    ```dockerfile
    COPY requirements.txt ./
    RUN pip install -r requirements.txt
    COPY ./ ./
    ```
1.  This way, Docker only reinstalls dependencies if `requirements.txt` changes, making builds faster.
