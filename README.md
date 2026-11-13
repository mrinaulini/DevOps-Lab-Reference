# DevOps Lab Reference

A complete, beginner-friendly DevOps practical reference repository based on the **DevOps Practical Guide – VS Code** PDF. This repository contains all source files, configurations, and instructions for performing DevOps experiments 1, 2, 3, 4, 6, and 7 using Visual Studio Code on Windows 11.

## Lab Overview

This repository is organized by experiment. Each experiment has its own folder containing the required source files, a README with step-by-step instructions, and the exact commands to run. The original PDF is included at the root of this repository.

| Experiment | Folder | Tools |
|------------|--------|-------|
| Experiment 1 – Git Essentials | `ex1/` | VS Code, Git, GitHub |
| Experiment 2 – Flask Application Using Docker | `flask-docker-app/` | VS Code, Python, Docker Desktop, Docker Hub |
| Experiment 3 – CI Pipeline Using GitHub Actions and Docker Hub | `python-ci-docker-lab/` | VS Code, Python, Git, GitHub Actions, Docker Hub |
| Experiment 4 – Jenkins Deployment Using Docker | `jenkins-static-site/` | VS Code, Docker Desktop, Jenkins, browser |
| Experiment 6 – Kubernetes CLI | `kubernetes-exp6/` | VS Code, Docker Desktop, Minikube, kubectl |
| Experiment 7 – Kubernetes Deployment and Service | `kubernetes-exp7/` | VS Code, Docker Desktop, Minikube, kubectl, Docker Hub |

> **Note:** Experiment 5 is not included in the source PDF and is therefore not present in this repository.

## Software Requirements

| Tool | Used For |
|------|----------|
| VS Code | Creating and editing project files |
| Git | Tracking code changes |
| Python | Running Python applications |
| Docker Desktop | Building and running containers |
| Minikube | Running a local Kubernetes cluster |
| kubectl | Managing Kubernetes resources |
| Web browser | GitHub, Docker Hub and local web applications |

### Verify Tools

```bash
git --version
python --version
docker --version
kubectl version --client
minikube version
```

If a command is not recognized, install that tool, make sure it is added to PATH, and restart VS Code.

## Installation and Setup

1. Install all required tools listed above.
2. Restart VS Code after installation.
3. Open this folder in VS Code (`File → Open Folder`).
4. Open a terminal (`Terminal → New Terminal`).
5. Docker Desktop must be running before Docker- or Minikube-based experiments.
6. Replace sample usernames and repository URLs with your own details (e.g., `USERNAME`, `REPOSITORY_URL`).

## Folder Structure

```
DevOps-Lab-Reference/
├── README.md
├── QUICK-REFERENCE.md
├── DevOps_Practical_Guide_VS_Code.pdf
├── ex1/
│   ├── README.md
│   ├── ex.py
│   └── ex1.py
├── flask-docker-app/
│   ├── README.md
│   ├── app.py
│   ├── requirements.txt
│   └── Dockerfile
├── python-ci-docker-lab/
│   ├── README.md
│   ├── app.py
│   ├── requirements.txt
│   ├── Dockerfile
│   ├── pytest.ini
│   ├── tests/
│   │   └── test_app.py
│   └── .github/
│       └── workflows/
│           └── ci-dockerhub.yml
├── jenkins-static-site/
│   ├── README.md
│   ├── docker-compose.yml
│   └── Jenkinsfile
├── kubernetes-exp6/
│   ├── README.md
│   └── nginx-pod.yaml
└── kubernetes-exp7/
    ├── README.md
    ├── python-deployment.yaml
    └── python-service.yaml
```

## Experiment-wise Instructions

Each experiment folder contains its own `README.md` with detailed step-by-step instructions. Below is a summary:

### Experiment 1 – Git Essentials (`ex1/`)

1. Create two files: `ex.py` and `ex1.py` (provided).
2. Configure Git: `git config --global user.name "Your Name"` and `git config --global user.email "your_email@gmail.com"`.
3. Initialize repo: `git init`, check with `git status`.
4. Stage and commit: `git add .`, `git commit -m "initial commit"`.
5. Create GitHub repo named `ex1`, connect: `git remote add origin https://github.com/USERNAME/ex1.git`.
6. Push: `git push -u origin main`.
7. Create branch `main1`, modify `ex.py`, commit and push.
8. On GitHub, create Pull Request from `main1` into `main` and merge.

