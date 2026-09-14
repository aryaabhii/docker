# 🐳 Docker & CI/CD Notes

A beginner-friendly guide to Docker, Docker Images, Containers, Docker Compose, Volumes, Networking, Dockerfile, and CI/CD.

---

# 🐳 What is Docker?

**Docker** is a platform that helps us build, package, run, and deploy applications inside **containers**.

Docker allows us to package an application along with its required dependencies into a single portable unit called a **container**.

### Example

```text
Application
├── Source Code
├── Runtime
├── Libraries
├── Dependencies
└── Configuration
```

With Docker:

```text
Docker Container
├── Application
├── Runtime
├── Libraries
└── Dependencies
```

---

# 📦 What is a Container?

A **container** is a lightweight and isolated environment where an application and its dependencies can run together.

### Properties of Containers

### 1. Portable

Containers can be moved and run across different systems that support Docker.

```text
Developer Machine
       ↓
     Docker
       ↓
   Container
       ↓
 Server / Cloud
```

### 2. Lightweight

Containers are generally lightweight and start much faster than traditional virtual machines because they share the host operating system's kernel.

### 3. Isolated

Containers provide an isolated environment for applications and their dependencies.

---

# 🖼️ What is a Docker Image?

A **Docker Image** is a read-only template used to create Docker containers.

It contains the files, dependencies, configuration, and instructions required to run an application.

We can remember it like this:

```text
Docker Image     = Blueprint
Docker Container = Running Instance
```

Example:

```text
Docker Image
     ↓
 docker run
     ↓
Docker Container
```

One Docker image can be used to create multiple containers.

```text
              Docker Image
                   |
        ┌──────────┼──────────┐
        ↓          ↓          ↓
   Container 1  Container 2  Container 3
```

---

# 🐳 Docker Commands

## 1. Pull Image from Docker Hub

```bash
docker pull IMAGE_NAME
```

Example:

```bash
docker pull ubuntu
```

---

## 2. Pull Specific Image Version / Tag

```bash
docker pull IMAGE_NAME:TAG
```

Example:

```bash
docker pull node:20
```

---

## 3. See Docker Images

```bash
docker images
```

or

```bash
docker image ls
```

---

# 🚀 Docker Containers

## 4. Create and Run Container

```bash
docker run IMAGE_NAME
```

Example:

```bash
docker run ubuntu
```

`docker run` creates a new container from an image and starts it.

---

## 5. Run Container in Interactive Mode

```bash
docker run -it IMAGE_NAME
```

Example:

```bash
docker run -it ubuntu
```

Where:

```text
-i = Interactive
-t = Allocate Terminal
```

This allows us to interact with the container terminal.

---

## 6. Run Container in Detached Mode

```bash
docker run -d IMAGE_NAME
```

Example:

```bash
docker run -d nginx
```

`-d` runs the container in the background.

---

## 7. Give Custom Name to Container

```bash
docker run --name CONTAINER_NAME -d IMAGE_NAME
```

Example:

```bash
docker run --name my-nginx -d nginx
```

---

# 🔍 Checking Containers

## 8. Show Running Containers

```bash
docker ps
```

---

## 9. Show All Containers

```bash
docker ps -a
```

This shows both running and stopped containers.

---

# ⏯️ Start / Stop / Restart Containers

## 10. Stop Container

```bash
docker stop CONTAINER_NAME
```

or

```bash
docker stop CONTAINER_ID
```

Example:

```bash
docker stop my-nginx
```

---

## 11. Start Existing Container

```bash
docker start CONTAINER_NAME
```

or

```bash
docker start CONTAINER_ID
```

---

## 12. Restart Existing Container

```bash
docker restart CONTAINER_NAME
```

or

```bash
docker restart CONTAINER_ID
```

---

# 🗑️ Remove Containers and Images

## 13. Remove Container

```bash
docker rm CONTAINER_NAME
```

