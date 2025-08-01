# 🐳 Docker & Kubernetes Notes

This repository contains notes and commands related to Docker and Kubernetes.

> **💡 Note:** This repository includes references to various Dockerfiles with names formatted like `Dockerfile [notes]`. When running these on your machine, be sure to remove the square brackets (`[]`) from the filename. Docker requires the file to be named exactly `Dockerfile`, without any extensions or additional characters.

## 🛠️ Manipulating Containers with `docker` CLI

### ▶️ `docker run hello-world`

Runs a test container to verify Docker installation.

```
Hello from Docker!
This message shows that your installation appears to be working correctly.
...
```

### ▶️ `docker run busybox echo 'Hello World!'`

Runs BusyBox and echoes a message.

```
Hello World!
```

### 📦 `docker create hello-world`

Creates a container without starting it.

```
a1b2c3d4e5f678901234567890abcdef1234567890abcdef1234567890abcdef
```

### 🔼 `docker start <container-id>`

Starts a container (no logs are shown).

```
a1b2c3d4e5f6
```

### 📜 `docker logs <container-id>`

Displays logs from a container.

```
Hello from Docker!
This message shows that your installation appears to be working correctly.
...
```

### 🔼📝 `docker start -a <container-id>`

Starts a container and streams its logs.

```
Hello from Docker!
This message shows that your installation appears to be working correctly.
...
```

### 📋 `docker ps`

Lists currently running containers.

```
CONTAINER ID   IMAGE         COMMAND      CREATED          STATUS          PORTS     NAMES
a1b2c3d4e5f6   busybox       "echo test"  2 minutes ago    Up 2 minutes              busy_container
```

### 📋🕑 `docker ps --all`

Lists all containers (including stopped).

```
CONTAINER ID   IMAGE         COMMAND      CREATED          STATUS                      NAMES
a1b2c3d4e5f6   hello-world   "/hello"     5 minutes ago    Exited (0) 4 minutes ago    awesome_hello
```

### 🧹 `docker system prune`

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

### 🛑 `docker stop <container-id>`

Gracefully stops a container.

```
a1b2c3d4e5f6
```

### 💀 `docker kill <container-id>`

Immediately kills a container.

```
a1b2c3d4e5f6
```

### 🏃‍♂️ `docker exec -it <container-id> <exec cmd>`

Executes a command in a running container.

```bash
bin
dev
etc
home
...
```

### 🐚 `docker exec -it <container-id> sh`

Opens a shell inside the container.

```
#
```

### 🐚▶️ `docker run -it busybox sh`

Open a shell while running the new container.

```
#
```

## 🏗️ Build Custom Images

### 🏗️ `docker build <working-dir-with-Dockerfile>`

Create new container with custom instructions.

**Example Output (with Buildkit):**

```sh
docker build .
```

```
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

```sh
DOCKER_BUILDKIT=0 docker build .
```

```
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

### 📄 Introduction to Dockerfile

A Dockerfile is a script containing a series of instructions to build a Docker image. It automates the process of creating containerized environments, ensuring consistency and reproducibility.

### 🧩 Basic Dockerfile Blocks:

1. 🏷️ Specify the base image.
2. ⚡ Run necessary boot scripts to the image.
3. 🏁 Define the commands to run during the container's startup.

```
# 1. Specify the base image.
FROM alpine

# 2. Run necessary boot scripts to the image.
RUN apk add --update redis

# 3. Define the commands to run during the container's startup.
CMD ["redis-server"]
```

### 🔄 Docker Build Flow

#### 🏷️ FROM

1. Already downloaded?
   1. ✅ Load from cache
1. ❌ NO!
   1. ⬇️ Download base image

#### ⚡ RUN

1. Get previous image
   1. Is this instruction in cache from current image?
      1. ✅ Load from cache
      1. 🆕 Generate new image out of it.
   1. ❌ NO!
      1. 🏗️ Create temporary container
      1. ⚡ Perform RUN instruction on that container
      1. 📸 Take a snapshot of temporary container
      1. 🆕 Generate new image out of it
      1. 🗑️ Destroy temporary container

