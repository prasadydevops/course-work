- stop api load balancer service and replace it with node port (Load balancer costs money where node port not)
- create ingress service for exposing web to outside cluster
- create daemonset for logging
- create secret file for postgres password
- create configmap for postgres connection

- enable ingress addon

```bash
minikube addons enable ingress
```

- get ingress ip

```bash
minikube get ingress
# minikube ip
```
