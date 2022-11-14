## Docker

| Action      | Command                                                                                                                                                                                                         |
|-------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| DB          | `docker pull mcr.microsoft.com/azure-sql-edge`<br/>`docker run --cap-add SYS_PTRACE -e 'ACCEPT_EULA=1' -e 'MSSQL_SA_PASSWORD=Pass@word' -p 5433:1433 --name eshop-sql-data -d mcr.microsoft.com/azure-sql-edge` |
| Rabbit      | `docker pull rabbitmq`<br/>`docker run -p 15672:15672 -p 5672:5672 --name rabbitmq -d eshop-rabbitmq:3-management`                                                                                              |
| API Gateway | `docker run -d --add-host host.docker.internal:host-gateway -v $(pwd)/ApiGateways/Envoy/config/webshopping-local:/etc/envoy -p 5202:80 -p 15202:8001 --name eshop-webshoppingapigw envoyproxy/envoy:v1.11.1`    |

## K8S
| Action                                          | Command                                                                                                                                       |
|-------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------|
| Convert docker-compose to k8s .yaml files       | `docker-compose config > docker-compose-resolved.yml && kompose -f docker-compose-resolved.yml -f docker-compose.override.yml -o k8s convert` |
| Apply all yaml files from the current directory | `kubectl apply -f .`                                                                                                                          |
| Install the NGINX Ingress controller            | `kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.1.1/deploy/static/provider/cloud/deploy.yaml`      |
| To check the public service exposed             | `kubectl get ing`                                                                                                                             |
| Check Deployment Status                         | `kubectl get deployment  `                                                                                                                    |

docker-compose config > docker-compose-resolved.yaml && kompose convert -f docker-compose-resolved.yaml
kubectl delete all  --all -n ingress-nginx

# Helm
| Action                              | Command                                                                                              |
|-------------------------------------|------------------------------------------------------------------------------------------------------|
| Deploy Public Images From DockerHub | `pwsh ./deploy-all-mac.ps1 -useLocalk8s:$true`                                                       |
| Unisntall all eshop- helm charts    | `helm uninstall $(helm ls --filter eshop -q), helm uninstall $(helm ls --filter eshop -q) --dry-run` |