#### 🏁 CMD

1. Get previous image
1. 🏗️ Create temporary container
1. 🏁 Perform CMD instruction on that container
1. 📸 Take a snapshot of temporary container and generate new image out of it
1. 🗑️ Destroy temporary container
1. if no more instruction
   1. 🆕 Return newly generated latest image id

#### 🔗 End to End Flow

```mermaid
flowchart TD
    %% %% Dockerfile Execution Flowchart
    %% CMD: Set command to run when container starts

    %% BLOCK NODES
    START([START])
    END([END])
    b1[FROM: base_image_name:tag]
    b2[Load base image from cache]
    b3[Download base image from DockerHub]

    b4[INSTRUCTIONS: RUN, COPY, ADD etc.]
    b5[Load INSTRUCTIONS from cache]
    b6[Create temporary container for INSTRUCTIONS]
    b7[Perform INSTRUCTIONS on temporary container]
    b8[Take snapshot of temporary container]
    b9[Destroy temporary container]

    b10[CMD: start_up_command_for_container]
    b11[Load CMD from cache]
    b12[Create temporary container for CMD]
    b13[Set CMD as start up command for temporary container]
    b14[Take snapshot of temporary container]
    b15[Destroy temporary container]

    %% CONDITION NODES
    c1{Is base image cached?}
    c2{Is INSTRUCTIONS cached from current image?}
    c3{Are there more INSTRUCTIONS?}
    c4{Is CMD cached from current image?}

    %% RETURN NODES
    r1[Generate & Return image: using this base image for below instructions]
    r2[Generate & Return image: using this base image for below instructions]
    r3[Generate & Return image: using this base image for below instructions]
    r4[Return Final Image]


    %% FLOW

    %% FROM: Get base image
    START --> b1
    b1 --> c1
    c1 -- Yes --> b2
    c1 -- No --> b3
    b2 --> r1
    b3 --> r1

    %% INSTRUCTIONS: RUN, COPY, ADD, etc.
    r1 --> b4
    b4 --> c2
    c2 -- Yes --> b5
    b5 --> r2
    c2 -- No --> b6
    b6 --> b7
    b7 --> b8
    b8 --> r2
    b8 --> b9
    r2 --> c3
    c3 -- Yes --> b4

    %% CMD: Set command to run when container starts
    c3 -- No --> b10
    b10 --> c4
    c4 -- Yes --> b11
    b11 --> r3
    c4 -- No --> b12
    b12 --> b13
    b13 --> b14
    b14 --> r3
    b14 --> b15

    %% Return final image ID
    r3 --> r4
    r4 --> END

     %% Style the return nodes
    class r1,r2,r3,r4 highlighted-yellow;

    classDef highlighted-yellow fill:#00ffff,color:#000,font-weight:bold;

    class START,END highlighted-green;

    classDef highlighted-green fill:#00ff00,color:#000,font-weight:bold;
```

### 🖼️ What? An Image Can Be Created from a Container!?

Yes, that's correct! You can create a new Docker image from any running container by specifying a start command (CMD).

To do this manually, use the following Docker CLI command:

```sh
docker commit -c 'CMD ["redis-server"]' <CONTAINER_ID>
```

### 🏷️ Can We Name (or Tag) Docker Images?

Absolutely! You can assign a name (or tag) to your Docker images using the following methods:

1. **🏷️ Tagging during build:**

   ```sh
   docker build -t sanjoke/redis:latest .
   ```

   - This command tags the image as you build it.
   - Here, `sanjoke` is your Docker Hub username, `redis` is the custom image name, and `latest` is the tag.

2. **🏷️ Tagging an existing image:**
   ```sh
   docker tag 9dfadec01 sanjoke/redis:latest
   ```
   - This command tags an already built image (referenced by its image ID or SHA).
   - You can use this to add or change tags for images after they are built.

## 🚀 Simple Project with Docker

### 🌐 Create a Web Application

1. Build a simple web app using any language or framework you like.

### 📝 Write a Dockerfile

