# Deploying LMS Backend To AKS

- Launch AKS creation wizard from Azure portal (https://portal.azure.com/#create/microsoft.aks)
- Select cluster preset "Dev/Test"
- Region of your choice
- API server availability 99.5%
- Node size (VM: B2s General purpose)
- Node count (1) to (5)
- Rest keep default

- Create a container registry
- Add docker file to the repo

- Using pipeline template "Deploy to Azure Kubernetes Service"
- Fill the correct values for container registry and path to docker file
- Save the yaml file to the repo

- Open `/manifests/deployment.yml` and add the env variables for the container at `spec.containers[0].env` section.
- Run pipeline to deploy the container to AKS
- Visit AKS and find the external IP for the service
