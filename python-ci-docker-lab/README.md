# Experiment 3 – CI Pipeline Using GitHub Actions and Docker Hub

**Tools:** VS Code, Python, Git, GitHub Actions, Docker Hub  
**Aim:** To create a Python application and implement a CI pipeline using GitHub Actions to test the code, build a Docker image and push it to Docker Hub.

## Files

| File | Description |
|------|-------------|
| `app.py` | Python application with `add()` function |
| `requirements.txt` | Python dependencies (pytest) |
| `Dockerfile` | Docker image build instructions |
| `pytest.ini` | pytest configuration |
| `tests/test_app.py` | Unit tests for `add()` function |
| `.github/workflows/ci-dockerhub.yml` | GitHub Actions CI workflow |

## Source Code

### app.py
```python
def add(a, b):
    return a + b
if __name__ == "__main__":
    print("Hello from Python CI Lab!")
    print("2 + 3 =", add(2, 3))
```

### tests/test_app.py
```python
from app import add
def test_add():
    assert add(2, 3) == 5
    assert add(-1, 1) == 0
```

### requirements.txt
```
pytest==8.3.2
```

### pytest.ini
```
[pytest]
pythonpath = .
```

### Dockerfile
```
FROM python:3.11-slim
WORKDIR /app
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt
COPY . .
CMD ["python", "app.py"]
```

### .github/workflows/ci-dockerhub.yml
```yaml
name: ci-dockerhub
on:
  push:
    branches: ["main"]
    tags: ["*"]
  pull_request:
    branches: ["main"]
jobs:
  build-test-push:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v4
      - name: Set up Python
        uses: actions/setup-python@v5
        with:
          python-version: '3.11'
      - name: Install dependencies
        run: |
          python -m pip install --upgrade pip
          pip install -r requirements.txt
      - name: Run tests
        run: pytest -q
      - name: Docker metadata
        id: meta
        uses: docker/metadata-action@v5
        with:
          images: ${{ secrets.DOCKERHUB_USERNAME }}/python-ci-lab
          tags: |
            type=raw,value=latest,enable={{is_default_branch}}
            type=sha,prefix=sha-,format=short
            type=ref,event=tag
      - name: Set up QEMU
        uses: docker/setup-qemu-action@v3
      - name: Set up Docker Buildx
        uses: docker/setup-buildx-action@v3
      - name: Login to Docker Hub
        uses: docker/login-action@v3
        with:
          username: ${{ secrets.DOCKERHUB_USERNAME }}
          password: ${{ secrets.DOCKERHUB_TOKEN }}
      - name: Build and push
        uses: docker/build-push-action@v6
        with:
          context: .
          push: true
          tags: ${{ steps.meta.outputs.tags }}
          labels: ${{ steps.meta.outputs.labels }}
          platforms: linux/amd64
```

## Project Structure

```
python-ci-docker-lab/
|-- app.py
|-- requirements.txt
|-- Dockerfile
|-- pytest.ini
|-- tests/
|   |-- test_app.py
|-- .github/
    |-- workflows/
        |-- ci-dockerhub.yml
```

## Local Test and Git Commands

```bash
pip install -r requirements.txt
python app.py
pytest -q

docker build -t USERNAME/python-ci-lab:local .
docker run --rm USERNAME/python-ci-lab:local

git init
git add .
git commit -m "init lab"
git branch -M main
git remote add origin REPOSITORY_URL
git push -u origin main
git tag v1.0.0
git push origin v1.0.0
```

## Procedure

1. Create a folder named `python-ci-docker-lab`.
2. Create `app.py`, `requirements.txt`, `Dockerfile` and `pytest.ini`.
3. Create the `tests` folder and add `tests/test_app.py`.
4. Create `.github/workflows` and add `ci-dockerhub.yml`.
5. Install dependencies and run the Python application and tests locally.
6. Create a GitHub repository named `python-ci-docker-lab`.
7. In GitHub Settings → Secrets and variables → Actions, add `DOCKERHUB_USERNAME` and `DOCKERHUB_TOKEN`.
8. Push the project to the main branch of GitHub.
9. Open the Actions tab and verify the workflow steps.
10. Create and push the version tag `v1.0.0`, then verify the image tags in Docker Hub.

> **Note:** The GitHub repository secrets must be named exactly `DOCKERHUB_USERNAME` and `DOCKERHUB_TOKEN`. Use a Docker Hub access token, not your account password. Replace `USERNAME` and `REPOSITORY_URL` with your own values.

## Result

The Python tests run through GitHub Actions, and the workflow builds and pushes the Docker image to Docker Hub.