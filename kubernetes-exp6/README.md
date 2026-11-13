# Experiment 6 – Kubernetes CLI

**Tools:** VS Code, Docker Desktop, Minikube, kubectl  
**Aim:** To manage Kubernetes resources using kubectl by creating Pods and Deployments, scaling applications, performing rolling updates and rollbacks, and exposing applications through a Service.

## Files

| File | Description |
|------|-------------|
| `nginx-pod.yaml` | NGINX Pod definition |

## Source Code

### nginx-pod.yaml
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx-pod
  labels:
    app: nginx
spec:
  containers:
  - name: nginx
    image: nginx:latest
    ports:
    - containerPort: 80
```

## Project Structure

```
kubernetes-exp6/
|-- nginx-pod.yaml
```

## Kubernetes Commands

```bash
minikube start --driver=docker
minikube status
kubectl get nodes
kubectl apply -f nginx-pod.yaml
kubectl get pods
kubectl describe pod nginx-pod
kubectl port-forward pod/nginx-pod 8085:80
# Open http://localhost:8085 in a browser.
# Keep port-forward running while testing.
# Open another terminal:
kubectl delete pod nginx-pod
kubectl create deployment my-nginx --image=nginx
kubectl get deployments
kubectl get pods
kubectl scale deployment my-nginx --replicas=3
kubectl get pods
kubectl set image deployment/my-nginx nginx=nginx:1.25
kubectl rollout status deployment/my-nginx
kubectl rollout undo deployment/my-nginx
kubectl expose deployment my-nginx --type=NodePort --port=80
kubectl get svc
minikube service my-nginx --url
kubectl delete svc my-nginx
kubectl delete deployment my-nginx
```

## Procedure

1. Start Docker Desktop and wait until Docker Engine is running.
2. Create and open a folder named `kubernetes-exp6` in VS Code.
3. Start Minikube using the Docker driver and check the node status.
4. Create `nginx-pod.yaml` and define an NGINX Pod.
5. Apply the YAML file and check the Pod status.
6. Describe the Pod and access NGINX using port forwarding.
7. Delete the Pod and create an NGINX Deployment.
8. Scale the Deployment to 3 replicas.
9. Update the NGINX image and check rollout status.
10. Perform a rollback to the previous Deployment version.
11. Expose the Deployment using a NodePort Service and open the service URL.
12. Delete the Service and Deployment after testing.

> **Note:** The port-forward command occupies its terminal until stopped. Use a second VS Code terminal for the remaining commands. The `minikube service` command displays the URL to open.

## Result

A Pod and Deployment were created. The Deployment was scaled to 3 replicas, updated and rolled back. The NGINX application was exposed through a Service.