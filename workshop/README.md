# Worksop Steps

## Prerequisites
- Install [Docker Desktop](https://www.docker.com/products/docker-desktop/) or [Docker Engine](https://docs.docker.com/engine/install/) and [Minikube](https://minikube.sigs.k8s.io/docs/start/)
- Install JetBrains [Rider](https://www.jetbrains.com/rider/), [VS 2022](https://visualstudio.microsoft.com/vs/), or [VS Code](https://code.visualstudio.com/)
- Install [NodeJs](https://nodejs.org/)
- Clone this repository
- Set up env variables in .zshrc file on Mac. On Windows use either of these two guides: [1](http://www.dowdandassociates.com/blog/content/howto-set-an-environment-variable-in-windows-command-line-and-registry/), [2](https://docs.oracle.com/en/database/oracle/machine-learning/oml4r/1.5.1/oread/creating-and-modifying-environment-variables-on-windows.html) 
```
# .zshrc on mac
# use host.docker.internal instead of docker.for.mac.localhost for windows
export COMPOSE_PROJECT_NAME=eshop
export ESHOP_EXTERNAL_DNS_NAME_OR_IP=docker.for.mac.localhost
export ESHOP_STORAGE_CATALOG_URL=http://docker.for.mac.localhost:5202/c/api/v1/catalog/items/[0]/pic/
export ESHOP_AZURE_SERVICE_BUS=docker.for.mac.localhost

export ESHOP_AZURE_CATALOG_DB="Server=docker.for.mac.localhost,5433;Database=Microsoft.eShopOnContainers.Service.CatalogDb;User Id=sa;Password=Pass@word"
export ESHOP_AZURE_IDENTITY_DB="Server=docker.for.mac.localhost,5433;Database=Microsoft.eShopOnContainers.Service.IdentityDb;User Id=sa;Password=Pass@word"
export ESHOP_AZURE_ORDERING_DB="Server=docker.for.mac.localhost,5433;Database=Microsoft.eShopOnContainers.Services.OrderingDb;User Id=sa;Password=Pass@word"
export ESHOP_AZURE_WEBHOOKS_DB="Server=docker.for.mac.localhost,5433;Database=Microsoft.eShopOnContainers.Services.WebhooksDb;User Id=sa;Password=Pass@word"
```
- Follow the [slides](https://docs.google.com/presentation/d/1Rg07RdJfuUJ4KqJxtTXw7TEw9VZEQuAZ0z4pPWb5NmU/edit?usp=sharing) and steps from this document along with the workshop presenter 

## 1. Run all services using Rider Compound Configuration
- Skip this step if you have VS 2022 or VS Code
- The compound run configuration is avaialble in `src/.run/All.run.xml`

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
❌ How do I run my setup on multiple machines? → ✅ Kubernetes (K8S)<br/>
❌ How do I create K8S config? → ✅ [Kompose](http://kompose.io)

## 4. Convert docker-compose.yml to k8s .yaml files using [Kompose](https://kompose.io/)
- <mark>make sure that env variables from the Prerequisites section are initialised. Kompose doesn't support .env file</mark>
- `kompose -f docker-compose.yml -f docker-compose.override.yml -o k8s --volumes hostPath convert`
- `update volume configs: eshop-sqldata, eshop-nosqldata, eshop-basketdata`
- `cd k8s`
- `kubectl apply -f .`
- `fix env var for a health-check and redeploy`

### Issues
❌ How do I open ports? → ✅ [Proxy / Node Ports / Load Balancer / Ingress](https://medium.com/google-cloud/kubernetes-nodeport-vs-loadbalancer-vs-ingress-when-should-i-use-what-922f010849e0)

## 5. Configure Nginx Ingres and use Helm
- Follow [Deploy to local K8S](https://github.com/dotnet-architecture/eShopOnContainers/wiki/Deploy-to-Local-Kubernetes#Install-NGINX-Ingress-Controller)