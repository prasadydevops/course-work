- Remove existing docker images

```
docker rm -f lms-web lms-public-api
```

- Run `docker-compose up` to start the containers, this will rebuild the images as we removed the existing ones.

- Start minikube

```
minikube start
```

- Load the docker images into minikube

```
minikube image load lms-web lms-public-api
```

- Checkout to `kube2` branch

```
git checkout kube2
```

- Create the deployment and service

```
kubectl apply -f k8s
```

- Get Service access URLs

```
minikube service --all
```

- Access the application using the URL from the above command

## Dashboard

- Start the dashboard

```
minikube dashboard
```

- Access the dashboard using the URL from the above command

- Login into the pod running postgres using dashboard UI.

- Run the following command to access the postgres database and verify the data

```
su - postgres;

psql;

\d;

\c postgres;

SELECT * FROM  "Public";
```
