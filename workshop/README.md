# Worksop Steps

## Prerequisites
- Install [Docker Desktop](https://www.docker.com/products/docker-desktop/) or [Docker Engine](https://docs.docker.com/engine/install/) and [Minikube](https://minikube.sigs.k8s.io/docs/start/)
- Install JetBrains [Rider](https://www.jetbrains.com/rider/), [VS 2022](https://visualstudio.microsoft.com/vs/), or [VS Code](https://code.visualstudio.com/)
- Install [NodeJs](https://nodejs.org/)
- Clone this repository
- Follow the [slides](https://docs.google.com/presentation/d/1Rg07RdJfuUJ4KqJxtTXw7TEw9VZEQuAZ0z4pPWb5NmU/edit?usp=sharing) and steps from this document along with the workshop presenter 

## 1. Run all services using Rider Compound Configuration
The compound run configuration is avaialble in `src/.run/All.run.xml`

### Issues
❌ Dependencies are not installed → ✅ Run them in docker

## 2. Run Redis, DB, and API Gateway in Docker
### DB
- `docker pull mcr.microsoft.com/azure-sql-edge`
- `docker run --cap-add SYS_PTRACE -e 'ACCEPT_EULA=1' -e 'MSSQL_SA_PASSWORD=Pass@word' -p 5433:1433 --name eshop-sql-data -d mcr.microsoft.com/azure-sql-edge`

### RabbitMQ
- `docker pull rabbitmq`
- `docker run -p 15672:15672 -p 5672:5672 --name rabbitmq -d eshop-rabbitmq:3-management`

### Envoy API Gateway
- `docker run -d --add-host host.docker.internal:host-gateway -v $(pwd)/ApiGateways/Envoy/config/webshopping-local:/etc/envoy -p 5202:80 -p 15202:8001 --name eshop-webshoppingapigw envoyproxy/envoy:v1.11.1`

### Issues
❌ Where do I put all these scripts? → ✅ docker-compose.yml

## 3. Run [docker compose](https://docs.docker.com/compose/gettingstarted/)
- Check `src/.env file` for OS-specific settings
- docker compose up -d
- Debug local containers using [Rider](https://blog.jetbrains.com/dotnet/2018/07/18/debugging-asp-net-core-apps-local-docker-container/) / [Visual Studio](https://learn.microsoft.com/en-us/visualstudio/containers/edit-and-refresh?view=vs-2022)

### Issues
❌ How do I run my setup on multiple machines? → ✅ Kubernetes (K8S)
❌ How do I create K8S config? → ✅ [Kompose](http://kompose.io)

## 4. Convert docker-compose.yml to k8s .yaml files using [Kompose](https://kompose.io/)
- `kompose -f docker-compose.yml -f docker-compose.override.yml -o k8s --volumes hostPath convert`
- `update volume configs: eshop-sqldata, eshop-nosqldata, eshop-basketdata`
- `cd k8s`
- `kubectl apply -f .`
- `fix env var for a health-check and redeploy`

### Issues
❌ How do I open ports? → ✅ Proxy / Node Ports / Load Balancer / Ingress

## 5. Configure Nginx Ingres
- Follow [Deploy to local K8S](https://github.com/dotnet-architecture/eShopOnContainers/wiki/Deploy-to-Local-Kubernetes#Install-NGINX-Ingress-Controller)