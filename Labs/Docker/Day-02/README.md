# Docker Day 02 — Images & Dockerfiles

## Overview

Day 02 focused on understanding **Docker images and Dockerfiles** and how Dockerfiles are used to create reproducible application environments.

I practiced building custom Docker images, running containers from those images, copying files into images, setting working directories, defining default commands, exposing application ports, and publishing container ports to the host.

---

## Objectives

By the end of this session, I practiced:

* Understanding the purpose of a Dockerfile
* Creating a custom Docker image
* Using `FROM`
* Using `RUN`
* Using `COPY`
* Using `WORKDIR`
* Using `CMD`
* Using `EXPOSE`
* Building images with `docker build`
* Running containers with `docker run`
* Publishing container ports using `-p`
* Running a Python web server inside a container
* Understanding host-to-container port mapping
* Troubleshooting Docker build context issues

---

# 1. Dockerfile Basics

A **Dockerfile** is a text file containing instructions used by Docker to build a container image.

Basic flow:

```text
Dockerfile
    ↓
docker build
    ↓
Docker Image
    ↓
docker run
    ↓
Container
```

---

# 2. First Dockerfile

Created my first Dockerfile:

```dockerfile
FROM ubuntu

CMD ["echo", "Hello from my first Docker image"]
```

Built the image:

```bash
docker build -t my-first-image .
```

Ran a container from the image:

```bash
docker run --name first-container my-first-image
```

Output:

```text
Hello from my first Docker image
```

### What I learned

* `FROM` specifies the base image.
* `CMD` specifies the default command executed when the container starts.
* `docker build` creates an image from a Dockerfile.
* `docker run` creates and starts a container from an image.

---

# 3. RUN vs CMD

Created a Dockerfile to understand the difference between `RUN` and `CMD`:

```dockerfile
FROM ubuntu

RUN echo "RUN executed during image build"

CMD ["echo", "CMD executed when container starts"]
```

Built the image:

```bash
docker build -t run-cmd-demo .
```

Ran the container:

```bash
docker run --name run-cmd-container run-cmd-demo
```

Output:

```text
CMD executed when container starts
```

### Key Difference

| Instruction | When it executes      | Purpose                                       |
| ----------- | --------------------- | --------------------------------------------- |
| `RUN`       | During image build    | Executes commands while creating the image    |
| `CMD`       | When container starts | Defines the default command for the container |

### Interview Answer

> `RUN` executes commands during the Docker image build process, while `CMD` defines the default command that runs when a container starts.

---

# 4. COPY Instruction

Practiced copying application files into a Docker image.

Created an `app.txt` file:

```text
Hello from my Docker application
```

Dockerfile:

```dockerfile
FROM ubuntu

COPY app.txt /app.txt

CMD ["cat", "/app.txt"]
```

Built the image:

```bash
docker build -t copy-demo .
```

Ran the container:

```bash
docker run --name copy-container copy-demo
```

Output:

```text
Hello from my Docker application
```

### What I learned

`COPY` copies files or directories from the Docker build context into the Docker image.

Example:

```dockerfile
COPY app.txt /app.txt
```

means:

```text
Local build context
      ↓
    app.txt
      ↓
Docker image
      ↓
   /app.txt
```

---

# 5. WORKDIR Instruction

Practiced setting the working directory inside the container.

Dockerfile:

```dockerfile
FROM ubuntu

WORKDIR /app

COPY app.txt .

CMD ["cat", "app.txt"]
```

Built the image:

```bash
docker build -t workdir-demo .
```

Ran the container:

```bash
docker run --name workdir-container workdir-demo
```

Output:

```text
Hello from my Docker application
```

Verified the working directory:

```bash
docker run --rm workdir-demo pwd
```

Output:

```text
/app
```

### What I learned

`WORKDIR` sets the working directory for subsequent Dockerfile instructions and for the container's default working directory.

Instead of using:

```dockerfile
COPY app.txt /app/app.txt
```

I can use:

```dockerfile
WORKDIR /app
COPY app.txt .
```

This makes Dockerfiles cleaner and easier to maintain.

### Interview Answer

> `WORKDIR` sets the working directory inside the image/container and affects subsequent Dockerfile instructions such as `RUN`, `COPY`, and `CMD`.

---

# 6. EXPOSE and Port Publishing

For this task, I created a Python-based web server container.

Dockerfile:

```dockerfile
FROM python:3.12-slim

WORKDIR /app

COPY app.txt .

EXPOSE 8000

CMD ["python", "-m", "http.server", "8000"]
```

Built the image:

```bash
docker build -t python-web-demo .
```

The build downloaded the Python base image and created the custom image.

---

## Running the Web Server

Initially ran the container directly:

```bash
docker run --name python-web-container python-web-demo
```

The container displayed:

```text
Serving HTTP on 0.0.0.0 port 8000
```

I stopped the foreground container using:

```text
Ctrl + C
```

This stopped the running process inside the container.

---

# 7. Publishing the Container Port

Started the container in detached mode and published the port:

```bash
docker run -d --name python-web-container2 -p 8000:8000 python-web-demo
```

Verified the running container:

```bash
docker
```
