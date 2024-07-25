# Java Weather App Deployment on Minikube with Docker and Kubernetes

This project demonstrates how to deploy a scalable Java weather app using Docker and Kubernetes on a local Minikube cluster.

## Prerequisites

- Docker installed on your machine
- Minikube installed (for local Kubernetes cluster)
- Docker Hub account with the weather app image uploaded

## Project Setup

### Step 1: Pull the Docker Image

Pull the Docker image of your weather app from Docker Hub:

```sh
docker pull <your-dockerhub-username>/weather-app
```
![image](https://github.com/user-attachments/assets/23c88af1-544c-4e54-be66-c4180a6e25b0)


### Step 2: Start Minikube
Start Minikube to create a local Kubernetes cluster:

```sh
minikube start
```
![image](https://github.com/user-attachments/assets/86cdc06c-ecd6-4ddb-8562-a919507b0407)

### Step 3: Create Kubernetes Deployment and Service Files
Create a directory for your Kubernetes manifests:

```sh
mkdir k8s
cd k8s
```
- 3.1. Deployment YAML
Create a file named deployment.yaml:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: weather-app
spec:
  replicas: 3
  selector:
    matchLabels:
      app: weather-app
  template:
    metadata:
      labels:
        app: weather-app
    spec:
      containers:
      - name: weather-app
        image: <your-dockerhub-username>/weather-app:latest
        ports:
        - containerPort: 8080
```
- 3.2. Service YAML

Create a file named service.yaml:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: weather-app-service
spec:
  type: NodePort
  ports:
  - port: 80
    targetPort: 8080
    nodePort: 30007
  selector:
    app: weather-app
```
    
### Step 4: Deploy to Kubernetes
- 4.1. Apply the Deployment and Service
```sh
kubectl apply -f deployment.yaml
kubectl apply -f service.yaml
```
![image](https://github.com/user-attachments/assets/50b56787-6ef4-49c6-926d-8cf58a6bddf2)

- 4.2. Verify Deployment
Check the status of the pods to ensure they are running:

```sh
kubectl get pods
```
![image](https://github.com/user-attachments/assets/9185de21-7af1-45aa-851d-dcbf4c635ce1)

Check the service to get the NodePort:

```sh
kubectl get services
```

### Step 5: Access the Application
Get the Minikube IP address:

```sh
minikube ip
```

Access the application in your browser using the Minikube IP and the NodePort:

```arduino
http://<minikube-ip>:30007
```
![image](https://github.com/user-attachments/assets/0aacecf0-ce93-4039-a5a9-45b461e32ad9)


### Step 6: Scaling the Application
To scale the application, you can simply update the replicas in the deployment file or use the kubectl scale command:

```sh
kubectl scale deployment weather-app --replicas=5
```
![image](https://github.com/user-attachments/assets/368c8467-79f7-456f-bcab-ee9e4616ec91)

### Step 7: Clean Up
To delete the deployment and service:

```sh
kubectl delete -f deployment.yaml
kubectl delete -f service.yaml
```
![image](https://github.com/user-attachments/assets/846c6ca5-e562-4b25-aa9f-994e26dea23e)
![image](https://github.com/user-attachments/assets/27baabe9-0cfe-476d-b9fa-422c3b163d9f)

## Summary
In this project, you pulled a Docker image for a Java weather app, started a local Kubernetes cluster using Minikube, created Kubernetes deployment and service manifests, deployed the application to Minikube, accessed the application, and scaled it. This provides a basic understanding of deploying and managing a scalable application using Docker and Kubernetes on Minikube.
