# 🚀 CI/CD Pipeline Patterns for Modern Applications

This guide provides **end-to-end CI/CD pipeline strategies** using various tools like GitHub Actions, Jenkins, Docker, Kubernetes, Terraform, and ArgoCD. These patterns are commonly used in DevOps for microservices, serverless, GitOps, and secure deployments.

---

## 1. CI/CD Pipeline for Microservices

### 🎯 Objective

Automate the process of testing, building, and deploying multiple microservices.

### 🛠️ Tools

* **CI Tool**: GitHub Actions / Jenkins
* **Containerization**: Docker
* **Orchestration**: Kubernetes

### 🔁 Pipeline Steps

1. **Build**: Docker image per microservice using `docker build`
2. **Test**: Run unit tests (e.g., PyTest, Jest)
3. **Deploy**: Push to Kubernetes using manifests or Helm

### 📄 GitHub Actions Example

```yaml
yaml
name: CI/CD for Microservices
on:
  push:
    branches: [main]
jobs:
  build_and_deploy:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v2

      - name: Set up Docker Buildx
        uses: docker/setup-buildx-action@v1

      - name: Build Docker Image
        run: |
          docker build -t myapp:$GITHUB_SHA .
          docker push myapp:$GITHUB_SHA

      - name: Deploy to Kubernetes
        run: |
          kubectl apply -f k8s/deployment.yaml
          kubectl apply -f k8s/service.yaml
```

---

## 2. Jenkins Pipeline for Multi-Environment Deployment

### 🎯 Objective

Deploy applications to **Dev → Staging → Prod** environments.

### 📄 Jenkinsfile

```groovy
pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        script {
          docker.build("myapp:${GIT_COMMIT}")
        }
      }
    }
    stage('Deploy to Dev') {
      steps {
        script { deployToK8s('dev') }
      }
    }
    stage('Deploy to Staging') {
      steps {
        script { deployToK8s('staging') }
      }
    }
    stage('Deploy to Prod') {
      steps {
        script { deployToK8s('prod') }
      }
    }
  }
}

def deployToK8s(env) {
  sh "kubectl config use-context ${env}-context"
  sh "kubectl apply -f k8s/${env}/deployment.yaml"
  sh "kubectl apply -f k8s/${env}/service.yaml"
}
```

---

## 3. CI/CD with Infrastructure as Code (IaC)

### 🎯 Objective

Provision infrastructure using **Terraform** in the pipeline.

### 📄 GitHub Actions with Terraform

```yaml
name: IaC Pipeline
on:
  push:
    branches: [main]
jobs:
  terraform:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v2

      - name: Set up Terraform
        uses: hashicorp/setup-terraform@v1

      - name: Terraform Init
        run: terraform init

      - name: Terraform Plan
        run: terraform plan

      - name: Terraform Apply
        run: terraform apply -auto-approve

      - name: Deploy App
        run: |
          kubectl apply -f k8s/deployment.yaml
          kubectl apply -f k8s/service.yaml
```

---

## 4. Docker + Kubernetes CI/CD Pipeline

### 📄 Jenkinsfile

```groovy
pipeline {
  agent any
  stages {
    stage('Build Docker Image') {
      steps {
        script { docker.build("myapp:${GIT_COMMIT}") }
      }
    }
    stage('Push Image') {
      steps {
        script { docker.push("myapp:${GIT_COMMIT}") }
      }
    }
    stage('Deploy to K8s') {
      steps {
        script {
          sh "kubectl apply -f k8s/deployment.yaml"
          sh "kubectl apply -f k8s/service.yaml"
        }
      }
    }
  }
}
```

---

## 5. CI/CD for Serverless (AWS Lambda)

### 📄 GitHub Actions

```yaml
name: Serverless CI/CD
on:
  push:
    branches: [main]
jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v2

      - name: Set up AWS CLI
        run: |
          aws configure set aws_access_key_id ${{ secrets.AWS_ACCESS_KEY_ID }}
          aws configure set aws_secret_access_key ${{ secrets.AWS_SECRET_ACCESS_KEY }}
          aws configure set region ${{ secrets.AWS_REGION }}

      - name: Deploy Lambda
        run: |
          aws lambda update-function-code \
            --function-name my-function \
            --zip-file fileb://function.zip
```

