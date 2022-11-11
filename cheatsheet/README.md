| Action                                          | Command                                                                                                                                  |
|-------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------|
| aaa                                             |
| Convert docker-compose to k8s .yaml files       | `kompose convert -f docker-compose.yaml`                                                                                                 |
| Apply all yaml files from the current directory | `kubectl apply -f .`                                                                                                                     |
| Install the NGINX Ingress controller            | `kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.1.1/deploy/static/provider/cloud/deploy.yaml` |
| To check the public service exposed             | `kubectl get ing`                                                                                                                        |
| Check Deployment Status                         | `kubectl get deployment  `                                                                                                               |
|                                                 |                                                                                                                                          |
| Deploy Public Images From DockerHub             | `pwsh ./deploy-all-mac.ps1 -imageTag linux-dev -useLocalk8s:$true`                                                                       |
| Unisntall all eshop- helm charts                | `helm uninstall $(helm ls --filter eshop -q), helm uninstall $(helm ls --filter eshop -q) --dry-run`                                     |
