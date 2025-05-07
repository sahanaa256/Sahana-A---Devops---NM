# Setup Instructions: Integrating Security Scanning with SonarQube and Trivy

## Prerequisites

- Docker & Docker Compose
- Jenkins installed (or use Jenkins container)
- Kubernetes cluster (minikube/kind/eks/gke)
- Trivy installed (`brew install aquasecurity/trivy/trivy` or `apt install trivy`)
- SonarQube Scanner installed
- Git

## Step-by-Step Guide

### 1. Start SonarQube

```bash
cd sonarqube
docker-compose up -d
```

Visit `http://localhost:9000` to access SonarQube.

### 2. Configure Jenkins

- Install Jenkins plugins:
  - Pipeline
  - Docker Pipeline
  - Kubernetes CLI Plugin
  - SonarQube Scanner

- In Jenkins:
  - Go to *Manage Jenkins* → *Global Tool Configuration*
  - Add SonarQube Scanner and configure its name as `SonarQubeScanner`
  - Configure SonarQube server under *Manage Jenkins* → *Configure System*

### 3. Connect GitHub Repo

Ensure Jenkins can access your Git repository or fork and set up a new one with the same structure.

### 4. Run Jenkins Pipeline

- Create a new pipeline job.
- Point to the `jenkins/Jenkinsfile`.
- Run the pipeline.

### 5. Trivy Vulnerability Scanning

Trivy scans the Docker image built in the pipeline.

You can run it manually as well:

```bash
docker build -t my-app:latest .
trivy image --exit-code 1 --severity HIGH,CRITICAL my-app:latest
```

### 6. Deploy to Kubernetes

Make sure your `kubectl` is configured and apply the manifests:

```bash
kubectl apply -f k8s/deployment.yaml
```

---

## Notes

- Update the Jenkinsfile with your SonarQube project key and repository.
- Customize the Dockerfile and Kubernetes manifests for your real app.