or

```bash
docker rm CONTAINER_ID
```

If the container is running:

```bash
docker stop CONTAINER_NAME
```

```bash
docker rm CONTAINER_NAME
```

---

## 14. Remove Docker Image

```bash
docker rmi IMAGE_NAME
```

Example:

```bash
docker rmi ubuntu
```

---

# 🔌 Port Binding

Port binding allows us to access an application running inside a container from the host machine.

Syntax:

```bash
docker run -p HOST_PORT:CONTAINER_PORT IMAGE_NAME
```

Example:

```bash
docker run -p 8080:3033 my-app
```

This means:

```text
Host Machine
localhost:8080
       ↓
Docker Container
port 3033
```

---

# 📜 Docker Logs

To see container logs:

```bash
docker logs CONTAINER_ID
```

Example:

```bash
docker logs my-app
```

To continuously follow logs:

```bash
docker logs -f my-app
```

---

# 💻 Execute Commands Inside Container

## Using Bash

```bash
docker exec -it CONTAINER_ID /bin/bash
```

Example:

```bash
docker exec -it my-container /bin/bash
```

---

## Using Shell

Some lightweight images don't contain Bash.

In that case:

```bash
docker exec -it CONTAINER_ID /bin/sh
```

Example:

```bash
docker exec -it my-container /bin/sh
```

---

# 🌐 Docker Networking

Docker networking allows containers to communicate with each other.

## 15. Show Networks

```bash
docker network ls
```

---

## 16. Create Network

```bash
docker network create NETWORK_NAME
```

Example:

```bash
docker network create my-network
```

---

## 17. Remove Network

```bash
docker network rm NETWORK_NAME
```

Example:

```bash
docker network rm my-network
```

---

# 📦 Docker Volumes

Docker volumes are used to persist data outside the container's writable layer.

This is useful because data inside a container can be lost when the container is removed.

```text
Container
    ↓
 Volume
    ↓
Persistent Data
```

Volumes are commonly used for:

* Databases
* Uploaded files
* Application data
* Persistent storage

---

# Volume Types

## 1. Named Volume

```bash
docker run -v VOLUME_NAME:CONTAINER_PATH IMAGE_NAME
```

Example:

```bash
docker run -v mysql_data:/var/lib/mysql mysql
```

Here:

```text
mysql_data
     ↓
/var/lib/mysql
```

---

## 2. Anonymous Volume

```bash
docker run -v CONTAINER_PATH IMAGE_NAME
```

Example:

```bash
docker run -v /var/lib/mysql mysql
```

Docker automatically creates a volume without requiring us to provide a name.

---

## 3. Bind Mount

A bind mount maps a directory from the host machine to a directory inside the container.

```bash
docker run -v HOST_DIRECTORY:CONTAINER_DIRECTORY IMAGE_NAME
```

Example:

```bash
docker run -v ./data:/app/data my-app
```

Here:

```text
Host
./data
  ↓
Container
/app/data
```

---

## 4. List Volumes

```bash
docker volume ls
```

---

## 5. Remove Unused Volumes

```bash
docker volume prune
```

> ⚠️ Be careful with this command because unused volumes may contain important data.

---

# 📄 Dockerfile

A **Dockerfile** contains instructions for building a Docker image.

It tells Docker how to package our application.

### Example

```dockerfile
FROM node:20

WORKDIR /app

COPY package*.json ./

RUN npm install

COPY . .

EXPOSE 3000

CMD ["npm", "start"]
```

---

# Dockerfile Instructions

| Instruction  | Purpose                               |
| ------------ | ------------------------------------- |
| `FROM`       | Defines the base image                |
| `WORKDIR`    | Sets the working directory            |
| `COPY`       | Copies files into the image           |
| `ADD`        | Adds files/directories                |
| `RUN`        | Executes commands while building      |
| `EXPOSE`     | Documents the application port        |
| `CMD`        | Default command when container starts |
| `ENTRYPOINT` | Defines the main executable           |