1. Choose a base image from [Docker Hub](https://hub.docker.com/).
1. **📦 Copy your app files into the container:**  
   Example:

   ```dockerfile
   COPY ./ ./
   ```

### 🏗️ Build the Docker Image

1. Run this command to build your Docker image:

   ```
   docker build . -t sanjoke/simplewebapp:latest
   ```

### ▶️ Run the Docker Container

1. Start your app in a container:

   ```
   docker run -p 8080:5000 sanjoke/simplewebapp:latest
   ```

1. **🔌 About the `-p` flag:**  
   `-p INCOMING_PORT:CONTAINER_PORT` maps a port on your computer to the app inside the container.  
   Example: `-p 8080:5000` lets you access the app at `localhost:8080`.

### 👀 View the Web Application

1. Open your browser and go to `http://localhost:8080`.

### 📂 Set the Working Directory

1. **Set a working directory in your Dockerfile:**  
   Example:

   ```dockerfile
   WORKDIR /home/simplewebapp
   ```

### ⚡ Optimize Docker Builds with Caching

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
1.  This way, Docker only reinstalls dependencies if `requirements.txt` changes, making builds faster. 🚀

## 🧩 Docker Compose Introduction

**Docker Compose** is a tool used to define and manage **multi-container Docker applications**. It allows you to describe a set of services (containers), their configurations, networks, and volumes in a single **YAML file** (`docker-compose.yml`), making it easier to develop, test, and deploy applications.

### 🎯 Scenario

#### ❓ Problem

- **Problem**: Node.js app & Redis run in **separate containers**.
- **Need**: Node.js must **connect to Redis** to read/write/display data.
- **Challenge**: How to **enable communication** between these containers?

#### 🚀 Solution 1

We could **manually run both containers** and configure the **networking settings ourselves** to allow them to communicate. However, this approach requires extra effort to manage and maintain the network configuration.

#### 🚀 Solution 2

A better approach is to use a `docker-compose.yml` file. **Docker Compose simplifies container orchestration** and automatically creates a shared network so the Node.js app and Redis container can easily connect and communicate with each other.

### 📝 Docker Compose File Explanation (Beginner Friendly)

This document explains each line of the following `docker-compose.yml` file:

```yaml
services:
  redis-server:
    image: redis
  node-app:
    build: .
    ports:
      - "4001:8081"
    restart: always
```

---

#### 🧩 `services:`

- **What it does:** Defines a group of services (containers) that will be managed by Docker Compose.
- **Why it matters:** Each service represents a separate container in your app (like a database or web server).

  ##### 🗄️ `database-server:`

  - **What it does:** This is the name of the first service (Customizable).
  - **Why it matters:** You can use this name to refer to the service in networks or from other services (e.g., in environment variables).
  - **Referance:** We can directly refer this name in our webapp code as host, **_Docker will automatically convert it into database URI_**.

    ```js
    const client = redis.createClient({
      host: "database-server",
      port: 6379,
    });
    ```

  ###### 🐳 `image: redis`

  - **What it does:** Tells Docker to use the official **Redis image** from Docker Hub.
  - **Why it matters:** This will pull and run a Redis server without needing a custom Dockerfile.

  ##### 🖥️ `webapp-server:`

  - **What it does:** This is the name of the second service (Customizable).
  - **Why it matters:** This service will be your custom Node.js application.

    ###### 🏗️ `build: .`

    - **What it does:** Tells Docker to build an image using the `Dockerfile` in the **current directory** (`.`).
    - **Why it matters:** Instead of using a prebuilt image, this builds one from your app's source code.

    ###### 🔌 `ports:`

    - **What it does:** Maps ports between your computer and the container.
    - **Why it matters:** This allows you to access the app running inside the container from your browser or other tools.

    ###### 🔢 `- "4001:8081"`

    - **What it does:** Maps **port 4001** on your computer to **port 8081** in the container.
    - **Why it matters:** You can visit `http://localhost:4001` to access the app inside the container.

    ###### 🔄 `restart: always`

    - **What it does:** Automatically restarts the container if it crashes or your computer restarts.
    - **Why it matters:** Helps keep your services running without manual intervention.
    - **Options:**
      - `no`: Never restart automatically (default).
      - `always`: Always restart.
      - `on-failure`: Restart only on non-zero exit codes.
      - `unless-stopped`: Restart unless you manually stop the container.

## 🛠️ Manipulating Containers with `docker-compose` CLI

### `docker-compose up`

➡️ **Starts and runs all services defined in the `docker-compose.yml` file.**

### `docker-compose up --build`

➡️ **Builds images before starting the containers, useful when code or Dockerfile changes.**

### `docker-compose up -d`

➡️ **Starts all services in the background (detached mode), allowing the terminal to remain free.**

### `docker-compose down`

➡️ **Stops and removes all running containers, networks, and volumes created by `up`.**

### `docker-compose ps`

➡️ **Lists the current status of all running containers managed by Docker Compose.**

Absolutely! Here's a detailed **reference and explanation document for `docker-compose.yml`** tailored to your React development setup.

---

## 📘 `docker-compose.yml` Reference for React Development

This document explains how the `docker-compose.yml` file is structured and how it helps set up a robust development environment for a React application using Docker.

---

```yaml
services:
  react-app-dev:
    build:
      context: .
      dockerfile: Dockerfile.dev
    volumes:
      - /app/node_modules
      - .:/app
    ports:
      - "3000:3000"
```

---

### 🔢 `version: '3.8'`

- Specifies the version of the Docker Compose file format.
- Version `3.8` is widely compatible with recent Docker versions and supports features like named volumes and service configuration enhancements.

---

### 🧱 `services:`

- Defines all the containerized services needed for your application.
- Here, there's only one service: `react-app-dev`.

---

### 📦 `react-app-dev:`

This is the service definition for the React development container.

#### 🏗️ `build:`

Specifies how Docker should build the image for this service.

```yaml
context: .
dockerfile: Dockerfile.dev
```

- **`context: .`** — Uses the current directory as the build context (all files are sent to Docker during build).
- **`dockerfile: Dockerfile.dev`** — Points to a custom Dockerfile optimized for development (usually contains hot-reloading and unminified code).

---

#### 📁 `volumes:`

Volumes are used to persist data or mount local files into the container.

```yaml
- /app/node_modules
- .:/app
```

- **`/app/node_modules`**
  An anonymous volume that ensures the **container maintains its own `node_modules`** directory. Yes we sync all current directory to `/app` folder inside continer in next live but except `/app/node_modules` folder.

- **`.:/app`**
  Mounts the current directory (`.`) from the host into `/app` in the container. Enables **live syncing** — changes made locally appear inside the container instantly.

> 📝 Tip: This setup ensures hot-reloading works while protecting `node_modules` from being overwritten.

---

#### 🌐 `ports:`

Maps network ports from the container to your host machine.

```yaml
- "3000:3000"
```

- Maps **port 3000** in the container (where the React dev server runs) to **port 3000** on your host.
- Allows you to access the app by visiting `http://localhost:3000` in your browser.

## 🐳 Multi-Stage Dockerfile for React App Deployment

This Dockerfile uses **multi-stage builds** to create a production-ready React app image. It first builds the app using Node.js and then serves it using Nginx.

```yml
FROM node:lts-alpine as build

WORKDIR /app

COPY package.json ./
RUN npm install

COPY ./ ./
RUN npm run build

FROM nginx:alpine
COPY --from=build /app/build /usr/share/nginx/html
```

---

### 🧱 **Stage 1: Build the React App**

```dockerfile
FROM node:lts-alpine as build
```

- **Base Image**: Uses a lightweight Node.js image (`node:lts-alpine`) to install dependencies and build the app.
- **Alias `build`**: This name (`build`) is used later to refer to this stage.

```dockerfile
WORKDIR /app
```

- **Set Working Directory**: All subsequent commands will be run inside the `/app` directory.

```dockerfile
COPY package.json ./
RUN npm install
```

- **Copy `package.json`**: Copies only `package.json` first to leverage Docker’s layer caching.
- **Install Dependencies**: Runs `npm install` to install all node modules.

```dockerfile
COPY ./ ./
RUN npm run build
```

- **Copy Source Files**: Copies the full app source into the container.
- **Build React App**: Compiles the React project into static files inside `/app/build`.

---

### 🚀 **Stage 2: Serve with Nginx**

```dockerfile
FROM nginx:alpine
```

- **Base Image**: Uses a lightweight Nginx image to serve static files efficiently in production.

```dockerfile
COPY --from=build /app/build /usr/share/nginx/html
```

- **Copy Build Output**: Copies the compiled React app from the `build` stage into the default Nginx web directory.

---

### ✅ Final Image Behavior

- The resulting image **only contains the production-ready static files and Nginx**, making it lightweight and secure.
- React app is served at the Nginx default port `80`.

## Other CLI Commands & Documents

### `docker build -f Dockerfile.dev .`

➡️ **Builds a Docker image using the development-specific Dockerfile.**

### `docker run -v $(pwd):/app <container-id>`

➡️ **Runs a container with the current directory mounted to `/app` inside the container for live code syncing.**

### `docker run -v /app/node_modules -v $(pwd):/app <container-id>`

➡️ **Runs a container while ensuring the container's `/app/node_modules` directory is preserved and not overwritten by a volume mount.**

### `docker run -v /app/node_modules -v $(pwd):/app <container-id>`

➡️ **Runs a container with the current directory mounted to `/app` and preserves the container's `/app/node_modules` to avoid overwriting with the host's version.**

### Define custom Dockerfile & update volumes in `docker-compose.yml`

```yml
services:
  react-app-dev:
    build:
      context: .
      dockerfile: Dockerfile.dev
    volumes:
      - /app/node_modules
      - .:/app
    ports:
      - "3000:3000"
```

# Introduction to Kubernetes

## WHAT?

Kubernetes (often abbreviated as K8s) is an open-source system for automating the deployment, scaling, and management of containerized applications.

It helps you run and coordinate many containers across a cluster of machines (physical or virtual), treating them as a single unit. Instead of running your containers manually on individual servers, Kubernetes allows you to define how your applications should run, and then manages the rest for you — including scheduling, scaling, networking, and failover.

**Example:**
Imagine you have an application with a frontend, backend, and database — each as a separate Docker container. Kubernetes lets you deploy all three across multiple servers, ensuring they run reliably and can communicate with each other. If the frontend crashes, Kubernetes automatically restarts it.

## WHY?

Running a few containers on a single machine is simple. But in a real-world application, you might need to:

- Run hundreds of containers
- Spread them across different machines
- Ensure availability if something fails
- Automatically scale up or down based on traffic
- Perform updates without downtime

Kubernetes handles all of this for you.

**Example:**
Suppose your e-commerce site experiences a traffic spike during a sale. Kubernetes can detect the increased load and automatically spin up more frontend containers to handle the traffic, and scale them down when it's over — without manual intervention.

## Key Terms & Points

- Kubernetes is a system to deploy containterized apps
- Kubernetes Cluster
  - Worker Nodes are individual machines or vm's that run containers
  - Master Node are machines. or vm's with a set of programs to manage nodes
- Kubernetes didn't build our images - it got them from remote (docker hub)
- Kubernetes's master decide where to run each container & each worker node can run any set or containers.
- Kubernetes's get all informations via config file which will be picked by master to perform task.
- Kubernetes's master works constantly to meet your desired state

## minikube & kubectl

kubectl – Used to manage containers and Kubernetes objects inside the VM (Node).

minikube or Docker Desktop with Kubernetes – Used to manage the VM (Node) that runs the Kubernetes cluster.

## Example Config File

[client-pod.yml](simplek8s/client-pod.yml)

```yml
apiVersion: v1
kind: Pod
metadata:
  name: client-pod
  labels:
    component: web
spec:
  containers:
    - name: client
      image: stephengrider/multi-client
      ports:
        - containerPort: 3000
```

[client-node-port.yml](simplek8s/client-node-port.yml)

```yml
apiVersion: v1
kind: Service
metadata:
  name: client-node-port
spec:
  type: NodePort
  ports:
    - port: 3050
      targetPort: 3000
      nodePort: 31515
  selector:
    component: web
```

## Types of Object in k8s

Here's your updated section with **Persistent Volume (PV)** and **Persistent Volume Claim (PVC)** added in a clean, consistent format:

---

## Types of Object in k8s

* **Pod**

  * A Pod is the smallest unit in Kubernetes.
  * It can contain one or more containers.
  * If containers are tightly coupled (e.g., they must run together and share resources), group them in the same Pod.
  * For all independent containers, create separate Pods.
  * This is different from docker-compose, where multiple services can be defined together regardless of how tightly they are coupled.
  * Good for dev, but not for prod.

* **Service**

  * Sets up networking in a k8s cluster
  * Types: NodePort, ClusterIP, LoadBalancer, Ingress

    * **NodePort** – Exposes a container to the outside world (only for dev, not for prod)
    * **ClusterIP** – Default service type. Exposes the service internally within the cluster. Other services or pods can access it, but it’s not accessible from outside the cluster. Ideal for internal communication between microservices.
    * **LoadBalancer** - (Legacy) Provisions an external load balancer (via the cloud provider) and assigns a public IP to expose the service outside the cluster. Commonly used in production for exposing services to the internet. Automatically routes traffic to backend pods.
    * **Ingress** - (Advance) A more advanced and flexible way to expose HTTP/HTTPS services. Instead of creating a LoadBalancer per service, Ingress uses a single entry point (usually backed by a LoadBalancer) to route traffic based on paths or hostnames to multiple services. Ideal for managing multiple services behind a single public IP with routing rules.

    | LoadBalancer           | Ingress                  |
    | ---------------------- | ------------------------ |
    | One per service        | One for many services    |
    | Higher cloud cost      | Cost-efficient           |
    | Simple to set up       | More setup, but flexible |
    | No routing logic       | Supports routing rules   |
    | Works for any protocol | Best for HTTP/S traffic  |


* **Deployment**

  * Runs a set of identical pods (one or more)
  * Monitors the state of each pod, updating as necessary
  * Good for dev and prod

* **Persistent Volume Claim (PVC)**

  * A request made by a pod (or user) for storage.
  * Specifies size and access mode requirements.
  * Kubernetes automatically binds a matching PV to the PVC.
  * Pods use PVCs to access persistent storage, making data available even if pods are deleted or rescheduled.

* **Secret**

  * Used to store sensitive information like passwords, tokens, and SSH keys.
  * Encoded in base64 but not encrypted by default (requires encryption at rest or an external secrets manager for full security).
  * Can be mounted into pods as files or exposed as environment variables.
  * Helps keep sensitive data separate from application code and configuration.
  * Mostly we create Secret using CLI, as we dont want to store them as a plain text in config file.
    
    ```shell
    # kubectl create secret (generic|docker-registry|tls) <secret-name> --from-literal <key>=<value>
    kubectl create secret generic db-secret --from-literal username=XXXX --from-literal password=XXXX
    ```

## `selector` and `label` mapping

### 🟢 `client-pod.yml`

- **Label**: `component: web` ← 🔑 This is important

---

### 🌐 `client-node-port.yml`

- 🔍 **Selector**: `component: web` ← 🔑 exact same key

---

### 💡 How They Work Together

- The **Service** finds the **Pod** by matching the label `component: web`.
- Then it forwards traffic from:

  - `Node IP:31515` → to → `client-pod:3000`

- So if you go to `http://<NodeIP>:31515`, you'll reach the client container running inside the pod.

## `ports` mapping (type: NodePort)

### 🌐 1. **NodePort** (External Port on the Node)

- This is the **port on the physical/virtual machine** (Node) that accepts traffic from outside the cluster.
- In your YAML:
  `nodePort: 31515`
- You can access it via:
  `http://<NodeIP>:31515`

---

### 🚪 2. **Service Port**

- This is the **internal port** that the Kubernetes **Service** listens on.
- Other services inside the cluster would use this port.
- In your YAML:
  `port: 3050`

> In a NodePort service, this port isn’t usually important for external traffic — it's more relevant for internal service-to-service communication.

---

### 🎯 3. **Target Port** (Inside the Pod)

- This is the **port on the container** where your actual application is running.
- In your YAML:
  `targetPort: 3000`
- That matches what's in your [client-pod.yml](simplek8s/client-pod.yml):
  The container exposes `port: 3000`.

---

So traffic flows like:

```
You → NodeIP:31515 → Service (3050) → Pod (3000)
```

## Kubernetes CLI Commands

### 🏃‍♂️ `kubectl apply -f <filename>/<folder>`

> pod updates may not change fields other than `spec.containers[*].image`,`spec.initContainers[*].image`,`spec.activeDeadlineSeconds`,`spec.tolerations` (only additions to existing tolerations),`spec.terminationGracePeriodSeconds` (allow it to be set to 1 if it was previously negative)

### 🏃‍♂️ `kubectl get <object-type>(s)` or `kubectl get <object-type> <object-name>`

### 🏃‍♂️ `kubectl describe <object-type>(s)` or `kubectl describe <object-type> <object-name>`

### 🏃‍♂️ `kubectl delete -f <filename>`

### 🏃‍♂️ `kubectl set image <object-type>/<object-name> <container-name-from-config>=<new-image-tag>`

### 🏃‍♂️ `kubectl delete <object-type> <object-name>`

### 🏃‍♂️ `kubectl logs <pod-name>`

### 🏃‍♂️ `kubectl get storageclass`

## Object File Combining 

```yml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: client-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      component: web
  template:
    metadata:
      labels:
        component: web
    spec:
      containers:
        - name: client
          image: stephengrider/multi-client
          ports:
            - containerPort: 3000

--- # Just by spliting the configuration by ---, we can have all object config in single file

apiVersion: v1
kind: Service
metadata:
  name: client-cluster-ip-service
spec:
  type: ClusterIP
  selector:
    component: web
  ports:
    - port: 3000
      targetPort: 3000
```

Here's a documentation-style overview for **Skaffold**, tailored to your use case of local Kubernetes development with live reloading:

---

# Skaffold

**Skaffold** is a command-line tool that automates the workflow for building, pushing, and deploying applications to Kubernetes — with real-time reloading, perfect for **local development**.

It handles the tedious parts of the developer lifecycle so you can focus on writing code, not rebuilding and redeploying manually.

---

## ✨ Features

* **File Sync + Live Reloading**
  Automatically rebuilds images and redeploys when source code changes.

* **Supports Multiple Build Tools**
  Docker, Buildpacks, Bazel, Kaniko, custom scripts, and more.

* **Flexible Deployment Options**
  Works with `kubectl`, `helm`, `kustomize`, and others.

* **Port Forwarding & Logs**
  View container logs, and automatically port-forward services to localhost.

* **Profiles**
  Switch between dev, staging, prod environments easily.

---

## 🔧 Use Case

> "A real-time reloader for Kubernetes local development."

Skaffold watches your files, rebuilds your images, and redeploys to your dev cluster on every save. Ideal for rapid feedback when working with Kubernetes.

---

## 📁 Example `skaffold.yaml`

```yaml
apiVersion: skaffold/v4beta6
kind: Config
metadata:
  name: my-app
build:
  artifacts:
    - image: my-app
      context: .
      docker:
        dockerfile: Dockerfile
deploy:
  kubectl:
    manifests:
      - k8s/*.yaml
portForward:
  - resourceType: service
    resourceName: my-app
    port: 8080
    localPort: 8080
```


# Referance 

- https://github.com/StephenGrider/DockerCasts
- https://hub.docker.com/
- https://github.com/kubernetes/ingress-nginx
- https://www.joyfulbikeshedding.com/blog/2018-03-26-studying-the-kubernetes-ingress-system.html
- https://kubernetes.github.io/ingress-nginx/deploy/
- https://helm.sh/docs/intro/install/#from-script
- https://kubernetes.github.io/ingress-nginx/deploy/#using-helm
- https://skaffold.dev/docs/references/yaml/
