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
                      │
         PersistentVolumeClaim (PVC)
                      │
         PersistentVolume (PV)
                      │
         Host Storage (Kind Node)
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
- PersistentVolume (PV)
- PersistentVolumeClaim (PVC)
- Resource Requests & Limits
- Startup, Readiness & Liveness Probes

## Project Structure

```
k8s/
├── backend/
│   ├── deployment.yml
│   └── service.yml
│
├── frontend/
│   ├── deployment.yml
│   └── service.yml
│
├── mongodb/
│   ├── statefulset.yml
│   └── service.yml
│
├── storage/
│   ├── pv.yml
│   └── pvc.yml
│
├── config/
│   ├── configmap.yml
│   ├── secret.yml
│   └── secret-example.yml
│
└── namespace.yml
```

## Deployment

Create the namespace:

```bash
kubectl apply -f k8s/namespace.yml
```

Deploy configuration:

Copy the example secret file and update the credentials as required:
```bash
cp k8s/config/secret-example.yml k8s/config/secret.yml
```
Apply the Secret:
```bash
kubectl apply -f k8s/config/
```

Deploy MongoDB:

```bash
kubectl apply -f k8s/mongodb/
```
### Deploy Storage

```bash
kubectl apply -f k8s/storage/
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
- Kubernetes Deployments and StatefulSets
- Internal service-to-service communication
- MongoDB authentication using Kubernetes Secrets
- Configuration management using ConfigMaps
- Persistent MongoDB storage using PV & PVC
- Health checks (Startup, Readiness & Liveness)
- Resource Requests & Limits
- Headless Service for StatefulSet networking

## Persistent Storage

MongoDB data is stored using Kubernetes Persistent Volumes.

Components used:

- PersistentVolume (PV)
- PersistentVolumeClaim (PVC)
- StatefulSet volume mount

The MongoDB data directory (`/data/db`) is mounted to the PersistentVolume through the PersistentVolumeClaim, ensuring data persists even if the MongoDB Pod is deleted and recreated.

## Verification

Check all resources:

```bash
kubectl get all -n assessment
```

Verify Persistent Volume:

```bash
kubectl get pv
```

Verify Persistent Volume Claim:

```bash
kubectl get pvc -n assessment
```

Verify storage mount:

```bash
kubectl describe pod mongodb-0 -n assessment
```