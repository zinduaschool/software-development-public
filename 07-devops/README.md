# Specialization: DevOps Engineering
# 12-Week DevOps Program: From Beginner to professional

## Course Objective:
By the end of this course, learners will be able to:

- Understand the DevOps lifecycle and its benefits.  
- Use Git and the command line for version control and collaboration.  
- Set up and use build tools like Maven and Gradle for software compilation and packaging.  
- Implement Continuous Integration and Continuous Deployment (CI/CD) pipelines using Jenkins.  
- Work with Docker to containerize applications.  
- Deploy and manage applications on Kubernetes.  
- Automate infrastructure using Terraform.  
- Deploy applications to Azure using cloud computing best practices.  
- Set up monitoring and logging for applications.  
- Work on real-world DevOps projects using industry-standard tools.

## Skills to be Learned:
- Linux & Bash Scripting
- Git & CLI
- Build Automation (Maven & Gradle)
- Continuous Integration & Deployment (Jenkins)
- Containerization (Docker)
- Kubernetes Orchestration
- Infrastructure as Code (Terraform)
- Cloud Computing (Azure)
- Monitoring & Logging (Prometheus, Grafana, Loki)
- Security & Best Practices

## Curriculum Breakdown

A **day-by-day DevOps course** combining theory, hands-on labs, and weekly projects.  
**Focus:** Azure, Kubernetes, CI/CD, Terraform, ArgoCD (GitOps), and DevOps culture.

---

## Overview
- **Duration:** 12 weeks (4 days of learning + 1 project day per week).  
- **Tools:** Azure, Docker, Kubernetes (AKS), Terraform, ArgoCD, Bash, Git, Azure Pipelines, Prometheus.  
- **Target Role:** DevOps Engineer.  

---

## 🗓 Weekly Breakdown

### **Week 1: DevOps Fundamentals + Linux & Bash Scripting**
> **Objective:** Understand DevOps culture and automate routine work: learners will be able to explain the DevOps lifecycle and the CALMS framework, work efficiently in the Linux command line, and write Bash scripts that automate and monitor system tasks securely.

| **Day** | **Topics**                          | **Activities**                                                                 | **Tools**               |  
|---------|-------------------------------------|--------------------------------------------------------------------------------|-------------------------|  
| **1**   | DevOps Principles & Lifecycle      | - Write a summary of CALMS framework <br>       | Linux CLI, VSCode       |  
| **2**   | Linux Basics & Bash Scripting       | - Practice `cd`, `ls`, `chmod` <br> - Copy and move items to various dir `mv`, `cp`, `scp`   |Linux CLI, VSCode Bash, Cron              |  
| **3**   | Advanced Bash Scripting & CLI Tools           | - Parse logs with `grep`/`awk`                           | `grep`, `awk`      |  
| **4**   | Security & Collaboration           | - Best Security Practices <br> - Collaboration in DevOps           | Trivy <br> Azure Key Vault   |  
| **Project** | **System Health Monitor** | - Bash script to log CPU/memory and email alerts.                             |                         |  

---

### **Week 2: Git & Version Control**  
> **Objective:** Collaborate safely on code: learners will be able to use Git branching, merging and pull-request workflows, resolve conflicts, and apply a team branching strategy.

| **Day** | **Topics**                          | **Activities**                                                                 | **Tools**               |  
|---------|-------------------------------------|--------------------------------------------------------------------------------|-------------------------|  
| **1**   | Git Basics                          | - Create a repo in Github <br> - Commit/push code                         | Git, Github      |  
| **2**   | Branching Strategies                | - Simulate GitFlow (feature/release branches) <br> - Resolve merge conflicts   | Git, Github, Azure DevOps       |  
| **3**   | Advanced Git; Rebasing vs Merging and Resolving conflicts             | - Fork/clone a repo, submit a PR <br> - Review and merge code                        | Azure Repos, GitHub CLI |  
| **4**   | Git Best Practices and Collaboration              | - Git workflows <br> - Branchig Strategies                         | Git, Github, Azure DevOps       |  
| **Project** | **Collaborative PR Workflow** | - Fork a repo, make changes, and automate PR checks.                        |                         |  

---

### **Week 3: Build Tools & CI/CD (Azure Pipelines/Jenkins)**  
> **Objective:** Automate build, test and release: learners will be able to configure build tools and design CI/CD pipelines in Azure Pipelines or Jenkins that build, test and deploy an application on every change.