---

# 🏗️ Build Docker Image

If the Dockerfile is in the current directory:

```bash
docker build -t my-app .
```

Explanation:

```text
docker build
     ↓
-t my-app
     ↓
Image Name
     ↓
.
     ↓
Current Directory
```

Then run:

```bash
docker run -p 3000:3000 my-app
```

---

# 🆚 Docker Image vs Container

| Docker Image              | Docker Container           |
| ------------------------- | -------------------------- |
| Blueprint / Template      | Running instance           |
| Read-only template        | Has a writable layer       |
| Used to create containers | Created from an image      |
| Can be stored and shared  | Can be started and stopped |
| Example: `node:20`        | Example: `my-node-app`     |

Remember:

```text
IMAGE
  ↓
docker run
  ↓
CONTAINER
```

---

# 🌐 Docker Compose

**Docker Compose** is a tool used to define and run multi-container applications.

Instead of manually running multiple Docker commands, we can define all services inside a YAML file.

Common files:

```text
compose.yml
```

or

```text
docker-compose.yml
```

---

# Example Docker Compose

Suppose our application contains:

```text
Frontend
Backend
Database
```

We can define them in a Compose file:

```yaml
services:

  frontend:
    image: nginx
    ports:
      - "8080:80"

  backend:
    image: node:20

  database:
    image: mysql:8
```

---

# Docker Compose Commands

## 1. Start Services

```bash
docker compose up
```

---

## 2. Start Services in Detached Mode

```bash
docker compose up -d
```

---

## 3. Run Using Specific YAML File

```bash
docker compose -f filename.yml up -d
```

Example:

```bash
docker compose -f docker-compose.yml up -d
```

---

## 4. Stop and Remove Services

```bash
docker compose down
```

---

## 5. Stop Using Specific YAML File

```bash
docker compose -f filename.yml down
```

Example:

```bash
docker compose -f docker-compose.yml down
```

---

# 🌐 Docker Compose Networking

Docker Compose automatically creates a network for services defined in the Compose application.

Example:

```text
       Docker Compose Network
                |
       ┌────────┼────────┐
       ↓        ↓        ↓
   Frontend   Backend   Database
```

Services can communicate with each other using their service names.

For example:

```text
Backend → Database
```

The backend can use:

```text
database
```

as the database hostname.

Therefore, for a basic Compose application, we usually don't need to manually create a Docker network.

---

# 🆚 Docker vs Virtual Machine

| Docker Container        | Virtual Machine            |
| ----------------------- | -------------------------- |
| Lightweight             | Heavier                    |
| Starts quickly          | Usually slower             |
| Shares host OS kernel   | Includes complete guest OS |
| Uses fewer resources    | Uses more resources        |
| Smaller deployment size | Larger deployment size     |

---

# 🖥️ Docker Desktop

Docker Desktop provides a convenient way to use Docker on Windows and macOS.

On Windows and macOS, Docker Desktop commonly uses a lightweight Linux virtualized environment for running Linux containers.

---

# 🔄 CI/CD Pipeline

CI/CD stands for:

```text
CI = Continuous Integration
CD = Continuous Delivery / Continuous Deployment
```

---

# 🔵 Continuous Integration

Continuous Integration means developers frequently merge their code into a shared repository.

Automated processes can then:

```text
Code Push
    ↓
Build
    ↓
Test
    ↓
Code Validation
```

---

# 🟢 Continuous Delivery

Continuous Delivery means that after the application successfully passes automated checks, it is prepared and kept ready for deployment.

```text
Code
 ↓
Build
 ↓
Test
 ↓
Package
 ↓
Ready for Deployment
```

---

# 🟠 Continuous Deployment

Continuous Deployment automatically deploys successfully tested changes to the production environment.

