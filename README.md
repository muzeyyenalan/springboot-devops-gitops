
# Spring Boot DevOps GitOps Lab

A hands-on DevOps project demonstrating CI/CD automation and Kubernetes deployment using a Java Spring Boot application.

## Architecture

Developer → GitHub → Jenkins CI/CD → Maven → Docker Image → Kubernetes

**Next phase:** Argo CD GitOps integration.

## Technology Stack

- **Language:** Java 21
- **Framework:** Spring Boot
- **Build Tool:** Maven
- **CI/CD:** Jenkins
- **Containerization:** Docker
- **Orchestration:** Kubernetes
- **Version Control:** Git & GitHub
- **GitOps:** Argo CD (planned)
- **Monitoring:** Prometheus & Grafana

## Application

The application exposes a simple HTTP endpoint:

`GET /`

Response:

`Hello from DevOps Lab!`

Application port: `8081`

## Run Locally

Build the application:

```bash
mvn clean package
```

Run the application:

```bash
java -jar target/demo-0.0.1-SNAPSHOT.jar
```

Open `http://localhost:8081/` in your browser.

## Docker

Build the Docker image:

```bash
docker build -t devops-demo:1.0 .
```

## Kubernetes

Deploy the application:

```bash
kubectl apply -f k8s/deployment.yaml
kubectl apply -f k8s/service.yaml
```

Verify the deployment:

```bash
kubectl get deployments
kubectl get pods
kubectl get services
```

## CI/CD Pipeline

The Jenkins pipeline is designed to automate:

1. Application build and testing with Maven
2. Docker image creation
3. Kubernetes deployment
4. Deployment status verification

## GitOps Roadmap

- [x] Develop