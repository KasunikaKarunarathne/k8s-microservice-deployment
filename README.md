# Containerized Microservice Deployment on Kubernetes

This project demonstrates how a simple REST microservice can be containerized using Docker and deployed on a Kubernetes cluster using Minikube. The objective of this mini project is to gain hands-on experience with Kubernetes fundamentals such as Deployments, Services, replica management, and health probes, while understanding how containerized applications are managed in a cluster environment.

The application exposes two endpoints:

- `/` – returns a basic response to verify service accessibility  
- `/health` – returns application health status, used by Kubernetes readiness and liveness probes

---

## Project Objectives

- Containerize a Python Flask microservice using Docker  
- Deploy the containerized service on Kubernetes using Minikube  
- Configure Deployment and Service resources  
- Enable automatic health monitoring using liveness and readiness probes  
- Scale the application using multiple replicas  
- Access the service externally via NodePort  
- Inspect logs and cluster state using kubectl  

This project focuses on core Kubernetes concepts without introducing additional complexity such as ingress controllers or monitoring stacks.

---

## Technologies Used

- Python (Flask)
- Docker
- Kubernetes
- Minikube
- kubectl

---

## Repository Structure

k8s-microservice-deployment/
│
├── app.py # Flask microservice
├── requirements.txt # Python dependencies
├── Dockerfile # Container build configuration
│
├── k8s/
│ ├── deployment.yaml # Kubernetes Deployment manifest
│ └── service.yaml # Kubernetes Service manifest
│
└── README.md


---

## Application Overview

The Flask application provides:

- Root endpoint (`/`) for basic connectivity testing  
- Health endpoint (`/health`) returning JSON status  

The `/health` endpoint is used by Kubernetes to determine container readiness and liveness.

---

## Dockerization

The application is packaged into a Docker image using a lightweight Python base image. The Dockerfile installs dependencies, copies application files, exposes port 5000, and runs the Flask server.

This enables consistent execution across environments and allows Kubernetes to manage the application as a container.

---

## Kubernetes Deployment

The application is deployed using:

### Deployment
- Runs two replicas of the containerized service  
- Uses RollingUpdate strategy  
- Configures readiness and liveness HTTP probes on `/health`  
- Automatically replaces unhealthy pods  

### Service
- NodePort type  
- Exposes the application outside the cluster  
- Routes traffic to all running replicas  

---

## How to Run

### Start Minikube

```bash
minikube start --driver=docker

Build Docker image inside Minikube
eval $(minikube -p minikube docker-env)
docker build -t flask-k8s:1.0 .

Apply Kubernetes manifests
kubectl apply -f k8s/deployment.yaml
kubectl apply -f k8s/service.yaml

Verify pods and service
kubectl get pods
kubectl get svc

Access application
minikube service flask-k8s-svc --url

Outputs and Verification

Two pods running successfully

NodePort service exposing application externally

Continuous /health requests from Kubernetes probes

Deployment reports all replicas available

Logs confirm active health checks

Commands used for verification:

kubectl get pods -o wide
kubectl describe deployment flask-k8s
kubectl logs -l app=flask-k8s
---

Key Learnings

Containerized applications can be managed efficiently using Kubernetes Deployments

Services enable external access and load balancing across replicas

Readiness and liveness probes provide automated health monitoring

Minikube is sufficient for local Kubernetes experimentation

kubectl is essential for debugging and inspecting cluster state

Conclusion

This project provides practical exposure to Kubernetes fundamentals by deploying a real containerized microservice. It demonstrates the complete workflow from application development and Dockerization to Kubernetes deployment, service exposure, scaling, and health monitoring.