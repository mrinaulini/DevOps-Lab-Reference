# Experiment 2 – Flask Application Using Docker

**Tools:** VS Code, Python, Docker Desktop, Docker Hub  
**Aim:** To create a Flask web application, containerize it using Docker and push the Docker image to Docker Hub.

## Files

| File | Description |
|------|-------------|
| `app.py` | Flask web application |
| `requirements.txt` | Python dependencies |
| `Dockerfile` | Docker image build instructions |

## Source Code

### app.py
```python
from flask import Flask
app = Flask(__name__)
@app.route('/')
def home():
    return "Welcome to Flask + Docker!"
if __name__ == '__main__':
    app.run(host='0.0.0.0', port=5000)
```

### requirements.txt
```
Flask==2.2.5
```

### Dockerfile
```
FROM python:3.9-slim
WORKDIR /app
COPY requirements.txt .
RUN pip install -r requirements.txt
COPY . .
EXPOSE 5000
CMD ["python", "app.py"]
```

## Project Structure

```
flask-docker-app/
|-- app.py
|-- requirements.txt
|-- Dockerfile
```

## Docker Commands

```bash
docker build -t flask-docker-app .
docker run -p 5000:5000 flask-docker-app
# Open http://localhost:5000
docker login
docker tag flask-docker-app USERNAME/flask-docker-app:latest
docker push USERNAME/flask-docker-app:latest
docker pull USERNAME/flask-docker-app:latest
docker run -p 5050:5000 USERNAME/flask-docker-app:latest
```

## Procedure

1. Create and open a folder named `flask-docker-app` in VS Code.
2. Create `app.py`, `requirements.txt` and `Dockerfile` (already provided).
3. Write the Flask application in `app.py`.
4. Add `Flask==2.2.5` to `requirements.txt`.
5. Write the Docker instructions in `Dockerfile`.
6. Start Docker Desktop and wait until Docker Engine is running.
7. Build the Docker image from the project folder.
8. Run the container and open `http://localhost:5000` in a browser.
9. Log in to Docker Hub and create a repository named `flask-docker-app`.
10. Tag the image with your Docker Hub username and push it.
11. Pull the image and run it using host port 5050.

> **Note:** Replace `USERNAME` with your Docker Hub username. Keep the container terminal open while testing the page. Press Ctrl+C to stop a foreground container.

## Result

The Flask application was built into a Docker image, run as a container, pushed to Docker Hub, pulled and run again.