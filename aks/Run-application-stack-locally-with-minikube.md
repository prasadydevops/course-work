# Run Application Stack Locally with Minikube

- Create a VM with minimum memory 4GB
- Install Docker and Run the Docker daemon as a non-root user (Rootless mode)
  - [Run the Docker daemon as a non-root user (Rootless mode) | Docker Documentation](https://docs.docker.com/engine/security/rootless/)
- Install minikube and kubectl
  - [minikube start | minikube](https://minikube.sigs.k8s.io/docs/start/)
  - [Install Tools | Kubernetes](https://kubernetes.io/docs/tasks/tools/)
- Clone the lms-public repo
- Create a minikube cluster

```bash
# Add the current user to the docker group if not already added before running below command
minikube start --driver docker --container-runtime=containerd
```

- Install kompose tool
- Kompose convert docker-compose.yml to kubernetes manifests

```bash
wget https://github.com/kubernetes/kompose/releases/download/v1.26.1/kompose_1.26.1_amd64.deb # Replace 1.26.1 with latest tag
sudo apt install ./kompose_1.26.1_amd64.deb
```

- Generate kubernetes manifests

```bash
kompose convert -f docker-compose.yml
```

- Move all the generated manifests to the kubernetes folder

```bash
mv *.yaml kubernetes/
```

- Run docker-compose up, this will build the images required for the application stack

```bash
docker-compose up
```

- Load the images into minikube

```bash
minikube image load <image-name>
```

- View the images in minikube

```bash
minikube image ls
```

- Deploy the application stack

```bash
kubectl apply -f kubernetes/
```

- Check the status of the pods

```bash
kubectl get pods # use --watch to watch the status of the pods
```

- Check the status of the services

```bash
kubectl get services
```

- View service endpoints

```bash
minikube service list
```

- Verify the application stack is running

```bash
curl http://<service-endpoint>
```

- View the logs of the pods

```bash
kubectl logs <pod-name>
```

<!-- https://github.com/kubernetes/minikube/issues/11530#issuecomment-850090790 -->
