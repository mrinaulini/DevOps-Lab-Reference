# Experiment 7 – Kubernetes Deployment and Service for Python App

**Tools:** VS Code, Docker Desktop, Minikube, kubectl, Docker Hub  
**Aim:** To deploy a Python web application stored in Docker Hub to a Kubernetes cluster using a Deployment and Service, and access it through Minikube.

## Files

| File | Description |
|------|-------------|
| `python-deployment.yaml` | Deployment with 2 replicas, container port 5000 |
| `python-service.yaml` | NodePort Service for the Python app |

## Source Code

### python-deployment.yaml
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: python-web-deployment
  labels:
    app: python-web
spec:
  replicas: 2
  selector:
    matchLabels:
      app: python-web
  template:
    metadata:
      labels:
        app: python-web
    spec:
      containers:
      - name: python-web
        image: deepa232/flask-docker-app:latest
        ports:
        - containerPort: 5000
```

### python-service.yaml
```yaml
apiVersion: v1
kind: Service
metadata:
  name: python-web-service
spec:
  type: NodePort
  selector:
    app: python-web
  ports:
  - protocol: TCP
    port: 5000
    targetPort: 5000
```

## Project Structure

```
kubernetes-exp7/
|-- python-deployment.yaml
|-- python-service.yaml
```

## Kubernetes Commands

```bash
minikube start --driver=docker
kubectl get nodes
docker pull deepa232/flask-docker-app:latest
kubectl apply -f python-deployment.yaml
kubectl get deployments
kubectl get pods
kubectl describe deployment python-web-deployment
kubectl apply -f python-service.yaml
kubectl get svc
minikube service python-web-service --url
# Open the displayed URL in a browser.
kubectl delete svc python-web-service
kubectl delete deployment python-web-deployment
```

## Procedure

1. Start Docker Desktop and wait until Docker Engine is running.
2. Create and open a folder named `kubernetes-exp7` in VS Code.
3. Start Minikube and check the Kubernetes node.
4. Pull the Flask image from Docker Hub.
5. Create `python-deployment.yaml` with 2 replicas and container port 5000.
6. Apply the Deployment YAML and check the Deployment and Pods.
7. Describe the Deployment to view its details.
8. Create `python-service.yaml` with Service type NodePort.
9. Apply the Service YAML and check the Service.
10. Run `minikube service python-web-service --url` and open the displayed URL.
11. Delete the Service and Deployment after testing.

> **Note:** The Docker image must exist and be accessible in Docker Hub. If you built your own Flask image in Experiment 2, replace `deepa232/flask-docker-app:latest` in both the pull command and Deployment YAML with your image name. The image must listen on port 5000.

## Result

The Flask application image was deployed using a Kubernetes Deployment with 2 replicas and exposed through a NodePort Service.