| **Day** | **Topics**                          | **Activities**                                                                 | **Tools**               |  
|---------|-------------------------------------|--------------------------------------------------------------------------------|-------------------------|  
| **1**   | Introduction to Build Tools & CI Basics                | - Write a springboot code <br> - Run a build            | IDE, CLI, Maven/Gradle         |  
| **2**   | Build Configuration & Jenkins Jobs     | - Configure Maven to package JARs <br> - Integrate with Azure Pipelines/Jenkins <br> - Dependency management (Nexus/Artifactory)        | Nexus, Github, Git, Jenkins  |  
| **3**   | Automation & Pipeline Orchestration                  | - Jenkins Declarative Pipelines (`Jenkinsfile`) <br> - Parallel stages, error handling, notifications           | Jenkins <br> Git <br> Github         |  
| **4**   | Integration & Best Practices            | - Build/push Docker images to Dockerhub <br>   - Jenkins security (RBAC, credentials) <br>  - Build optimization (caching, incremental builds)                        | Docker, Jenkins <br> Git, Github <br> DockerHub Account             |  
| **Project** | **Java CI Pipeline** | - Implement a CI/CD pipeline for a sample application to Auto-build, test, scan, and push a Java app to Dockerhub.                              |                         |  

---

### **Week 4: Docker & Containerization**  
> **Objective:** Package applications consistently: learners will be able to write Dockerfiles, build and optimise images, run multi-container apps with Docker Compose, and publish images to a registry.

| **Day** | **Topics**                          | **Activities**                                                                 | **Tools**               |  
|---------|-------------------------------------|--------------------------------------------------------------------------------|-------------------------|  
| **1**   | Introduction to Docker and Containers                       | - Pull pulic images of webservers(ngnix) and run locally | Docker, Dockerhub                |  
| **2**   | Creating Docker Images and Running Containers        | - Create a Dockerfile for a Spring Boot app (Maven/Gradle) <br> - Build the image: `docker build -t my-app` . <br> - Run the container: `docker run -d -p 8080:8080 my-app` | Docker, IDE       |  
| **3**   |Managing Multi-Container Applications with Docker Compose     | - Use `docker-compose` to link a web app + Redis     | Docker                   |  
| **4**   | Docker Networking and Security Best Practices            | - Create custom bridge networks `docker network create` <br> - Restrict container privileges: `--cap-drop flags` <br> - Scan images for vulnerabilities with docker scan or Trivy <br> - Configure network segmentation to isolate containers        | Docker CLI  Docker CLI, Trivy/Docker Scout            |  
| **Project** | **Local Microservices Stack** | - Containerize a web application and run it using Docker               |                         |  

---
### **Week 5: Orchestration with Kubernetes (AKS)**  
> **Objective:** Run containers at scale: learners will be able to deploy and manage applications on Kubernetes (AKS) with deployments, services and config, and scale and update them safely.

| **Day** | **Topics**                          | **Activities**                                                                 | **Tools**               |  
|---------|-------------------------------------|--------------------------------------------------------------------------------|-------------------------|  
| **1**   | Introduction to Kubernetes and its Architecture          | - Deploy a local cluster using Minikube/Kind <br> - Explore control plane components (API Server, etcd, Scheduler) <br> - Use kubectl to interact with the cluster | Minikube/Kind, kubectl     |  
| **2**   | Deploying Applications on Kubernetes   | - Write YAML manifests for Deployments and Services <br> - Expose apps via NodePort/LoadBalancer <br> - Roll out updates using kubectl rollout       | kubectl, YAML, Minikube          |  
| **3**   | Managing Configurations with ConfigMaps and Secrets     | - Create ConfigMaps for environment variables <br> - Store sensitive data in Secrets (base64 encoding) <br> - Mount ConfigMaps/Secrets into Pods         | Minikube, kubectl, base64              |  
| **4**   | Kubernetes Best Practices for Scaling and Security  | - Configure Horizontal Pod Autoscaling (HPA) <br> - Implement RBAC for user permissions <br> - Enable healthChecks `probes` | HPA, RBAC, NetworkPolicy,           |  
| **Project** | **Deploy a Microservices-Based App** | - Deploy a 3-tier app (frontend, backend, database) using Kubernetes manifests. <br> - Expose the app via Ingress and secure secrets.         |   Kubernetes, Helm (optional)               |  
---
### **Week 6: Azure Cloud Fundamentals**  
> **Objective:** Deploy on the cloud: learners will be able to provision and manage core Azure compute, storage and identity services, and deploy an application following cloud best practices.

