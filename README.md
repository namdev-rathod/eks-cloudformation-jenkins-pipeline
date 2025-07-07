Here’s a professional and visually appealing `README.md` for your GitHub project, written to impress visitors and explain the project clearly with emojis and sections.

---

````markdown
# 🚀 EKS Cluster Setup using CloudFormation + Jenkins CI/CD Pipeline

Welcome to the **EKS + Jenkins + CloudFormation** project! This repository helps you set up an **Amazon EKS Cluster** using **CloudFormation**, configure **Jenkins Declarative Pipeline** on EC2, and deploy scalable applications using **HPA** and **ALB** with AWS Load Balancer Controller.

🔗 GitHub Repo: [namdev-rathod/eks-cloudformation-jenkins-pipeline](https://github.com/namdev-rathod/eks-cloudformation-jenkins-pipeline)

---

## 🧰 Tech Stack

| 🛠️ Technology         | 🔍 Purpose                                  |
|------------------------|----------------------------------------------|
| AWS EKS               | Managed Kubernetes Cluster                   |
| AWS CloudFormation    | Infrastructure as Code (IaC)                 |
| Jenkins               | CI/CD Pipeline Automation                    |
| EC2                   | Jenkins Host                                 |
| Grafana               | Monitoring and Visualization                 |
| Nginx                 | Reverse Proxy for clean port access          |
| GitHub                | Version Control and Webhook Trigger          |
| AWS ALB Controller    | Load Balancer Integration with Kubernetes    |
| HPA                   | Auto-scaling Pods based on Metrics           |

---

## 📌 Project Architecture

```text
GitHub Push Event ➡️ Jenkins Webhook Trigger ➡️ Deploy to EKS via CloudFormation ➡️ 
Monitor with Grafana ➡️ Auto-scale with HPA ➡️ Traffic handled by ALB
````

---

## 🪜 Step-by-Step Setup Guide

### 1️⃣ Clone This Repository

```bash
git clone https://github.com/namdev-rathod/eks-cloudformation-jenkins-pipeline.git
cd eks-cloudformation-jenkins-pipeline
```

---

### 2️⃣ Launch EKS using CloudFormation

* Go to AWS CloudFormation Console
* Create a new stack using the provided YAML/JSON template
* Parameters to provide:

  * VPC and Subnet IDs
  * EKS Cluster name
  * Node Group configuration

> ✅ Templates are available under `/cloudformation/eks-cluster.yaml`

---

### 3️⃣ Jenkins Setup on EC2 Instance 🧪

* Launch an Amazon Linux 2 EC2 instance
* Install Jenkins and required plugins
* Install Docker, kubectl, awscli, eksctl
* Configure IAM role for EC2 to access EKS

---

### 4️⃣ Configure Nginx as Reverse Proxy 🌐

* Avoid accessing Jenkins at `:8080` or Grafana at `:3000`
* Use Nginx to proxy Jenkins on port `80` and Grafana on port `81`

```bash
sudo yum install nginx
sudo vi /etc/nginx/conf.d/jenkins.conf
```

Sample Nginx config:

```nginx
server {
    listen 80;
    location / {
        proxy_pass http://localhost:8080;
    }
}

server {
    listen 81;
    location / {
        proxy_pass http://localhost:3000;
    }
}
```

---

### 5️⃣ Jenkins Declarative Pipeline 🎯

* Configure pipeline with stages like:

  * Checkout
  * Build & Test
  * Deploy to EKS
* Pipeline file: `Jenkinsfile`

```groovy
pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/namdev-rathod/eks-cloudformation-jenkins-pipeline.git'
            }
        }
        stage('Deploy to EKS') {
            steps {
                sh 'kubectl apply -f k8s/deployment.yaml'
            }
        }
    }
}
```

---

### 6️⃣ GitHub Webhook Integration 🔄

* Go to your GitHub repo → Settings → Webhooks
* Add Jenkins endpoint: `http://<your-jenkins-domain>/github-webhook/`
* Content type: `application/json`
* Select: **Just the push event**

---

### 7️⃣ Enable HPA for Application Scaling 📈

* Configure Horizontal Pod Autoscaler for your deployment

```bash
kubectl autoscale deployment my-app --cpu-percent=50 --min=1 --max=5
```

> Ensure `metrics-server` is installed in your cluster.

---

### 8️⃣ Set Up AWS Load Balancer Controller ⚖️

* Install ALB Controller via Helm

```bash
helm repo add eks https://aws.github.io/eks-charts
helm install aws-load-balancer-controller eks/aws-load-balancer-controller \
  --set clusterName=<your-cluster-name> \
  --set serviceAccount.create=false \
  --set region=<your-region> \
  ...
```

* Annotate service for ALB ingress:

```yaml
annotations:
  service.beta.kubernetes.io/aws-load-balancer-type: "external"
```

---

## 📊 Monitoring with Grafana

* Install Prometheus and Grafana in the cluster
* Expose Grafana dashboard using NodePort or Ingress
* Access via Nginx reverse proxy

---

## 🔚 Final Outcome

✅ Fully automated EKS deployment with:

* CI/CD pipeline using Jenkins
* Auto-scaling enabled via HPA
* Reverse proxy with Nginx
* Metrics monitoring using Grafana
* Load balancing with AWS ALB

---

## 🙌 Author

**Namdev Rathod**
🌐 GitHub: [namdev-rathod](https://github.com/namdev-rathod)
📺 YouTube: [namdev.devops](https://www.youtube.com/@namdev.devops)

---

## 🌟 Show Your Support

If you found this project helpful, please ⭐ star the repository and share with others!

```

---

Would you like a matching `Jenkinsfile` and CloudFormation `eks-cluster.yaml` template to go along with the repo for completeness?
```
