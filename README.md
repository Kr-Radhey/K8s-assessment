# Kubernetes 3-Tier Application Deployment

This project demonstrates the deployment of a containerized 3-tier application on a local Kubernetes cluster using **Kind**. The application consists of a Frontend (Nginx), Backend (Node.js), and MongoDB database, following Kubernetes best practices.

## Architecture

```
Browser
   │
kubectl port-forward
   │
Frontend Service (ClusterIP)
   │
Frontend Deployment (Nginx)
   │
Backend Service (ClusterIP)
   │
Backend Deployment (Node.js)
   │
MongoDB Headless Service
   │
MongoDB StatefulSet
```

## Tech Stack

- Kubernetes (Kind)
- Docker
- Node.js
- Nginx
- MongoDB

## Kubernetes Resources

- Namespace
- ConfigMap
- Secret
- Deployment (Frontend & Backend)
- StatefulSet (MongoDB)
- ClusterIP Services
- Headless Service
- Resource Requests & Limits
- Startup, Readiness & Liveness Probes

## Project Structure

```
k8s/
├── backend/
│   ├── deployment.yaml
│   └── service.yaml
├── frontend/
│   ├── deployment.yaml
│   └── service.yaml
├── mongodb/
│   ├── statefulset.yaml
│   └── service.yaml
├── config/
│   ├── configmap.yaml
│   └── secret.yaml
└── namespace.yaml
```

## Deployment

Create the namespace:

```bash
kubectl apply -f k8s/namespace.yaml
```

Deploy configuration:

```bash
kubectl apply -f k8s/config/
```

Deploy MongoDB:

```bash
kubectl apply -f k8s/mongodb/
```

Deploy Backend:

```bash
kubectl apply -f k8s/backend/
```

Deploy Frontend:

```bash
kubectl apply -f k8s/frontend/
```

Verify resources:

```bash
kubectl get all -n assessment
```

## Access the Application

Forward the Frontend service:

```bash
kubectl port-forward svc/frontend 8080:80 -n assessment
```

Open:

```
http://localhost:8080
```

## Features

- Containerized 3-tier application
- Internal service-to-service communication
- MongoDB authentication using Kubernetes Secrets
- Configuration management using ConfigMaps
- Health checks using Startup, Readiness, and Liveness Probes
- Resource requests and limits
- Stateful database deployment