```text
Developer
    ↓
Git Push
    ↓
CI/CD Pipeline
    ↓
Build
    ↓
Test
    ↓
Docker Image
    ↓
Deploy
    ↓
Production
```

---

# 🐳 Docker + CI/CD

Docker is commonly used in CI/CD pipelines to provide consistent application environments.

Example workflow:

```text
Developer
    │
    │ git push
    ↓
GitHub / GitLab
    │
    ↓
CI/CD Pipeline
    │
    ├── Install Dependencies
    ├── Run Tests
    ├── Build Docker Image
    ├── Push Image
    │
    ↓
Docker Registry
    │
    ↓
Production Server
    │
    ↓
Docker Container
```

---

# 📚 Docker Command Cheat Sheet

| Task                  | Command                               |
| --------------------- | ------------------------------------- |
| Pull image            | `docker pull IMAGE`                   |
| List images           | `docker images`                       |
| Run container         | `docker run IMAGE`                    |
| Interactive container | `docker run -it IMAGE`                |
| Detached container    | `docker run -d IMAGE`                 |
| Custom container name | `docker run --name NAME IMAGE`        |
| Port mapping          | `docker run -p HOST:CONTAINER IMAGE`  |
| Running containers    | `docker ps`                           |
| All containers        | `docker ps -a`                        |
| Start container       | `docker start CONTAINER`              |
| Stop container        | `docker stop CONTAINER`               |
| Restart container     | `docker restart CONTAINER`            |
| Remove container      | `docker rm CONTAINER`                 |
| Remove image          | `docker rmi IMAGE`                    |
| Container logs        | `docker logs CONTAINER`               |
| Enter container       | `docker exec -it CONTAINER /bin/bash` |
| List networks         | `docker network ls`                   |
| Create network        | `docker network create NAME`          |
| Remove network        | `docker network rm NAME`              |
| Build image           | `docker build -t NAME .`              |
| Compose start         | `docker compose up`                   |
| Compose background    | `docker compose up -d`                |
| Compose stop/remove   | `docker compose down`                 |
| List volumes          | `docker volume ls`                    |
| Remove unused volumes | `docker volume prune`                 |

---

# 🎯 Docker Learning Flow

A good order for learning Docker:

```text
1. Docker Basics
       ↓
2. Images
       ↓
3. Containers
       ↓
4. Dockerfile
       ↓
5. Port Mapping
       ↓
6. Volumes
       ↓
7. Networks
       ↓
8. Docker Compose
       ↓
9. Docker Registry
       ↓
10. Docker + CI/CD
       ↓
11. Docker Deployment
```

---

# 🧠 Important Concepts

```text
Docker
  │
  ├── Image
  │      └── Blueprint
  │
  ├── Container
  │      └── Running instance of an image
  │
  ├── Dockerfile
  │      └── Instructions to build an image
  │
  ├── Volume
  │      └── Persistent storage
  │
  ├── Network
  │      └── Container communication
  │
  └── Docker Compose
         └── Manage multiple services
```

---

# 🚀 Real-World Application Example

A typical web application can contain:

```text
                 Docker Compose
                       │
        ┌──────────────┼──────────────┐
        ↓              ↓              ↓
     Frontend        Backend        Database
     Next.js         Laravel          MySQL
        │              │              │
        └──────────────┼──────────────┘
                       │
                  Docker Network
                       │
                    Volumes
                       │
                 Persistent Data
```

Docker makes it easier to develop, test, and deploy the complete application consistently.

---

# 📝 Conclusion

Docker provides a standardized way to package and run applications.

The most important concepts are:

* **Image** → Blueprint
* **Container** → Running instance
* **Dockerfile** → Instructions for building an image
* **Volume** → Persistent storage
* **Network** → Communication between containers
* **Docker Compose** → Manage multiple services
* **CI/CD** → Automate build, testing, and deployment

These concepts provide a strong foundation for modern application development and deployment.
