# Docker Day 05 — Node.js Application

## Objective
Build and run a Node.js application in Docker, then apply basic container improvements.

## Application
- Node.js HTTP server
- Listens on port 3000
- Returns a plain-text response

## Dockerfile Concepts
- `FROM`: Uses the Node.js Alpine base image.
- `WORKDIR`: Sets `/app` as the working directory.
- `COPY`: Copies the application files into the image.
- `EXPOSE`: Documents port 3000.
- `USER`: Runs the application as the non-root `node` user.
- `HEALTHCHECK`: Checks whether the application responds successfully.
- `CMD`: Starts the application using `npm start`.

## .dockerignore
Excludes:
- `node_modules`
- `npm-debug.log`
- `.git`
- `.env`

## Build and Run

```powershell
docker build -t devops-node-app:v4 .
docker run -d --name node-app -p 3000:3000 devops-node-app:v4