| **Day** | **Topics**                          | **Activities**                                                                 | **Tools**               |  
|---------|-------------------------------------|--------------------------------------------------------------------------------|-------------------------|  
| **1**   | Introduction To Cloud Computing & Azure Portal Basics                 | - Define cloud Computing <br> - Models Of Cloud Computing <br> - Create a VM, Storage Account, and VNet                                       | Azure Portal            |  
| **2**   | Azure CLI & PowerShell              | - Deploy a VM using CLI commands <br> - Automate with scripts                  | Azure CLI               |  
| **3**   | ARM Templates                       | - Deploy infrastructure using ARM templates                                   | ARM, Azure Portal       |  
| **4**   | Networking & Security               | - Configure NSGs, subnets, and peering                                        | Azure Networking        |  
| **Project** | **Static Website Hosting** | - Build a secure, automated blog hosting platform with a static frontend, API backend, and database using Azure services.                 |                         |  

---

### **Week 7: Infrastructure as Code (Terraform)**  
> **Objective:** Manage infrastructure as code: learners will be able to define, version and provision Azure infrastructure with Terraform using modules and remote state.

| **Day** | **Topics**                                 | **Activities**                                                                                                | **Tools**                            |  
|---------|--------------------------------------------|---------------------------------------------------------------------------------------------------------------|--------------------------------------|  
| **1**   | Introduction to IaC & Terraform Basics     | - Understand IaC concepts <br> - Install Terraform <br> - Learn Terraform workflow (`init`, `plan`, `apply`, `destroy`) <br> - Write basic `.tf` configurations          | Terraform, Azure (for examples)      |  
| **2**   | Terraform Language (HCL), Variables, Outputs | - Learn HCL syntax <br> - Define providers and resources <br> - Use input variables to parameterize configurations <br> - Define outputs to extract information | Terraform, Azure                     |  
| **3**   | State Management, Modules, Data Sources    | - Understand Terraform state <br> - Configure remote state (Azure Blob Storage) <br> - Create and use local/remote modules <br> - Use data sources to reference existing resources | Terraform, Azure Storage             |  
| **4**   | Advanced Topics & CI/CD Integration        | - Explore advanced concepts (e.g., Workspaces) <br> - Learn Terraform best practices <br> - Integrate Terraform with CI/CD using GitHub Actions | Terraform, GitHub Actions, Azure     |  
| **Project** | **AKS Web App Deployment**  | - Provision AKS cluster & networking with Terraform <br> - Deploy a web application to AKS <br> - Configure remote state backend <br> - Integrate Terraform with GitHub Actions CI/CD | Terraform, Azure (AKS, Networking, Storage), Docker, GitHub Actions |  

---

### **Week 8: Monitoring & Logging**  
> **Objective:** See what your systems are doing: learners will be able to collect metrics and logs with Prometheus, Grafana and Loki, build dashboards, and configure alerts.

| **Day** | **Topics**                          | **Activities**                                                                 | **Tools**               |  
|---------|-------------------------------------|--------------------------------------------------------------------------------|-------------------------|  
| **1**   | Azure Monitor Basics               | - Describe Azure Monitor <br> - Azure Log Analytics <br> - Azure Monitor Alerts <br> - Applications Insights <br> - Set up metrics and alerts for a VM                                         | Azure Monitor           |  
| **2**   | Basics of Grafana, Loki & Prometheus                      | - Describe Monitoring and log aggreration <br> - Understand various tools and the Architecture                                  | Logging and Monitoring |  
| **3**   | Prometheus & Grafana               | - Set Up Grafana & Build dashboards <br> - Deploy Prometheus and Loki on Kuberenetes using Helm <br> - Configure Prometheus to scrape Kuberentes metrics <br> - Set up Loki to collect and index container logs  <br> - Build unified dashboards in Grafana           | Prometheus, Loki, Grafana  |  
| **4**   | Security and Best Practices in Monitoring       |- Secure Prometheus/Grafana with RBAC and TLS <br> - Encrypt Loki logs at rest <br> - Restrict access to monitoring tools using Network Policies <br> - Audit monitoring configurations with kube-bench                         | Prometheus, Grafana, Loki, kube-bench   |  
| **Project** | **Grafana Dashboard for A Cluster** | - Create a Grafana dashboard for pod health and latency.                     |                         |  

---

### **Week 9: GitOps with ArgoCD**  
> **Objective:** Deploy declaratively from Git: learners will be able to implement a GitOps workflow with ArgoCD that keeps Kubernetes clusters in sync with a Git repository.

| **Day** | **Topics**                          | **Activities**                                                                 | **Tools**               |  
|---------|-------------------------------------|--------------------------------------------------------------------------------|-------------------------|  
| **1**   | GitOps Principles & ArgoCD Setup    | - Install ArgoCD on Kubernetes Cluster <br> - Sync manifests from Git repo                    | ArgoCD, Kubernetes Cluster             |  
| **2**   | Application Management              | - Deploy apps via ArgoCD UI/CLI <br> - Configure auto-sync & health checks     | ArgoCD CLI, Kubernetes Cluster   |  
| **3**   | Progressive Delivery                | - Implement canary deployments with Argo Rollouts                             | Argo Rollouts, Kubernetes Cluser          |  
| **4**   | Multi-Environment Strategies        | - Manage staging/prod with ArgoCD App of Apps <br> - Sync waves               | ArgoCD, Helm            |  
| **Project** | **GitOps Pipeline with ArgoCD** | - Deploy and update a microservice using ArgoCD with automated rollbacks. |                         |  

