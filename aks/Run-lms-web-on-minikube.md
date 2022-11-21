- Create a VM
- Install docker

  ```bash
  sudo snap install docker --classic

  # Running Docker as normal user
  sudo addgroup --system docker
  sudo adduser $USER docker
  newgrp docker
  sudo snap disable docker
  sudo snap enable docker
  # sudo usermod -aG docker
  ```

- Install kubectl

  ```bash
  sudo snap install kubectl --classic
  ```

- Install minikube

  ```bash
  curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube_latest_amd64.deb
  sudo dpkg -i minikube_latest_amd64.deb
  ```

- Clone repo

  ```bash
  git clone https://github.com/KonaMarsTech/lms-public.git
  ```

- Build docker image

  ```bash
  cd lms-public/webapp
  docker build -t lms-web .
  ```

- Start minikube

  ```bash
  minikube start
  # minikube start --driver=docker --container-runtime=docker
  ```

- Load image to minikube

  ```bash
  minikube image load lms-web
  ```

- Verify image is loaded

  ```bash
  minikube image ls
  ```

- Run image

  ```bash
  kubectl run lms-web --image=lms-web --port=80 --image-pull-policy=Never
  ```

- Verify pod is running

  ```bash
  kubectl get pods
  ```

- Expose service

  ```bash
  kubectl expose pod lms-web --type=LoadBalancer --port=80
  ```

- Verify service is running

  ```bash
  kubectl get services
  ```

- Get minikube ip

  ```bash
  minikube ip
  ```

- Open browser and go to minikube ip

  ```bash
  http://<minikube ip>:<load-balancer-service-port>
  ```

- Check all services running on minikube

  ```bash
  minikube service --all
  ```

- Stop minikube

  ```bash
  minikube stop
  ```

- Delete minikube

  ```bash
  minikube delete
  ```

-