---

## 6. Rollback Automation in CI/CD

### 📄 GitHub Actions

```yaml
name: CI/CD with Rollback
on:
  push:
    branches: [main]
jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v2

      - name: Deploy
        run: kubectl apply -f k8s/deployment.yaml

      - name: Monitor & Rollback
        run: |
          kubectl rollout status deployment/myapp
          if [ $? -ne 0 ]; then
            echo "Rollback triggered"
            kubectl rollout undo deployment/myapp
          fi
```

---

## 7. GitOps Pipeline with ArgoCD

### 📄 ArgoCD App Manifest

```yaml
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: myapp
spec:
  destination:
    server: https://kubernetes.default.svc
    namespace: default
  source:
    repoURL: 'https://github.com/myorg/myapp.git'
    targetRevision: HEAD
    path: k8s/
  syncPolicy:
    automated: {}
```

> 💡 GitOps ensures that Git is the source of truth for deployments.

---

## 8. Multistage Dockerfile + CI/CD

### 📄 Dockerfile

```dockerfile
# Stage 1: Build
FROM node:14 AS build
WORKDIR /app
COPY . .
RUN npm install && npm run build

# Stage 2: Run
FROM node:14
WORKDIR /app
COPY --from=build /app/dist .
CMD ["npm", "start"]
```

> 💡 Multistage builds reduce image size and separate concerns.

---

## 9. CI/CD with Security & Compliance

### 📄 GitHub Actions

```yaml
name: CI/CD with Security
on:
  push:
    branches: [main]
jobs:
  security:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v2

      - name: Snyk Scan
        run: snyk test

      - name: SonarQube Analysis
        run: sonar-scanner

      - name: Deploy
        run: kubectl apply -f k8s/deployment.yaml
```

---

## 10. Canary and Blue-Green Deployments

### 📄 GitHub Actions for Canary

```yaml
name: Canary Deployment
on:
  push:
    branches: [main]
jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v2

      - name: Deploy to Canary
        run: kubectl apply -f k8s/canary-deployment.yaml

      - name: Monitor & Promote
        run: |
          kubectl rollout status deployment/myapp-canary
          if [ $? -eq 0 ]; then
            kubectl apply -f k8s/production-deployment.yaml
```

## 11. Automated Testing and Quality Checks

### 🌟 Objective

Automate unit, integration, and E2E testing and ensure code quality before deploying to production.

### 🚀 Pipeline Steps

* **Checkout Code**: Pull latest changes.
* **Run Tests**: Use frameworks like Jest, JUnit, or Selenium.
* **Code Quality**: Run static analysis (e.g., SonarQube, ESLint).
* **Deploy**: Only if all checks pass.

### GitHub Actions Example

```yaml
name: Test and Quality Checks
on:
  push:
    branches:
      - main
jobs:
  test_and_quality:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v2

      - name: Run Unit Tests
        run: npm test

      - name: Run SonarQube Quality Check
        run: sonar-scanner

      - name: Deploy to Kubernetes
        run: |
          kubectl apply -f k8s/deployment.yaml
```

### 🔑 Key Concepts

* **Testing**: Automated validation of code correctness.
* **Quality Gate**: Prevent bad code from reaching production.

---

## 12. Database Migration and Rollback

### 🌟 Objective

Automate DB schema changes and enable rollback mechanisms.

### 🚀 Pipeline Steps

* **Run Migrations**: Use tools like Liquibase or Flyway.
* **Deploy App**: Proceed only if migration succeeds.
* **Rollback**: Revert DB and app if failure occurs.

### Jenkinsfile Example

```groovy
pipeline {
    agent any
    stages {
        stage('Database Migration') {
            steps {
                script {
                    sh "liquibase update"
                }
            }
        }
        stage('Deploy Application') {
            steps {
                script {
                    sh "kubectl apply -f k8s/deployment.yaml"
                }
            }
        }
        stage('Rollback') {
            when {
                failure()
            }
            steps {
                script {
                    sh "liquibase rollbackCount 1"
                    sh "kubectl rollout undo deployment/myapp"
                }
            }
        }
    }
}
```

