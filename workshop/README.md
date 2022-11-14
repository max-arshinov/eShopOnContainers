# Worksop Steps

## Prerequisites

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
- ❌ How do I run my setup on multiple machines? → ✅ Kubernetes (K8S)
- ❌ How do I create K8S config? → ✅ [Kompose](http://kompose.io)

## 4. Convert docker-compose.yml to k8s .yaml files using [Kompose](https://kompose.io/)
- `docker-compose config > docker-compose-resolved.yml && kompose -f docker-compose-resolved.yml -f docker-compose.override.yml -o k8s convert`
- `cd k8s`
- `kubectl apply -f .`

### Issues
❌ How do I open ports? → ✅ Ingress

## 5. Configure Nginx Ingres
- Uncomment labels in docker-compose.yml
- `kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.1.1/deploy/static/provider/cloud/deploy.yaml` |
- Update `etc/hosts` files if needed