k8s

Kubernetes is a container orchestration platform used to deploy and manage containers.
A pod is the smallest unit in Kubernetes that runs one or more containers.
To restart a pod: kubectl rollout restart deployment <name>
To check pod logs: kubectl logs <pod-name>
To check previous pod logs: kubectl logs <pod-name> --previous
To describe a pod and see events: kubectl describe pod <pod-name>
To check pod status: kubectl get pods
To check all pods in all namespaces: kubectl get pods --all-namespaces
To delete a pod and let it restart: kubectl delete pod <pod-name>
CrashLoopBackOff means the container keeps crashing and restarting - always check logs first.
OOMKilled means the container ran out of memory - increase memory limits in the deployment.
ImagePullBackOff means Kubernetes cannot pull the Docker image - check image name and registry credentials.
Pending pod means no node has enough resources - check node capacity with kubectl get nodes.
To scale a deployment: kubectl scale deployment <name> --replicas=3
To check deployment status: kubectl rollout status deployment <name>
To undo a bad deployment: kubectl rollout undo deployment <name>
To get all services: kubectl get services
To expose a deployment: kubectl expose deployment <name> --port=80 --type=LoadBalancer
A namespace is a way to isolate resources in a cluster.
To create a namespace: kubectl create namespace <name>
To set a context namespace: kubectl config set-context --current --namespace=<name>
ConfigMap stores non-secret configuration data as key-value pairs.
Secret stores sensitive data like passwords and API keys in base64 encoded format.
A liveness probe checks if a container is running.
A readiness probe checks if a container is ready to serve traffic.
Resource limits prevent a container from using too much CPU or memory.
Horizontal Pod Autoscaler scales pods based on CPU or memory usage automatically.

docker
Docker is a containerisation platform that packages apps into containers.
A container is a lightweight isolated environment that runs an application.
A Docker image is a blueprint for creating containers.
Dockerfile is a text file with instructions to build a Docker image.
To build an image: docker build -t myapp .
To build with a specific tag: docker build -t myapp:v1 .
To run a container: docker run -p 8000:8000 myapp
To run in background: docker run -d -p 8000:8000 myapp
To list running containers: docker ps
To list all containers including stopped: docker ps -a
To stop a container: docker stop <container-id>
To remove a container: docker rm <container-id>
To view container logs: docker logs <container-id>
To follow logs in real time: docker logs -f <container-id>
To enter a running container: docker exec -it <container-id> bash
To list Docker images: docker images
To remove an image: docker rmi <image-id>
To push image to Docker Hub: docker push username/imagename
To pull image from Docker Hub: docker pull username/imagename
Docker Compose runs multiple containers together using a yml file.
To start all services: docker compose up --build
To start in background: docker compose up -d
To stop all services: docker compose down
To view compose logs: docker compose logs
A volume persists data even when a container is stopped or removed.
To create a volume: docker volume create myvolume
Environment variables pass configuration to containers using -e flag or .env file.
Never put secrets or API keys inside a Dockerfile - use environment variables.
A multi-stage build reduces final image size by separating build and runtime stages.
To check Docker disk usage: docker system df
To clean up unused resources: docker system prune


ci/cd

CI/CD stands for Continuous Integration and Continuous Deployment.
CI automatically builds and tests code on every git push.
CD automatically deploys code to production after tests pass.
GitHub Actions is a CI/CD tool built into GitHub.
A workflow file is stored at .github/workflows/deploy.yml
A workflow is triggered by events like push, pull request, or schedule.
A job is a set of steps that run on the same runner machine.
A step is a single command or action inside a job.
To build a Docker image in GitHub Actions use the docker build command.
To push to Docker Hub store credentials as GitHub Secrets.
GitHub Secrets store sensitive values like API keys and passwords securely.
Never hardcode passwords or API keys in workflow files.
A pull request triggers a workflow to test code before merging.
Branch protection rules prevent merging without passing CI checks.
Jenkins is an open source CI/CD automation server.
A Jenkinsfile defines pipeline stages like build test and deploy.
Pipeline stages in Jenkins are build, test, push, deploy.
ArgoCD is a GitOps tool that automatically syncs Kubernetes with GitHub.
A rolling deployment updates containers gradually with zero downtime.
A blue green deployment runs two identical environments and switches traffic.
A canary deployment releases to a small percentage of users first.
Health check endpoint returns 200 OK when app is running correctly.
Artifacts are files produced by a build like Docker images or test reports.
A registry stores Docker images - examples are Docker Hub, ECR, and GCR.
To trigger a deploy automatically add a deploy step after the push step.
Environment variables in CI/CD pipelines should always come from secrets.
A failed pipeline stops deployment so broken code never reaches production.


