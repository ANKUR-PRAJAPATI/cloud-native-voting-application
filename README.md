# 🗳️ Cloud-Native Voting Application

![Kubernetes](https://img.shields.io/badge/kubernetes-%23326ce5.svg?style=for-the-badge&logo=kubernetes&logoColor=white)
![Docker](https://img.shields.io/badge/docker-%230db7ed.svg?style=for-the-badge&logo=docker&logoColor=white)
![Python](https://img.shields.io/badge/python-3670A0?style=for-the-badge&logo=python&logoColor=ffdd54)
![Redis](https://img.shields.io/badge/redis-%23DD0031.svg?style=for-the-badge&logo=redis&logoColor=white)
![PostgreSQL](https://img.shields.io/badge/postgresql-%23316192.svg?style=for-the-badge&logo=postgresql&logoColor=white)

A distributed, cloud-native voting application built with microservices architecture, demonstrating modern DevOps practices including containerization, orchestration, CI/CD pipelines, and real-time data processing.

---

## 📋 Table of Contents

- [Problem Statement](#-problem-statement)
- [Solution](#-solution)
- [Tech Stack](#-tech-stack)
- [Tools Used](#-tools-used)
- [Application Architecture](#-application-architecture)
- [Features](#-features)
- [Getting Started](#-getting-started)
- [Deployment](#-deployment)
- [Monitoring](#-monitoring)
- [Future Scope](#-future-scope)
- [Contributing](#-contributing)
- [License](#-license)

---

## 🎯 Problem Statement

Traditional voting and polling applications face numerous challenges in modern cloud environments:

- **Scalability Issues** 📈 - Unable to handle sudden traffic spikes during peak voting periods
- **Single Point of Failure** ⚠️ - Monolithic architecture causes complete system downtime
- **Data Inconsistency** 🔄 - Race conditions in concurrent vote processing
- **Slow Response Times** 🐌 - Poor user experience during high load
- **Limited Reliability** 💔 - No fault tolerance or automatic recovery
- **Complex Deployments** 🚀 - Lengthy manual deployment processes
- **Poor Observability** 👁️ - Difficulty monitoring system health and performance
- **Vote Manipulation** 🔒 - Lack of security measures to prevent duplicate voting

---

## ✨ Solution

This project implements a cloud-native voting application using microservices architecture with the following capabilities:

### 🚀 Key Features

- **Microservices Architecture** - Independent, loosely-coupled services for better scalability
- **Real-time Results** - Live vote counting and result updates using Redis
- **Horizontal Scaling** - Auto-scale based on traffic with Kubernetes HPA
- **High Availability** - Multi-replica deployments with load balancing
- **Data Persistence** - PostgreSQL for reliable vote storage
- **In-Memory Caching** - Redis for fast vote processing and real-time updates
- **Zero-Downtime Deployments** - Rolling updates with health checks
- **Service Discovery** - Kubernetes DNS for seamless service communication
- **Cloud-Agnostic** - Runs on any Kubernetes cluster (AWS EKS, GKE, AKS, on-premise)

---

## 🛠️ Tech Stack

### Frontend Services
- **Python Flask** 🐍 - Voting interface
- **Node.js + Express** 🟢 - Results dashboard
- **React.js** ⚛️ - Modern UI components (optional)
- **HTML/CSS/JavaScript** 🎨 - Web interface

### Backend Services
- **Python** 🐍 - Vote processing worker
- **Node.js** 📗 - Result aggregation service
- **.NET Core** 💠 - Worker service (alternative implementation)

### Databases
- **PostgreSQL** 🐘 - Persistent vote storage
- **Redis** 🔴 - In-memory queue and cache
- **MongoDB** 🍃 - Analytics data (optional)

### Infrastructure
- **Docker** 🐳 - Container runtime
- **Kubernetes** ☸️ - Container orchestration
- **Helm** ⎈ - Package management
- **Nginx Ingress** 🌐 - Load balancer and routing

---

## 🔨 Tools Used

### Development
- **VS Code** 💻 - Code editor
- **Git** 📚 - Version control
- **Docker Desktop** 🖥️ - Local container development
- **Minikube/Kind** 🎪 - Local Kubernetes cluster
- **Postman** 📮 - API testing

### Container & Orchestration
- **Docker** 🐋 - Containerization
- **Docker Compose** 🐙 - Multi-container orchestration
- **Kubernetes** ⚓ - Production orchestration
- **kubectl** ⚙️ - Kubernetes CLI
- **Helm** 📦 - Kubernetes package manager
- **Kustomize** 🎛️ - Kubernetes configuration management

### CI/CD Pipeline
- **GitHub Actions** ⚡ - Automated workflows
- **Jenkins** 🤖 - Build automation
- **ArgoCD** 🔄 - GitOps continuous delivery
- **Tekton** 🔧 - Cloud-native CI/CD
- **Flux** 🌊 - GitOps toolkit

### Monitoring & Observability
- **Prometheus** 📊 - Metrics collection
- **Grafana** 📈 - Visualization dashboards
- **Jaeger** 🔭 - Distributed tracing
- **ELK Stack** 🔍 - Centralized logging
- **Loki** 📝 - Log aggregation

### Cloud Platforms
- **AWS EKS** ☁️ - Elastic Kubernetes Service
- **Google GKE** 🌩️ - Google Kubernetes Engine
- **Azure AKS** 🔷 - Azure Kubernetes Service
- **DigitalOcean** 🌊 - Managed Kubernetes

### Security & Compliance
- **Vault** 🔐 - Secrets management
- **OPA** 🛡️ - Policy enforcement
- **Falco** 🦅 - Runtime security
- **Trivy** 🔍 - Container vulnerability scanning

---

## 🏗️ Application Architecture

```
                         ┌──────────────────┐
                         │   Load Balancer  │
                         │  (Nginx Ingress) │
                         └────────┬─────────┘
                                  │
                 ┌────────────────┼────────────────┐
                 │                                 │
        ┌────────▼─────────┐            ┌─────────▼─────────┐
        │  Voting Service  │            │  Results Service  │
        │   (Python Flask) │            │   (Node.js)       │
        │   Port: 5000     │            │   Port: 4000      │
        └────────┬─────────┘            └─────────┬─────────┘
                 │                                 │
                 └────────────┬────────────────────┘
                              │
                    ┌─────────▼──────────┐
                    │    Redis Queue     │
                    │  (In-Memory Cache) │
                    │    Port: 6379      │
                    └─────────┬──────────┘
                              │
                    ┌─────────▼──────────┐
                    │  Worker Service    │
                    │  (Python/Node.js)  │
                    │  Vote Processor    │
                    └─────────┬──────────┘
                              │
                    ┌─────────▼──────────┐
                    │   PostgreSQL DB    │
                    │  (Persistent Data) │
                    │    Port: 5432      │
                    └────────────────────┘
```

### Service Communication Flow

1. **User casts vote** → Voting Service (Frontend)
2. **Vote queued** → Redis (Message Queue)
3. **Worker processes** → Reads from Redis queue
4. **Vote stored** → PostgreSQL database
5. **Results updated** → Results Service queries database
6. **Real-time display** → Results shown to users

---

## ⚡ Features

### Core Functionality
- ✅ **Simple Voting Interface** - User-friendly UI for casting votes
- ✅ **Real-time Results** - Live vote count updates without refresh
- ✅ **Multiple Options** - Support for multiple voting choices
- ✅ **Vote Validation** - Prevent duplicate voting from same IP/session
- ✅ **Result Visualization** - Charts and graphs for vote distribution
- ✅ **Mobile Responsive** - Works seamlessly on all devices

### Cloud-Native Features
- 🔧 **Auto-scaling** - Horizontal Pod Autoscaler based on CPU/memory
- 🔧 **Self-healing** - Automatic restart of failed containers
- 🔧 **Load Balancing** - Traffic distribution across multiple pods
- 🔧 **Service Mesh** - (Optional) Istio for advanced traffic management
- 🔧 **Blue-Green Deployment** - Zero-downtime releases
- 🔧 **Canary Deployments** - Gradual rollout of new versions
- 🔧 **Health Checks** - Liveness and readiness probes
- 🔧 **Resource Limits** - CPU and memory constraints per service

### DevOps Features
- 🚀 **CI/CD Pipeline** - Automated build, test, and deploy
- 🚀 **GitOps Workflow** - Infrastructure as code with Git
- 🚀 **Multi-environment** - Dev, staging, production deployments
- 🚀 **Rollback Capability** - Quick revert to previous versions
- 🚀 **Monitoring Dashboards** - Real-time metrics and alerts
- 🚀 **Log Aggregation** - Centralized logging across services

---

## 🚀 Getting Started

### Prerequisites

- Docker Desktop (v20.10+)
- Kubernetes cluster (Minikube/Kind/Cloud)
- kubectl (v1.24+)
- Helm (v3.0+)
- Git

### Quick Start with Docker Compose

1. **Clone the repository**
   ```bash
   git clone https://github.com/ANKUR-PRAJAPATI/cloud-native-voting-app.git
   cd cloud-native-voting-app
   ```

2. **Start all services**
   ```bash
   docker-compose up -d
   ```

3. **Access the application**
   ```
   Voting Interface: http://localhost:5000
   Results Dashboard: http://localhost:4000
   ```

4. **Stop all services**
   ```bash
   docker-compose down
   ```

### Kubernetes Deployment

1. **Create namespace**
   ```bash
   kubectl create namespace voting-app
   ```

2. **Deploy Redis**
   ```bash
   kubectl apply -f k8s/redis-deployment.yaml -n voting-app
   kubectl apply -f k8s/redis-service.yaml -n voting-app
   ```

3. **Deploy PostgreSQL**
   ```bash
   kubectl apply -f k8s/postgres-deployment.yaml -n voting-app
   kubectl apply -f k8s/postgres-service.yaml -n voting-app
   ```

4. **Deploy Application Services**
   ```bash
   kubectl apply -f k8s/voting-deployment.yaml -n voting-app
   kubectl apply -f k8s/voting-service.yaml -n voting-app
   kubectl apply -f k8s/worker-deployment.yaml -n voting-app
   kubectl apply -f k8s/result-deployment.yaml -n voting-app
   kubectl apply -f k8s/result-service.yaml -n voting-app
   ```

5. **Deploy Ingress**
   ```bash
   kubectl apply -f k8s/ingress.yaml -n voting-app
   ```

6. **Check deployment status**
   ```bash
   kubectl get pods -n voting-app
   kubectl get services -n voting-app
   ```

### Using Helm Chart

```bash
# Install the application
helm install voting-app ./helm-chart \
  --namespace voting-app \
  --create-namespace \
  --values values.yaml

# Upgrade the application
helm upgrade voting-app ./helm-chart \
  --namespace voting-app

# Uninstall
helm uninstall voting-app -n voting-app
```

---

## 🚢 Deployment

### Environment Variables

Create a `.env` file or Kubernetes ConfigMap:

```bash
# Database Configuration
POSTGRES_HOST=postgres
POSTGRES_PORT=5432
POSTGRES_DB=votes
POSTGRES_USER=postgres
POSTGRES_PASSWORD=postgres

# Redis Configuration
REDIS_HOST=redis
REDIS_PORT=6379

# Application Configuration
VOTE_OPTIONS=Cats,Dogs
RESULT_REFRESH_INTERVAL=5
```

### Kubernetes ConfigMap

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: voting-app-config
  namespace: voting-app
data:
  POSTGRES_HOST: "postgres"
  REDIS_HOST: "redis"
  VOTE_OPTIONS: "Cats,Dogs"
```

### Horizontal Pod Autoscaling

```yaml
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata:
  name: voting-app-hpa
  namespace: voting-app
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: voting-service
  minReplicas: 2
  maxReplicas: 10
  metrics:
  - type: Resource
    resource:
      name: cpu
      target:
        type: Utilization
        averageUtilization: 70
  - type: Resource
    resource:
      name: memory
      target:
        type: Utilization
        averageUtilization: 80
```

### Resource Limits

```yaml
resources:
  requests:
    memory: "128Mi"
    cpu: "100m"
  limits:
    memory: "256Mi"
    cpu: "200m"
```

---

## 📊 Monitoring

### Prometheus Metrics

```yaml
# ServiceMonitor for Prometheus
apiVersion: monitoring.coreos.com/v1
kind: ServiceMonitor
metadata:
  name: voting-app-metrics
  namespace: voting-app
spec:
  selector:
    matchLabels:
      app: voting-app
  endpoints:
  - port: metrics
    interval: 30s
```

### Grafana Dashboard

Import the provided Grafana dashboard (`grafana-dashboard.json`) to monitor:

- Total votes cast
- Votes per second
- Response time
- Error rate
- Pod CPU/Memory usage
- Database connections
- Redis queue length

### Health Endpoints

```bash
# Voting Service Health
curl http://localhost:5000/health

# Results Service Health
curl http://localhost:4000/health

# Sample Response
{
  "status": "healthy",
  "database": "connected",
  "redis": "connected",
  "uptime": "3600s"
}
```

---

## 🔮 Future Scope

### Planned Enhancements

- [ ] **User Authentication** 🔐 - OAuth2/JWT-based user login
- [ ] **Vote Analytics** 📊 - Advanced analytics and insights dashboard
- [ ] **Multi-language Support** 🌍 - Internationalization (i18n)
- [ ] **Mobile Apps** 📱 - Native iOS and Android applications
- [ ] **Email Notifications** 📧 - Notify users of vote confirmation
- [ ] **Social Media Integration** 🔗 - Share results on Twitter, Facebook
- [ ] **AI-powered Insights** 🤖 - ML models for trend prediction
- [ ] **Blockchain Voting** ⛓️ - Immutable vote records using blockchain
- [ ] **Voice Voting** 🎤 - Voice-based vote casting with Alexa/Google Home
- [ ] **Real-time Chat** 💬 - Discussion forum for voters
- [ ] **Advanced Security** 🛡️ - Rate limiting, DDoS protection, captcha
- [ ] **A/B Testing** 🧪 - Feature flags and experimentation
- [ ] **Multi-tenancy** 🏢 - Support multiple voting campaigns
- [ ] **Scheduled Voting** ⏰ - Start/end times for voting periods
- [ ] **Export Results** 📥 - Download results in CSV, PDF formats
- [ ] **API Gateway** 🚪 - Kong/Ambassador for API management
- [ ] **Service Mesh** 🕸️ - Istio for advanced traffic control
- [ ] **Chaos Engineering** 💥 - Chaos Monkey for resilience testing
- [ ] **Cost Optimization** 💰 - Spot instances, right-sizing
- [ ] **Compliance** ✅ - GDPR, SOC2 compliance features

### Performance Improvements

- [ ] **CDN Integration** - CloudFlare/CloudFront for static assets
- [ ] **Database Sharding** - Horizontal PostgreSQL scaling
- [ ] **Read Replicas** - PostgreSQL read replicas for better performance
- [ ] **Caching Strategy** - Multi-level caching with Varnish
- [ ] **Compression** - Gzip/Brotli compression for responses
- [ ] **GraphQL API** - Efficient data fetching

---

## 🧪 Testing

### Unit Tests

```bash
# Run Python tests
cd voting-service
python -m pytest tests/

# Run Node.js tests
cd result-service
npm test
```

### Integration Tests

```bash
# Test complete flow
docker-compose -f docker-compose.test.yaml up --abort-on-container-exit
```

### Load Testing

```bash
# Using K6
k6 run load-tests/voting-load-test.js

# Using Apache Bench
ab -n 10000 -c 100 http://localhost:5000/vote
```

---

## 🤝 Contributing

Contributions are welcome! Please follow these steps:

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/RealTimeAnalytics`)
3. Commit your changes (`git commit -m 'Add real-time analytics dashboard'`)
4. Push to the branch (`git push origin feature/RealTimeAnalytics`)
5. Open a Pull Request

### Contribution Guidelines

- Write unit tests for new features
- Follow PEP 8 (Python) and Airbnb style guide (JavaScript)
- Update documentation for API changes
- Ensure all tests pass before submitting PR
- Add meaningful commit messages

---

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

## 👨‍💻 Author

**Ankur Prajapati**

💼 **LinkedIn:** [linkedin.com/in/ankur-prajapati-5618a1258](https://linkedin.com/in/ankur-prajapati-5618a1258)  
📧 **Email:** prajapatiankur37@gmail.com  
💻 **GitHub:** [@ANKUR-PRAJAPATI](https://github.com/ANKUR-PRAJAPATI)  
🔗 **Project Link:** [Cloud-Native Voting Application](https://github.com/ANKUR-PRAJAPATI/cloud-native-voting-app)

---

## 🙏 Acknowledgments

- Docker and Kubernetes communities for excellent documentation
- The original voting app example from Docker samples
- Cloud Native Computing Foundation (CNCF) for cloud-native tools
- PostgreSQL and Redis communities for robust databases
- Prometheus and Grafana teams for monitoring solutions
- Open-source contributors who inspired this project

---

<div align="center">
  
### ⭐ If you found this project helpful, please consider giving it a star!

**Made with ❤️ and lots of ☕**

📬 **Feel free to reach out for collaborations or questions!**

</div>