### 🔑 Key Concepts

* **Database CI/CD**: Version-controlled schema management.
* **Resilience**: Safe fallback during failures.

---

## 13. CI/CD with Docker Compose

### 🌟 Objective

Build, test, and deploy multi-container apps with Docker Compose.

### 🚀 Pipeline Steps

* **Build Images**: For services defined in `docker-compose.yml`.
* **Run Tests**: Inside isolated containers.
* **Deploy**: Use Compose or Kubernetes.

### GitHub Actions Example

```yaml
name: CI/CD with Docker Compose
on:
  push:
    branches:
      - main
jobs:
  docker_compose:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v2

      - name: Install Docker Compose
        run: sudo apt-get install docker-compose

      - name: Build Docker Images
        run: docker-compose build

      - name: Run Tests
        run: docker-compose run test

      - name: Deploy with Docker Compose
        run: docker-compose up -d
```

### 🔑 Key Concepts

* **Service Isolation**: Run and test services independently.
* **Multi-container orchestration**: Quick local testing environment.

---

## 14. Blue-Green Deployment Strategy

### 🌟 Objective

Deploy with near-zero downtime by toggling between environments.

### 🚀 Pipeline Steps

* **Deploy to Green**: Deploy new version.
* **Test Green**: Run health and performance checks.
* **Switch Traffic**: Move traffic from Blue to Green.
* **Rollback**: Revert to Blue if needed.

### Jenkinsfile Example

```groovy
pipeline {
    agent any
    stages {
        stage('Deploy to Green') {
            steps {
                script {
                    sh "kubectl apply -f k8s/green-deployment.yaml"
                }
            }
        }
        stage('Test Green') {
            steps {
                script {
                    sh "curl -f http://green-environment/myapp/healthcheck"
                }
            }
        }
        stage('Switch Traffic to Green') {
            steps {
                script {
                    sh "kubectl expose deployment green --name=myapp --port=80 --type=LoadBalancer"
                }
            }
        }
        stage('Rollback') {
            when {
                failure()
            }
            steps {
                script {
                    sh "kubectl expose deployment blue --name=myapp --port=80 --type=LoadBalancer"
                }
            }
        }
    }
}
```

### 🔑 Key Concepts

* **Zero Downtime**: User experience remains uninterrupted.
* **Safer Releases**: Easier rollback path.

---

## 15. Serverless Continuous Deployment

### 🌟 Objective

Automate deployments for serverless apps like AWS Lambda.

### 🚀 Pipeline Steps

* **Build Function**: Prepare serverless app code.
* **Deploy**: Use platform CLIs (e.g., AWS CLI).
* **Test**: Confirm deployment with test invocations.

### GitHub Actions Example

```yaml
name: Serverless CI/CD
on:
  push:
    branches:
      - main
jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v2

      - name: Set up AWS CLI
        run: |
          aws configure set aws_access_key_id ${{ secrets.AWS_ACCESS_KEY_ID }}
          aws configure set aws_secret_access_key ${{ secrets.AWS_SECRET_ACCESS_KEY }}
          aws configure set region ${{ secrets.AWS_REGION }}

      - name: Deploy to Lambda
        run: |
          aws lambda update-function-code --function-name my-function --zip-file fileb://function.zip
```

### 🔑 Key Concepts

* **Serverless CI/CD**: Deploy without provisioning infrastructure.
* **Cloud-native**: Lightweight and scalable deployment models.

---

## 16. Canary Deployment Strategy

### 🌟 Objective

Gradually release a new version of the application to a small subset of users before full deployment.

### 🚀 Pipeline Steps

* **Deploy Canary Version**: Roll out the new version to a limited user base.
* **Monitor Canary Deployment**: Check health status and app performance.
* **Full Deployment**: Promote the new version if everything works fine.
* **Rollback**: Revert changes if issues are detected.

###  Jenkinsfile Example

