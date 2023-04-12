# Context

- `docker-compose.yml` specifies a distributed application
- `deploy` folder contains different deployment options based on k8s

# ToDo

- Terraform (or any other similar tool that works best) script that deploys the same distributed application to AWS and
  Azure without K8S
- Consider `docker-compose.yml` vs docker deployment option via Terraform

## AWS

- Try to deploy web apps
  as [serverless](https://aws.amazon.com/blogs/developer/running-serverless-asp-net-core-web-apis-with-amazon-lambda/)
- Try to deploy web apps using [ECS](https://aws.amazon.com/ecs/)
- Update message broker setup to support SNS/SQS
- Replace MongoDB with DynamoDB in a separate branch

## Azure

- Try to deploy web apps as [Azure Web Apps](https://azure.microsoft.com/en-us/products/app-service/web/)
- Explore Azure [container options](https://azure.microsoft.com/en-us/products/category/containers/)
- Replace MongoDB with CosmosDB in a separate branch