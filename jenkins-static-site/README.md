# Experiment 4 – Jenkins Deployment Using Docker

**Tools:** VS Code, Docker Desktop, Jenkins, web browser  
**Aim:** To run Jenkins inside Docker using Docker Compose and create a Jenkins pipeline to generate and publish a simple website.

## Files

| File | Description |
|------|-------------|
| `docker-compose.yml` | Jenkins service configuration |
| `Jenkinsfile` | Jenkins pipeline script |

## Source Code

### docker-compose.yml
```yaml
services:
  jenkins:
    image: jenkins/jenkins:lts
    container_name: jenkins-exp4
    restart: unless-stopped
    ports:
      - "8090:8080"
      - "50001:50000"
    volumes:
      - jenkins_home:/var/jenkins_home
volumes:
  jenkins_home:
    driver: local
```

### Jenkinsfile
```groovy
pipeline {
    agent any
    stages {
        stage('Generate site') {
            steps {
                script {
                    sh 'rm -rf site || true'
                    sh 'mkdir -p site/assets'
                    writeFile file: 'site/index.html', text: """
                    &lt;!DOCTYPE html&gt;
                    &lt;html&gt;
                    &lt;head&gt;
                        &lt;title&gt;Karunya University&lt;/title&gt;
                        &lt;link rel="stylesheet" href="assets/style.css"&gt;
                    &lt;/head&gt;
                    &lt;body&gt;
                        &lt;h1&gt;Karunya University&lt;/h1&gt;
                        &lt;h2&gt;About Karunya University&lt;/h2&gt;
                        &lt;p&gt;Welcome to Karunya Institute of Technology and Sciences.&lt;/p&gt;
                    &lt;/body&gt;
                    &lt;/html&gt;
                    """
                    writeFile file: 'site/assets/style.css', text: """
                    body {
                        font-family: Arial;
                        text-align: center;
                        background: lightblue;
                        padding: 50px;
                    }
                    h1 {
                        color: darkblue;
                    }
                    """
                }
            }
        }
        stage('Publish site') {
            steps {
                publishHTML([
                    allowMissing: false,
                    alwaysLinkToLastBuild: true,
                    keepAll: true,
                    reportDir: 'site',
                    reportFiles: 'index.html',
                    reportName: 'Karunya University - Simple Site'
                ])
            }
        }
    }
    post {
        always {
            archiveArtifacts artifacts: 'site/**', fingerprint: true
        }
    }
}
```

## Docker Commands

```bash
docker compose up -d
docker ps
docker exec jenkins-exp4 cat /var/jenkins_home/secrets/initialAdminPassword
```

## Procedure

1. Create and open a folder named `jenkins-static-site` in VS Code.
2. Create `docker-compose.yml` and enter the Jenkins service configuration.
3. Start Jenkins using `docker compose up -d`.
4. Check the container using `docker ps`.
5. Retrieve the initial administrator password from the Jenkins container.
6. Open `http://localhost:8090`, enter the password and complete the setup.
7. Install the HTML Publisher plugin.
8. Create a new Pipeline job named `karunya-site-pipeline`.
9. Select Pipeline script and paste the Jenkinsfile script.
10. Save the job and click Build Now.
11. Open Console Output and check for `Finished: SUCCESS`.
12. Open the published Karunya University website from the build page.

> **Note:** Install the HTML Publisher plugin before running this pipeline. This pipeline uses shell commands (`sh`), so it is intended for the standard Linux-based Jenkins container shown above. The Docker socket mount is not needed for this website-generation pipeline.

## Result

Jenkins was started in Docker. The pipeline generated the website, published the HTML report and archived the generated files.