```groovy
pipeline {
    agent any
    stages {
        stage('Deploy Canary') {
            steps {
                script {
                    sh "kubectl apply -f k8s/canary-deployment.yaml"
                }
            }
        }
        stage('Monitor Canary') {
            steps {
                script {
                    sh "kubectl rollout status deployment/myapp-canary"
                }
            }
        }
        stage('Full Deployment') {
            steps {
                script {
                    sh "kubectl apply -f k8s/full-deployment.yaml"
                }
            }
        }
        stage('Rollback') {
            when {
                failure()
            }
            steps {
                script {
                    sh "kubectl rollout undo deployment/myapp"
                }
            }
        }
    }
}
```

### 🔑 Key Concepts

* **Canary Deployment**: Release new features incrementally.
* **Monitoring**: Validate performance before full rollout.

---

## 17. Multi-Repository Microservices Pipeline

### 🌟 Objective

CI/CD pipeline for multiple repositories representing different microservices.

### 🚀 Pipeline Steps

* **Clone Repositories**: Fetch source code for each microservice.
* **Build & Test**: Independently process each repo.
* **Deploy**: Deploy services together or independently.

### GitHub Actions Example

```yaml
name: Multi-Repository CI/CD
on:
  push:
    branches:
      - main
jobs:
  build_and_deploy:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout Service 1
        uses: actions/checkout@v2
        with:
          repository: user/service1

      - name: Build Service 1
        run: docker build -t service1:$GITHUB_SHA .

      - name: Checkout Service 2
        uses: actions/checkout@v2
        with:
          repository: user/service2

      - name: Build Service 2
        run: docker build -t service2:$GITHUB_SHA .

      - name: Deploy Services
        run: |
          kubectl apply -f service1/k8s/deployment.yaml
          kubectl apply -f service2/k8s/deployment.yaml
```

### 🔑 Key Concepts

* **Multi-Repo CI/CD**: Coordination across services.
* **Independent Pipelines**: Flexibility to update individual services.

---

## 18. Self-Healing CI/CD Pipeline

### 🌟 Objective

Automatically detect failures and recover deployments.

### 🚀 Pipeline Steps

* **Deploy Application**: Initiate deployment.
* **Monitor Health**: Use scripts to validate application state.
* **Auto-Heal**: Trigger rollback or redeployment if failure is detected.

### Jenkinsfile Example

```groovy
pipeline {
    agent any
    stages {
        stage('Deploy Application') {
            steps {
                script {
                    sh "kubectl apply -f k8s/deployment.yaml"
                }
            }
        }
        stage('Monitor Health') {
            steps {
                script {
                    def result = sh(script: 'kubectl get pods -o jsonpath="{.items[0].status.phase}"', returnStdout: true).trim()
                    if (result != 'Running') {
                        error "Application is not running!"
                    }
                }
            }
        }
        stage('Auto-Healing') {
            when {
                failure()
            }
            steps {
                script {
                    sh "kubectl rollout undo deployment/myapp"
                }
            }
        }
    }
}
```

### 🔑 Key Concepts

* **Self-Healing**: Automated failure detection and recovery.
* **Resilience**: Increase uptime and deployment reliability.

---

## 19. Cloud-Native Deployment with Terraform + Kubernetes

### 🌟 Objective

Provision cloud infrastructure and deploy a Kubernetes-native application.

### 🚀 Pipeline Steps

* **Provision Infrastructure**: Use Terraform to create cloud resources (VPC, EC2, EKS, etc.).
* **Deploy to Kubernetes**: Apply deployment and service YAMLs.

### GitHub Actions Example

```yaml
name: Cloud-Native App Pipeline
on:
  push:
    branches:
      - main
jobs:
  infrastructure_and_deployment:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v2

      - name: Set up Terraform
        uses: hashicorp/setup-terraform@v1

      - name: Terraform Apply
        run: terraform apply -auto-approve

      - name: Deploy to Kubernetes
        run: kubectl apply -f k8s
```

### 🔑 Key Concepts

* **IaC + Kubernetes**: Declarative provisioning and app delivery.
* **GitHub Actions**: Automate cloud-native deployments.

