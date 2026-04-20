Kubernetes is a container orchestration platform.
To restart a pod: kubectl rollout restart deployment <name>
To check pod logs: kubectl logs <pod-name>
To describe a pod: kubectl describe pod <pod-name>
To check pod status: kubectl get pods
CrashLoopBackOff means the container keeps crashing - check logs.
OOMKilled means the container ran out of memory - increase limits.
To scale a deployment: kubectl scale deployment <name> --replicas=3

Docker is a containerisation platform.
To build an image: docker build -t myapp .
To run a container: docker run -p 8000:8000 myapp
To list containers: docker ps
To stop a container: docker stop <container-id>
To view logs: docker logs <container-id>
Docker Compose runs multiple containers together.
To start compose: docker compose up --build
To stop compose: docker compose down

CI/CD automates building testing and deploying code.
GitHub Actions runs pipelines on every git push.
A pipeline has stages - build, test, push, deploy.
Jenkinsfile defines pipeline stages in Jenkins.
Always store secrets in environment variables not in code.
Docker Hub stores Docker images for deployment.
A health check endpoint confirms the app is running.
Rolling deployment updates containers with zero downtime.
