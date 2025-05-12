# Docker & Kubernetes Notes

This repository contains notes and commands related to Docker and Kubernetes.

---

### Basic Docker Commands with Examples

---

1. **`docker run hello-world`**

   _Runs a test container to verify Docker installation._

   **Example Output:**

   ```
   Hello from Docker!
   This message shows that your installation appears to be working correctly.
   ...
   ```

1. **`docker run busybox echo 'Hello World!'`**

   _Runs BusyBox and echoes a message._

   **Example Output:**

   ```
   Hello World!
   ```

1. **`docker create hello-world`**

   _Creates a container without starting it._

   **Example Output:**

   ```
   a1b2c3d4e5f678901234567890abcdef1234567890abcdef1234567890abcdef
   ```

1. **`docker start <container-id>`**

   _Starts a container (no logs are shown)._

   **Example Output:**

   ```
   a1b2c3d4e5f6
   ```

1. **`docker logs <container-id>`**

   _Displays logs from a container._

   **Example Output:**

   ```
   Hello from Docker!
   This message shows that your installation appears to be working correctly.
   ...
   ```

1. **`docker start -a <container-id>`**

   _Starts a container and streams its logs._

   **Example Output:**

   ```
   Hello from Docker!
   This message shows that your installation appears to be working correctly.
   ...
   ```

1. **`docker ps`**

   _Lists currently running containers._

   **Example Output:**

   ```
   CONTAINER ID   IMAGE         COMMAND      CREATED          STATUS          PORTS     NAMES
   a1b2c3d4e5f6   busybox       "echo test"  2 minutes ago    Up 2 minutes              busy_container
   ```

1. **`docker ps --all`**

   _Lists all containers (including stopped)._

   **Example Output:**

   ```
   CONTAINER ID   IMAGE         COMMAND      CREATED          STATUS                      NAMES
   a1b2c3d4e5f6   hello-world   "/hello"     5 minutes ago    Exited (0) 4 minutes ago    awesome_hello
   ```

1. **`docker system prune`**

   _Cleans up unused data._

   **Example Output:**

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

1. **`docker stop <container-id>`**

   _Gracefully stops a container._

   **Example Output:**

   ```
   a1b2c3d4e5f6
   ```

1. **`docker kill <container-id>`**

   _Immediately kills a container._

   **Example Output:**

   ```
   a1b2c3d4e5f6
   ```

1. **`docker exec -it <container-id> <exec cmd>`**

   _Executes a command in a running container._

   **Example Output:**

   ```bash
   bin
   dev
   etc
   home
   ...
   ```

1. **`docker exec -it <container-id> sh`**

   _Opens a shell inside the container._

   **Example Output:**

   ```
   #
   ```

1. **`docker run -it busybox sh`**

   _Open a shell while running the new continer._

   **Example Output:**

   ```
   #
   ```