---

### **Week 10: Azure Networking**  
> **Objective:** Design secure cloud networks: learners will be able to configure Azure virtual networks, subnets, security groups and load balancing for a multi-tier application.

| **Day** | **Topics**                          | **Activities**                                                                 | **Tools**               |  
|---------|-------------------------------------|--------------------------------------------------------------------------------|-------------------------|  
| **1**   | Azure Load Balancers               | - Deploy an internal load balancer for VMs                                   | Azure Portal            |  
| **2**   | Application Gateway                | - Configure path-based routing for microservices                             | Application Gateway     |  
| **3**   | Private Endpoints                  | - Secure Azure Storage with private endpoints                                | Azure Networking        |  
| **4**   | VPN & Hybrid Connectivity          | - Simulate on-premises to Azure connectivity                                | Azure VPN Gateway       |  
| **Project** | **Secure 3-Tier App** | - Deploy a web app behind AGW with private backend.                    |                         |  

---

### **Week 11: AZ-900 Certification Prepare**  
> **Objective:** Prepare for certification: learners will be able to explain core Azure concepts, services, pricing and governance at the level required for the AZ-900 exam.

| **Day** | **Topics**                          | **Activities**                                                                 | **Tools**               |  
|---------|-------------------------------------|--------------------------------------------------------------------------------|-------------------------|  
| **1**   | Describe Cloud Concepts        | - Understand Cloud & It's Concepts <br> - Benefits of Using Cloud Service <br> - Describe Cloud Type Services       | - Azure Portal       |  
| **2**   | Describe Azure Architecture & Services   | - Core Architecturural Components of Azure <br> - Azure Compute & Networking Services <br> - Azure Storage Services <br> - Azure Identity Access & Security  | - Azure account/portal   |  
| **3**   | Describe Azure management and governance |- Cost Management <br> Ferature & Tools for Governance and Compliance | Azure Account/Portal    |  
| **4**   | Resource Management & Automation | - Automation Tool in Azure <br> - Monitoring tools in Azure  | Azure Portal |  
| **Project** | **[Practice Assessment for Exam AZ-900: Microsoft Azure Fundamentals](https://learn.microsoft.com/en-us/credentials/certifications/azure-fundamentals/practice/assessment?assessment-type=practice&assessmentId=23&practice-assessment-type=certification)** | - Complete assesement <br> - Simulate Exam environment with sandbox provided.                     |  - Azure Account on microsoft learn |  

---

### **Week 12: Capstone Project**  
> **Objective:** Deliver an end-to-end DevOps project: learners will be able to build a pipeline that takes an application from code to a monitored, production-style deployment using the course toolchain.

| **Day** | **Topics**                          | **Activities**                                                                 | **Tools**               |  
|---------|-------------------------------------|--------------------------------------------------------------------------------|-------------------------|  
| **1**   | Architecture Design                | - Plan CI/CD, AKS, monitoring, and IaC                                       | Whiteboard/Diagramming  |  
| **2**   | Implement CI/CD                   | - Build, test, Scan and push Docker images to ACR                                 | Azure Pipelines         |  
| **3**   | Deploy & Monitor                  | - Use **ArgoCD for GitOps** <br> - Set up Prometheus alerts                  | ArgoCD, Prometheus      |  
| **4**   | Demo & Documentation              | - Present the pipeline <br> - Write a README for the project                 | Markdown, AKS           |  
| **Project** | **Production-Ready App** | - Full pipeline: Code → CI → ArgoCD (GitOps) → Monitoring.             |                         |  

---

## 🛠 Getting Started
1. **Prerequisites:**  
   - [Azure Free Account](https://azure.microsoft.com/free)  
   - Install [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli), [Docker](https://docs.docker.com/get-docker/), [Terraform](https://learn.hashicorp.com/tutorials/terraform/install-cli), [ArgoCD](https://argo-cd.readthedocs.io/en/stable/getting_started/).  
2. **Clone this Repo:**  
   ```bash
   git clone 
   git checkout curric-streamline

**Optional Bonus Lessons:**
- Dockerizing and Deploying Applications to AWS/GCP
- Managing APIs with WSO2
- Advanced Kubernetes Features (Operators, CRDs, Service Mesh)
- Using Ansible for Configuration Management
- Advanced HarshCorp Vault
- Helm Charts