### Experiment 2 – Flask Application Using Docker (`flask-docker-app/`)

1. Build image: `docker build -t flask-docker-app .`
2. Run container: `docker run -p 5000:5000 flask-docker-app`
3. Open `http://localhost:5000` in browser.
4. Log in to Docker Hub: `docker login`
5. Tag and push: `docker tag flask-docker-app USERNAME/flask-docker-app:latest`, `docker push USERNAME/flask-docker-app:latest`
6. Pull and run on port 5050: `docker pull USERNAME/flask-docker-app:latest`, `docker run -p 5050:5000 USERNAME/flask-docker-app:latest`

### Experiment 3 – CI Pipeline Using GitHub Actions and Docker Hub (`python-ci-docker-lab/`)

1. Install dependencies: `pip install -r requirements.txt`
2. Run app: `python app.py`
3. Run tests: `pytest -q`
4. Build locally: `docker build -t USERNAME/python-ci-lab:local .`
5. Run locally: `docker run --rm USERNAME/python-ci-lab:local`
6. Initialize git, commit, push to GitHub.
7. Add `DOCKERHUB_USERNAME` and `DOCKERHUB_TOKEN` to GitHub Secrets.
8. Create and push tag `v1.0.0`.

### Experiment 4 – Jenkins Deployment Using Docker (`jenkins-static-site/`)

1. Start Jenkins: `docker compose up -d`
2. Check container: `docker ps`
3. Get admin password: `docker exec jenkins-exp4 cat /var/jenkins_home/secrets/initialAdminPassword`
4. Open `http://localhost:8090`, complete setup.
5. Install HTML Publisher plugin.
6. Create Pipeline job `karunya-site-pipeline`, paste Jenkinsfile.
7. Build and check Console Output for `Finished: SUCCESS`.

### Experiment 6 – Kubernetes CLI (`kubernetes-exp6/`)

1. Start Minikube: `minikube start --driver=docker`
2. Check nodes: `kubectl get nodes`
3. Apply Pod: `kubectl apply -f nginx-pod.yaml`
4. Port forward: `kubectl port-forward pod/nginx-pod 8085:80`, open `http://localhost:8085`
5. Delete Pod, create Deployment: `kubectl create deployment my-nginx --image=nginx`
6. Scale: `kubectl scale deployment my-nginx --replicas=3`
7. Update image: `kubectl set image deployment/my-nginx nginx=nginx:1.25`
8. Rollback: `kubectl rollout undo deployment/my-nginx`
9. Expose: `kubectl expose deployment my-nginx --type=NodePort --port=80`
10. Get URL: `minikube service my-nginx --url`
11. Cleanup: `kubectl delete svc my-nginx`, `kubectl delete deployment my-nginx`

### Experiment 7 – Kubernetes Deployment and Service for Python App (`kubernetes-exp7/`)

1. Start Minikube: `minikube start --driver=docker`
2. Pull image: `docker pull deepa232/flask-docker-app:latest` (or your own image)
3. Apply Deployment: `kubectl apply -f python-deployment.yaml`
4. Apply Service: `kubectl apply -f python-service.yaml`
5. Get URL: `minikube service python-web-service --url`
6. Cleanup: `kubectl delete svc python-web-service`, `kubectl delete deployment python-web-deployment`

> **Note:** If you built your own Flask image in Experiment 2, replace `deepa232/flask-docker-app:latest` with your image name.

## How to Execute Each Experiment

1. Open the experiment folder in VS Code.
2. Open a terminal in VS Code (`Terminal → New Terminal`).
3. Follow the step-by-step procedure in that folder's `README.md`.
4. Refer to `QUICK-REFERENCE.md` for a condensed list of all terminal commands.

## Original PDF

The original PDF (`DevOps_Practical_Guide_VS_Code.pdf`) is included at the root of this repository for reference.

## Important Notes

- Some steps require a browser for GitHub, Docker Hub or Jenkins.
- Docker Desktop must be running before Docker- or Minikube-based experiments.
- Replace `USERNAME`, `REPOSITORY_URL`, and other placeholders with your own values.
- Experiment 5 is not covered in the source PDF and is not included in this repository.
- Do not publish Docker images or push to GitHub without explicit permission.