linux 

Linux is an open source operating system widely used in DevOps.
Ubuntu is a popular Linux distribution used for servers.
To update packages: sudo apt update && sudo apt upgrade
To install a package: sudo apt install <package-name>
To check running processes: ps aux
To kill a process: kill <pid>
To check disk usage: df -h
To check memory usage: free -h
To check CPU usage: top or htop
To view file contents: cat <filename>
To edit a file: nano <filename> or vim <filename>
To check open ports: netstat -tulpn
To check system logs: journalctl -xe
To start a service: sudo systemctl start <service>
To stop a service: sudo systemctl stop <service>
To check service status: sudo systemctl status <service>
To enable service on startup: sudo systemctl enable <service>
File permissions: chmod 755 gives owner full access and others read execute.
To change file owner: chown user:group <filename>
SSH connects securely to remote servers: ssh user@ip
To copy files to remote server: scp file.txt user@ip:/path
Environment variables: export KEY=value sets a variable for current session.
To make permanent add export to ~/.bashrc file.

azure 

Azure is Microsoft's cloud computing platform.
Azure App Service hosts web applications and APIs without managing servers.
Azure Container Registry stores Docker images similar to Docker Hub.
Azure Kubernetes Service is a managed Kubernetes service on Azure called AKS.
Azure DevOps is a CI/CD platform with pipelines repos and boards.
Azure Blob Storage stores files images and backups similar to AWS S3.
Azure Virtual Machine is a cloud server you can SSH into.
Azure Container Instances runs Docker containers without Kubernetes.
Azure Resource Group is a logical container that holds related Azure resources.
To login to Azure CLI: az login
To list subscriptions: az account list
To set subscription: az account set --subscription <id>
To create a resource group: az group create --name mygroup --location eastus
To list resource groups: az group list
To delete a resource group: az group delete --name mygroup
To create a virtual machine: az vm create --resource-group mygroup --name myvm --image Ubuntu2204
To connect to Azure VM: ssh azureuser@<public-ip>
To start a VM: az vm start --resource-group mygroup --name myvm
To stop a VM: az vm stop --resource-group mygroup --name myvm
To create Azure Container Registry: az acr create --name myregistry --resource-group mygroup --sku Basic
To login to container registry: az acr login --name myregistry
To build and push image to ACR: az acr build --registry myregistry --image infrachat:latest .
To list images in ACR: az acr repository list --name myregistry
To create Azure App Service plan: az appservice plan create --name myplan --resource-group mygroup --sku B1 --is-linux
To deploy container to App Service: az webapp create --resource-group mygroup --plan myplan --name infrachat --deployment-container-image-name myregistry.azurecr.io/infrachat:latest
To check App Service logs: az webapp log tail --name infrachat --resource-group mygroup
To deploy AKS cluster: az aks create --resource-group mygroup --name myaks --node-count 1 --generate-ssh-keys
To get AKS credentials: az aks get-credentials --resource-group mygroup --name myaks
To connect ACR to AKS: az aks update --name myaks --resource-group mygroup --attach-acr myregistry
Azure Monitor collects logs and metrics from all Azure services.
Azure Key Vault stores secrets API keys and certificates securely.
To set a secret in Key Vault: az keyvault secret set --vault-name myvault --name mykey --value myvalue
To get a secret from Key Vault: az keyvault secret show --vault-name myvault --name mykey
Azure Active Directory manages identity and access.
Azure Load Balancer distributes traffic across multiple virtual machines.
Azure free tier gives 750 hours of B1S virtual machine per month for 12 months.
Azure CLI is installed with: curl -sL https://aka.ms/InstallAzureCLIDeb | sudo bash
To check Azure CLI version: az --version
Azure DevOps pipeline is defined in azure-pipelines.yml file.
A stage in Azure DevOps pipeline contains jobs and steps.
Azure Artifacts stores packages like npm pip and Maven.
Azure Boards manages work items sprints and backlogs.
To create Azure DevOps pipeline: go to dev.azure.com and connect GitHub repo.
Azure Container Apps runs microservices and containerised apps serverlessly.
Azure Front Door is a global load balancer and CDN for web apps.
Azure DNS manages domain names and routing.
A managed identity lets Azure services authenticate without storing credentials.
