# QUICK-REFERENCE.md

All important terminal commands grouped by experiment. Use this during practical examinations.

---

## Experiment 1 – Git Essentials (`ex1/`)

```bash
# Configure Git
git config --global user.name "Your Name"
git config --global user.email "your_email@gmail.com"

# Initialize repository
git init
git status

# Stage and commit
git add .
git commit -m "initial commit"
git log

# Branch setup
git branch -M main

# Connect to GitHub
git remote add origin https://github.com/USERNAME/ex1.git
git remote -v
git push -u origin main

# Create and work on a branch
git checkout -b main1
# Modify ex.py, then:
git add .
git commit -m "updated file"
git push origin main1
```

---

## Experiment 2 – Flask Application Using Docker (`flask-docker-app/`)

```bash
# Build image
docker build -t flask-docker-app .

# Run container (port 5000)
docker run -p 5000:5000 flask-docker-app
# Open http://localhost:5000

# Docker Hub
docker login
docker tag flask-docker-app USERNAME/flask-docker-app:latest
docker push USERNAME/flask-docker-app:latest

# Pull and run on port 5050
docker pull USERNAME/flask-docker-app:latest
docker run -p 5050:5000 USERNAME/flask-docker-app:latest
```

---

## Experiment 3 – CI Pipeline Using GitHub Actions and Docker Hub (`python-ci-docker-lab/`)

```bash
# Local setup
pip install -r requirements.txt
python app.py
pytest -q

# Docker local test
docker build -t USERNAME/python-ci-lab:local .
docker run --rm USERNAME/python-ci-lab:local

# Git setup
git init
git add .
git commit -m "init lab"
git branch -M main
git remote add origin REPOSITORY_URL
git push -u origin main
git tag v1.0.0
git push origin v1.0.0
```

**GitHub Secrets required:** `DOCKERHUB_USERNAME`, `DOCKERHUB_TOKEN`

---

## Experiment 4 – Jenkins Deployment Using Docker (`jenkins-static-site/`)

```bash
# Start Jenkins
docker compose up -d

# Check container
docker ps

# Get initial admin password
docker exec jenkins-exp4 cat /var/jenkins_home/secrets/initialAdminPassword

# Access: http://localhost:8090
```

---

## Experiment 6 – Kubernetes CLI (`kubernetes-exp6/`)

```bash
# Start Minikube
minikube start --driver=docker
minikube status
kubectl get nodes

# Create and test Pod
kubectl apply -f nginx-pod.yaml
kubectl get pods
kubectl describe pod nginx-pod
kubectl port-forward pod/nginx-pod 8085:80
# Open http://localhost:8085 in another terminal

# Delete Pod, create Deployment
kubectl delete pod nginx-pod
kubectl create deployment my-nginx --image=nginx
kubectl get deployments
kubectl get pods

# Scale Deployment
kubectl scale deployment my-nginx --replicas=3
kubectl get pods

# Update image and check rollout
kubectl set image deployment/my-nginx nginx=nginx:1.25
kubectl rollout status deployment/my-nginx

# Rollback
kubectl rollout undo deployment/my-nginx

# Expose via NodePort Service
kubectl expose deployment my-nginx --type=NodePort --port=80
kubectl get svc
minikube service my-nginx --url

# Cleanup
kubectl delete svc my-nginx
kubectl delete deployment my-nginx
```

---

## Experiment 7 – Kubernetes Deployment and Service (`kubernetes-exp7/`)

```bash
# Start Minikube
minikube start --driver=docker
kubectl get nodes

# Pull Flask image
docker pull deepa232/flask-docker-app:latest
# Or use your own image: docker pull YOUR_USERNAME/flask-docker-app:latest

# Deploy
kubectl apply -f python-deployment.yaml
kubectl get deployments
kubectl get pods
kubectl describe deployment python-web-deployment

# Expose via Service
kubectl apply -f python-service.yaml
kubectl get svc
minikube service python-web-service --url
# Open the displayed URL in a browser

# Cleanup
kubectl delete svc python-web-service
kubectl delete deployment python-web-deployment
```

---

## Common Tool Checks

```bash
git --version
python --version
docker --version
kubectl version --client
minikube version
```

---

## Important Notes

- Replace `USERNAME`, `REPOSITORY_URL`, `deepa232/flask-docker-app:latest` with your own values.
- Docker Desktop must be running before Docker- or Minikube-based experiments.
- `port-forward` occupies its terminal; use a second terminal for remaining commands.
- Use a Docker Hub access token (not password) for `DOCKERHUB_TOKEN` secret.
- Experiment 5 is not included in the source PDF.