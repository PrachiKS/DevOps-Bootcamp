# Docker Day 03 — Image Management & Optimization

## Overview

This lab covers Docker image management, tagging, inspection, layers, cleanup, disk usage, `.dockerignore`, and image-size optimization.

## Tasks Completed

### 1. Image Tagging

Created an additional tag for an existing image:

```powershell
docker tag python-web-demo:latest python-web-demo:v1.0
docker images python-web-demo
```

**Learning:** Multiple tags can refer to the same image ID. Creating a tag does not create a separate image copy.

### 2. Inspecting Images

```powershell
docker image inspect python-web-demo:v1.0
```

Inspected image metadata, including its ID, tags, environment variables, exposed ports, command, working directory, architecture, operating system, and layers.

### 3. Understanding Image Layers

```powershell
docker history python-web-demo:v1.0
```

**Learning:** Docker images consist of layers. Base images contribute much of the filesystem, while instructions such as `COPY` can add data. Metadata instructions such as `CMD` and `EXPOSE` generally show no filesystem size in the history output.

### 4. Checking Image Size

```powershell
docker image ls python-web-demo:v1.0
```

Observed the image's disk usage and content size.

**Learning:** Image size can affect storage requirements and the time needed to transfer images.

### 5. Creating a `.dockerignore` File

Created `.dockerignore` with:

```text
.git
.gitignore
*.log
node_modules
```

**Learning:** `.dockerignore` excludes matching files and directories from the Docker build context.

### 6. Testing `.dockerignore`

Created a Dockerfile that copies the build context:

```dockerfile
FROM ubuntu

WORKDIR /app

COPY . .

CMD ["ls", "-la", "/app"]
```

Built and ran the image:

```powershell
docker build -t dockerignore-demo .
docker run --rm dockerignore-demo
```

**Result:** `debug.log` was excluded from the copied files, demonstrating that the `.dockerignore` rule worked.

### 7. Checking Dangling Images

```powershell
docker images --filter "dangling=true"
```

**Learning:** Dangling images are untagged images, commonly displayed as `<none>:<none>`. No dangling images were listed during this check.

### 8. Listing Containers and Images

```powershell
docker ps -a
docker image ls
```

Reviewed existing containers and images, including stopped containers and images that remained available.

### 9. Inspecting a Container's Source Image

```powershell
docker inspect --format='{{.Config.Image}}' python-web-container2
docker inspect --format='{{.Config.Image}}' workdir-container
```

**Learning:** `docker inspect` can reveal which image was used to create a container.

### 10. Tagging the `.dockerignore` Image

```powershell
docker tag dockerignore-demo:latest dockerignore-demo:v1.0
docker images dockerignore-demo
```

**Learning:** Both tags initially pointed to the same image ID.

### 11. Removing an Image Tag

```powershell
docker rmi dockerignore-demo:v1.0
docker images dockerignore-demo
```

**Learning:** Removing one tag does not necessarily remove the underlying image if another tag still references it.

### 12. Removing an Image

Checked for containers created from the image:

```powershell
docker ps -a --filter "ancestor=dockerignore-demo"
```

Then removed it:

```powershell
docker rmi dockerignore-demo:latest
docker images dockerignore-demo
```

**Result:** The image was removed after confirming no containers were using it.

### 13. Reviewing Unused Images

```powershell
docker images --filter "dangling=true"
docker image ls
```

Reviewed the difference between dangling images and tagged images that may be unused.

### 14. Checking Docker Disk Usage

```powershell
docker system df
docker system df -v
```

**Learning:** These commands report Docker's storage usage for images, containers, volumes, and build cache. The verbose option provides a more detailed breakdown.

### 15. Removing a Stopped Container

```powershell
docker rm festive_moore
docker ps -a --filter "name=festive_moore"
```

**Result:** The selected stopped container was removed.

**Learning:** `docker rm` removes a container; `docker rmi` removes an image.

### 16. Comparing Containers and Images

```powershell
docker ps -a
docker image ls
docker system df
```

Verified that removing a container does not automatically remove its image.

### 17. Removing an Unused Image

```powershell
docker rmi hello-world:latest
docker image ls
docker system df
```

**Result:** The unused `hello-world` image was removed.

### 18. Building an Alpine Image

Created `Dockerfile.alpine`:

```dockerfile
FROM alpine:latest

WORKDIR /app

COPY app.txt .

CMD ["cat", "app.txt"]
```

Built and tested it:

```powershell
docker build -f Dockerfile.alpine -t alpine-demo .
docker run --rm alpine-demo
```

**Result:** The container printed `Application file`.

### 19. Comparing Image Layers

```powershell
docker history alpine-demo
docker history python-web-demo:latest
```

**Learning:** Different base images and installed software contribute different amounts of data to the image.

### 20. Building a Python Alpine Image

Created `Dockerfile.python-alpine`:

```dockerfile
FROM python:3.12-alpine

WORKDIR /app

COPY app.txt .

EXPOSE 8000

CMD ["python", "-m", "http.server", "8000"]
```

Built the image:

```powershell
docker build -f Dockerfile.python-alpine -t python-alpine-demo .
```

Compared image sizes:

| Image                | Disk Usage | Content Size |
| -------------------- | ---------: | -----------: |
| `python-web-demo`    |     188 MB |      46.2 MB |
| `python-alpine-demo` |    74.1 MB |        18 MB |

Started and stopped a test container:

```powershell
docker run --rm -d --name python-alpine-test -p 8001:8000 python-alpine-demo
docker stop python-alpine-test
```

**Learning:** Choosing a smaller base image can reduce image size. Compatibility, dependencies, and maintenance also matter when selecting a base image.

### 21. Verifying Automatic Container Removal

```powershell
docker ps -a --filter "name=python-alpine-test"
docker image ls python-alpine-demo
```

**Result:** The test container was absent from the container list, while the image remained available.

**Learning:** `--rm` automatically removes the container after it exits; it does not remove the image.

## Important Commands Practiced

| Command                                  | Purpose                                         |
| ---------------------------------------- | ----------------------------------------------- |
| `docker tag`                             | Add another tag to an image                     |
| `docker image inspect`                   | View image metadata                             |
| `docker history`                         | Inspect image history and layers                |
| `docker images`                          | List images                                     |
| `docker rmi`                             | Remove an image or tag                          |
| `docker rm`                              | Remove a container                              |
| `docker ps -a`                           | List containers, including stopped ones         |
| `docker system df`                       | Review Docker disk usage                        |
| `docker images --filter "dangling=true"` | Find dangling images                            |
| `docker run --rm`                        | Remove a container automatically after it exits |

## Key Takeaways

* Tags identify image versions and can point to the same image ID.
* Image history helps explain where image data comes from.
* `.dockerignore` prevents selected files from entering the build context.
* Container cleanup and image cleanup are separate operations.
* `docker system df` helps review resource usage before cleanup.
* A smaller base image can reduce image size, but should still meet the application's requirements.

## Real-World DevOps Application

In a CI/CD workflow, engineers build and tag images, inspect them when troubleshooting, exclude unnecessary files with `.dockerignore`, choose suitable base images, and remove resources that are no longer needed.

## Day 03 Status

**Completed:** Image tagging, inspection, layer exploration, `.dockerignore`, cleanup, disk-usage review, and base-image comparison.
