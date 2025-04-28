# Docker Teaching Notes

## 1. What is Docker?

Docker is a tool that lets you **package** an application and all its **dependencies** into a **container**.

A **container** is like a small, lightweight **virtual machine**, but much faster and more efficient.

**Analogy:**  
> Docker containers are like shipping containers — no matter what’s inside, they’re moved the same way.

---

## 2. Why Use Docker?

- **Consistency:** Same app behavior everywhere (laptop, server, cloud).
- **Isolation:** Applications don't interfere with each other.
- **Portability:** Move containers easily across different systems.
- **Efficiency:** Containers use fewer resources than virtual machines.

---

## 3. Key Docker Concepts

| Concept | Meaning |
|:--------|:--------|
| **Image** | Blueprint for a container (e.g., Ubuntu + your app) |
| **Container** | Running instance of an image |
| **Dockerfile** | Instructions for building a Docker image |
| **Docker Hub** | Online library where you can download pre-built images |

---

## 4. Basic Docker Workflow

### Building and Running Containers

**Write a Dockerfile** (example):
```Dockerfile
# Use an official base image
FROM node:14

# Set working directory
WORKDIR /app

# Copy project files
COPY . .

# Install dependencies
RUN npm install

# Start the app
CMD ["node", "app.js"]


```markdown
# Docker Commands Guide

## 1. Build an Image from the Dockerfile

```bash
docker build -t myapp .
```

## 2. Run a Container from the Built Image

```bash
docker run -d -p 8080:80 myapp
```

## 3. View Running Containers

```bash
docker ps
```

## 4. Stop a Running Container

```bash
docker stop <container_id>
```

## 5. View All Containers (Even Stopped Ones)

```bash
docker ps -a
```

## 6. Remove a Container

```bash
docker rm <container_id>
```

## 7. List All Local Images

```bash
docker images
```

## 8. Remove an Image

```bash
docker rmi <image_id>
```

## 9. Pull an Image from Docker Hub

```bash
docker pull nginx
```

## 10. Push an Image to Docker Hub (After Login)

```bash
docker push myusername/myapp
```

## 11. View Container Logs

```bash
docker logs <container_id>
```

## 12. Enter Inside a Running Container

```bash
docker exec -it <container_id> /bin/bash
```

---

# 5. Important Docker Commands

| Command | Description |
|:--------|:------------|
| `docker build -t name .` | Build an image from a Dockerfile |
| `docker run -d -p 8080:80 image` | Run a container in detached mode |
| `docker ps` | List running containers |
| `docker ps -a` | List all containers |
| `docker stop <container_id>` | Stop a running container |
| `docker rm <container_id>` | Remove a container |
| `docker images` | List all images |
| `docker rmi <image_id>` | Remove an image |
| `docker pull <image>` | Download an image from Docker Hub |
| `docker push <image>` | Upload an image to Docker Hub |
| `docker logs <container_id>` | View logs of a container |
| `docker exec -it <container_id> /bin/bash` | Open a shell inside a container |
| `docker system prune` | Clean up unused containers, images, and volumes |

---

# 6. Practical Demo

## Run Your First Container

```bash
docker run hello-world
```
> This downloads and runs a test image to verify Docker installation.

## Run a Web Server (Nginx) Container

```bash
docker run -d -p 8080:80 nginx
```
Access it at: [http://localhost:8080](http://localhost:8080)

## View Running Containers

```bash
docker ps
```

## Stop the Nginx Container

```bash
docker stop <container_id>
```

## Remove the Nginx Container

```bash
docker rm <container_id>
```

## Clean Up All Stopped Containers and Unused Images

```bash
docker system prune
```

---

# 7. Quick Tips

- Always check running containers:

  ```bash
  docker ps
  ```

- View all containers (even exited ones):

  ```bash
  docker ps -a
  ```

- Stop containers you don't need:

  ```bash
  docker stop <container_id>
  ```

- Remove unused containers:

  ```bash
  docker rm <container_id>
  ```

- Save space by cleaning up:

  ```bash
  docker system prune
  ```

---

