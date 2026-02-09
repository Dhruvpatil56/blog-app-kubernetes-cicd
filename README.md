# 🚀 Blogging App - DevOps Pipeline on AWS EKS

> A modern Spring Boot blogging platform with full CI/CD automation, containerization, and production-grade monitoring on Kubernetes.

[![Java](https://img.shields.io/badge/Java-17-orange.svg)](https://openjdk.org/)
[![Spring Boot](https://img.shields.io/badge/Spring%20Boot-3.3.2-brightgreen.svg)](https://spring.io/projects/spring-boot)
[![Docker](https://img.shields.io/badge/Docker-Enabled-blue.svg)](https://www.docker.com/)
[![Kubernetes](https://img.shields.io/badge/Kubernetes-EKS-326CE5.svg)](https://aws.amazon.com/eks/)

---

## ✨ What's Inside

A fully automated DevOps workflow that takes code from your laptop to production on AWS:

- 📝 **Blog Application** - Share thoughts, create posts, explore trending DevOps news
- 🔄 **Auto CI/CD** - Push code → Jenkins builds → Deploys to EKS
- 📊 **Live Monitoring** - Prometheus + Grafana dashboards
- 🔒 **Secure** - Spring Security with authentication
- 🐳 **Containerized** - Docker multi-stage builds
- ☸️ **Cloud Native** - Running on AWS EKS

---

## 🛠 Tech Stack

**Backend**
- Spring Boot 3 • Spring Security • Spring Data JPA • H2 Database

**DevOps**
- Docker • Kubernetes (EKS) • Jenkins • GitHub Actions

**Monitoring**
- Prometheus • Grafana • Spring Actuator • Micrometer

**Cloud**
- AWS EC2 • EKS • IAM

---

## 🏗 Architecture
```
Developer → GitHub → Jenkins → Docker Hub → AWS EKS → Prometheus → Grafana
```

1. **Code Push** - Developer commits to GitHub
2. **CI/CD** - Jenkins builds, tests, and containerizes
3. **Deploy** - Kubernetes pulls image and deploys to EKS
4. **Monitor** - Prometheus scrapes metrics, Grafana visualizes

---

## 📦 Project Structure
```
.
├── Jenkinsfile              # CI/CD pipeline definition
├── Dockerfile               # Multi-stage Docker build
├── docker-compose.yml       # Local deployment
├── k8s/                     # Kubernetes manifests
│   ├── deployment.yaml
│   ├── service.yaml
│   └── servicemonitor.yaml
├── src/                     # Spring Boot application
└── pom.xml                  # Maven dependencies
```

---

## 🚦 CI/CD Pipeline

**Automated on every push to `main`:**

✅ Checkout code  
✅ Build with Maven  
✅ Run SonarQube analysis  
✅ Security scan with Trivy  
✅ Build Docker image  
✅ Push to Docker Hub  
✅ Deploy to EKS  
✅ Verify rollout  

---

## 📊 Monitoring

**Metrics tracked:**
- JVM memory & CPU usage
- HTTP request rates
- Thread pool stats
- Custom business metrics

**Access Grafana:** `http://<grafana-url>:3000`  
**Prometheus endpoint:** `/actuator/prometheus`

---

## 🔐 Security

- Spring Security enabled
- Public routes: `/login`, `/register`, `/actuator/prometheus`
- All other endpoints require authentication

---

## 🚀 Quick Start

**Local development:**
```bash
./mvnw spring-boot:run
```

**Docker:**
```bash
docker compose up -d
```

**Access:** `http://localhost:8080`

---

## 👨‍💻 Author

**Dhruv Patil**  
GitHub: [@Dhruvpatil56](https://github.com/Dhruvpatil56)

---

⭐ **Star this repo** if you found it helpful!
