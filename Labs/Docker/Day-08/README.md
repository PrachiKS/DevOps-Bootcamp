# Docker Day 08 - Container Troubleshooting
 ## Objective

Learn how to investigate stopped containers using Docker logs, inspection commands, exit codes, timestamps, and restart policies.

What I Learned
- docker ps -a lists running and stopped containers.
- docker ps -a --filter "status=exited" lists stopped containers.
- docker logs displays application output from a container.
- docker inspect provides detailed container configuration and state.
- Exit code 0 generally indicates successful completion.
- A non-zero exit code indicates an error or abnormal termination, but does not necessarily identify the root cause.
- Exit code 137 commonly indicates termination by SIGKILL.
- OOMKilled=false means Docker did not record an out-of-memory kill.
- Restart policies control whether Docker automatically restarts a container.
- RestartCount reports Docker-managed restart attempts recorded for the container.
Important Commands
# List all containers
docker ps -a

# List stopped containers
docker ps -a --filter "status=exited"

# View recent logs
docker logs --tail 30 node-app

# Inspect exit code and state
docker inspect node-app --format 'ExitCode={{.State.ExitCode}} Error={{.State.Error}} OOMKilled={{.State.OOMKilled}} FinishedAt={{.State.FinishedAt}}'

# Inspect restart policy
docker inspect node-app --format 'RestartPolicy={{.HostConfig.RestartPolicy.Name}} RestartCount={{.RestartCount}}'

# Inspect image and command
docker inspect node-app --format 'Image={{.Config.Image}} Command={{.Config.Cmd}}'

# Inspect lifecycle timestamps
docker inspect node-app --format 'StartedAt={{.State.StartedAt}} FinishedAt={{.State.FinishedAt}} Status={{.State.Status}}'
Troubleshooting Findings
Container: node-app
Image: devops-node-app:v4
Command: npm start
Exit code: 255
OOMKilled: false
Restart policy: no
Restart count: 0
Logs showed that the server started on port 3000.
The exact reason for termination was not established.
Container: python-web-container2
Exit code: 137
OOMKilled: false
Restart policy: no
Logs showed a successful HTTP 200 response and a favicon 404 response.
The exact reason for termination was not established.

Restart Policies

Policy	        Behavior
no	            Never automatically restart.
on-failure	    Restart after a non-zero exit, subject to retry limits if configured.
always	        Automatically restart when the container stops, subject to Docker's restart behavior.
unless-stopped	Restart automatically unless explicitly stopped.

## Real-World Use Case

When an application container stops in production, a DevOps engineer checks its logs, exit code, state, restart policy, and timestamps before investigating the underlying cause.

Key Takeaway

Troubleshoot systematically. Gather evidence before concluding why a container stopped. A restart policy may improve recovery, but it does not fix the underlying application problem.