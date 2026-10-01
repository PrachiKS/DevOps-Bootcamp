# Docker Day 07 — Docker Compose with HireAI

## Objective

Manage the HireAI frontend and backend together using Docker Compose.

## What I Learned

* Docker Compose manages multiple containers using a single YAML file.
* `services` defines the application's containers.
* `build.context` specifies the directory containing each Dockerfile.
* `ports` maps host ports to container ports.
* `env_file` loads backend environment variables from a file.
* `build.args` passes build-time arguments to the frontend image.
* `depends_on` defines a service startup dependency.
* `restart: unless-stopped` configures a restart policy.
* Compose creates a default network for services.
* `docker compose ps` displays service status.
* `docker compose logs` displays application logs.
* `docker compose stop` stops services but retains their containers.
* `docker compose start` starts existing containers.
* `docker compose down` removes Compose containers and networks.

## HireAI Architecture

| Service                   | Container Port | Host Port |
| ------------------------- | -------------: | --------: |
| Backend (Node.js/Express) |           5000 |      5000 |
| Frontend (React/Nginx)    |             80 |      3000 |

MongoDB Atlas is used as the database.

## Important Commands

```powershell
# Validate Compose configuration without printing environment values
docker compose config --quiet

# Build images and start services
docker compose up -d --build

# Check service status
docker compose ps

# View recent backend logs
docker compose logs --tail 20 backend

# Stop one service
docker compose stop frontend

# Restart one service
docker compose start frontend

# Stop all services
docker compose stop

# Restart all existing services
docker compose start

# Remove containers and the Compose network
docker compose down
```

## Verification

* Both containers started successfully.
* Frontend loaded at `http://localhost:3000`.
* Backend responded at `http://localhost:5000` with HTTP status 200.
* Backend logs confirmed a successful MongoDB connection.
* Frontend could be stopped and restarted independently.
* Both services could be stopped and restarted using Compose.

## Real-World Use Case

A developer can run the frontend and backend of a MERN application together with one Compose configuration instead of managing each container separately.

## Key Takeaway

Docker Compose simplifies local multi-container development by defining services, networking, configuration, ports, and lifecycle commands in one file.
