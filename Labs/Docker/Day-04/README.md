## Day 04 — Docker Storage, Networking, and Compose

### 1. Storage
- Demonstrated that data in a container's writable layer remains while the container exists.
- Created and tested the named volume `devops-data`.
- Verified that volume data persists after removing its writer container.
- Practiced bind mounts to share files between the Windows host and a container.
- Tested read-only bind mounts using `:ro`.

### 2. Networking
- Created a custom bridge network named `devops-network`.
- Connected containers and verified name resolution with `getent hosts`.
- Connected and disconnected a container from the custom network.
- Compared custom bridge network DNS behavior with the default bridge.

### 3. Docker Compose
- Created a Compose application with Nginx and Redis.
- Started the services with `docker compose up -d`.
- Verified Nginx through port 8080 and Redis using `redis-cli ping`.
- Tested service-name DNS resolution between Compose services.
- Inspected logs and validated the configuration with `docker compose config`.
- Removed the Compose stack with `docker compose down`.

### 4. Cleanup
- Removed the networking lab containers and custom network.
- Verified that Docker's standard networks remained.
- Confirmed that the `devops-data` named volume was preserved.

### Skills Practiced
Docker volumes, bind mounts, read-only mounts, bridge networking,
container DNS, Docker Compose, service logs, configuration validation,
and resource cleanup.