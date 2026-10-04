# Docker Day 09 — Image Management & Cleanup

## 🎯 Goal

Learn how to inspect Docker images, identify dangling and unused resources, understand Docker cleanup commands, and safely reclaim Docker build cache.

---

## 1. List Docker Images

```powershell
docker images
docker image ls
```

Both commands display the Docker image inventory.

Important columns:

* `REPOSITORY` → Image name
* `TAG` → Image version/tag
* `IMAGE ID` → Unique image identifier
* `CREATED` → Image creation time
* `SIZE` → Image disk usage

---

## 2. Check Dangling Images

```powershell
docker images --filter "dangling=true"
```

Dangling images are generally untagged images with:

```text
<none>:<none>
```

Result during this lab:

```text
0 dangling images
```

---

## 3. Display Images in a Clean Format

```powershell
docker images --format "table {{.Repository}}\t{{.Tag}}\t{{.ID}}"
```

This displays:

* Repository
* Tag
* Image ID

Multiple tags can point to the same image.

Example:

```text
python-web-demo:latest
python-web-demo:v1.0
```

Both pointed to the same Image ID.

---

## 4. Check Containers and Their Images

```powershell
docker ps -a --format "table {{.Names}}\t{{.Image}}\t{{.Status}}"
```

This shows which containers are using which images.

Important:

> An exited container still exists and can still reference its image.

---

## 5. Check Docker Disk Usage

```powershell
docker system df
```

Before cleanup:

```text
Images          20        10        963.5MB   227MB
Containers      13        2         4.284MB   4.157MB
Local Volumes   1         0         30B       30B
Build Cache     66        0         1.105GB   647.8MB
```

---

## 6. Docker Cleanup Commands

### Remove stopped containers

```powershell
docker container prune
```

### Remove dangling images

```powershell
docker image prune
```

### Remove all unused images

```powershell
docker image prune -a
```

### Remove unused Docker data

```powershell
docker system prune
```

### More aggressive system cleanup

```powershell
docker system prune -a
```

### Include unused volumes

```powershell
docker system prune --volumes
```

⚠️ Always inspect resources before using aggressive prune commands.

---

## 7. Build Cache Cleanup

Check available options:

```powershell
docker builder prune --help
```

Remove unused dangling build cache:

```powershell
docker builder prune
```

More aggressive:

```powershell
docker builder prune -a
```

---

## 8. Actual Cleanup Performed

The build cache was safely cleaned using:

```powershell
docker builder prune
```

Before cleanup:

```text
Build Cache     66        0         1.105GB   647.8MB
```

After cleanup:

```text
Build Cache     38        0         456.7MB   0B
```

Approximately **648 MB of reclaimable build cache** was removed.

Other Docker resources were left untouched.

---

## 9. Final Verification

```powershell
docker ps
```

Final running containers:

```text
hireai-frontend
hireai-backend
```

Both remained running after the cleanup.

Ports:

```text
Frontend → localhost:3000 → container:80
Backend  → localhost:5000 → container:5000
```

---

## 🧠 Interview Quick Notes

```text
docker images
→ List Docker images

docker image ls
→ List Docker images

docker images --filter "dangling=true"
→ Find dangling images

docker ps -a
→ List all containers

docker system df
→ Check Docker disk usage

docker container prune
→ Remove stopped containers

docker image prune
→ Remove dangling images

docker image prune -a
→ Remove unused images

docker builder prune
→ Remove unused build cache

docker system prune
→ Clean unused Docker resources
```

### ⚠️ Safety Rule

Do not blindly run aggressive cleanup commands.

First inspect:

```powershell
docker ps -a
docker images
docker system df
```

Then decide what is safe to remove.