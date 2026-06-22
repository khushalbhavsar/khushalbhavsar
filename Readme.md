<!-- Header -->
<div align="center">

<h1>Khushal Bhavsar</h1>

<a href="https://git.io/typing-svg">
  <img src="https://readme-typing-svg.demolab.com?font=JetBrains+Mono&weight=500&size=18&duration=3500&pause=1000&color=00D9FF&center=true&vCenter=true&width=680&height=40&lines=DevOps+Engineer+%E2%86%92+MLOps+Builder;GitOps+%7C+DevSecOps+%7C+Kubernetes+%7C+AWS;I+build+pipelines+that+don't+break+at+2am" alt="Typing SVG" />
</a>

<br/><br/>

<a href="https://www.linkedin.com/in/khushal-bhavsar-/"><img src="https://img.shields.io/badge/LinkedIn-0A66C2?style=flat-square&logo=linkedin&logoColor=white"/></a>
<a href="mailto:khushalbhavsar41@gmail.com"><img src="https://img.shields.io/badge/Gmail-EA4335?style=flat-square&logo=gmail&logoColor=white"/></a>
<a href="https://portfolio-topaz-omega-46.vercel.app/"><img src="https://img.shields.io/badge/Portfolio-111111?style=flat-square&logo=vercel&logoColor=white"/></a>
<a href="https://github.com/khushalbhavsar"><img src="https://img.shields.io/badge/GitHub-181717?style=flat-square&logo=github&logoColor=white"/></a>
<img src="https://komarev.com/ghpvc/?username=khushalbhavsar&label=profile+views&color=00d9ff&style=flat-square"/>

</div>

---

## About

B.Tech in Electronics & Telecommunication Engineering — specialized in cloud-native DevOps, now expanding into MLOps. I build production-grade infrastructure: multi-environment Terraform, GitOps pipelines with ArgoCD, DevSecOps CI/CD with real security gates, and Kubernetes workloads on AWS EKS.

Currently building an end-to-end ML pipeline with the same discipline I bring to infrastructure — observable, reproducible, and GitOps-managed.

- 📍 Pune, India &nbsp;|&nbsp; 📬 khushalbhavsar41@gmail.com
- 🔍 Open to DevOps, Cloud, and MLOps roles

---

## Projects

### 🔷 Swiggy Clone — GitOps CI/CD on AWS EKS

> Full production-grade DevOps project: React 18 food delivery app deployed to AWS EKS via **GitOps** (ArgoCD App-of-Apps), a **11-stage DevSecOps Jenkins pipeline**, multi-environment Terraform IaC, and a complete monitoring stack with **9 Grafana dashboards**.

**What makes it production-grade:**
- ArgoCD with `selfHeal: true` + `prune: true` — cluster state always matches Git, no drift
- Kyverno ClusterPolicies: `require-resource-limits` + `restrict-image-registries` (ECR only)
- 4-layer security: SonarQube → OWASP Dep-Check → Trivy FS Scan → Trivy Image Scan
- ECR scan-on-push with AES256 encryption
- Multi-env Terraform (dev/staging/prod) with S3 remote state

**Full pipeline flow:**
```
Code Push
  └─▶ GitHub Webhook
        └─▶ Jenkins (11 stages)
              ├─ SonarQube Analysis + Quality Gate
              ├─ OWASP Dependency-Check
              ├─ Trivy Filesystem Scan
              ├─ Docker Build → Push to ECR
              ├─ Trivy Image Scan
              └─ Update deployment.yaml (git push)
                    └─▶ ArgoCD Auto-Sync
                          └─▶ EKS Deploy (4 replicas)
                                └─▶ Prometheus + Grafana (9 dashboards)
```

**AWS Infrastructure (Terraform):**
```
VPC 10.0.0.0/16  ──  us-east-1a / us-east-1b
  ├─ Public Subnets   → EC2 Jumphost (Jenkins · SonarQube · 20+ tools)
  ├─ Private Subnets  → EKS Worker Nodes
  ├─ EKS Cluster      → Control Plane + Node Group (Autoscaler IAM)
  ├─ ECR              → swiggy repo (scan-on-push · AES256)
  └─ S3               → Remote Terraform state
```

**Stack:** `Terraform` `AWS EKS` `ECR` `VPC` `Jenkins` `ArgoCD` `Docker` `Prometheus` `Grafana` `SonarQube` `Trivy` `OWASP` `Kyverno` `React 18` `MariaDB` `PostgreSQL`

[![GitHub](https://img.shields.io/badge/View_Repo-181717?style=flat-square&logo=github&logoColor=white)](https://github.com/khushalbhavsar/Swiggy-Gitops-EKS)

---

### 🔷 RedBus Clone — Full-Stack DevOps on AWS EKS

> Full-stack bus booking application (React + Node.js + MongoDB) with a complete cloud-native DevOps implementation: parallel Jenkins pipeline stages, path-based Nginx ingress routing, Stripe payment integration, and a fully custom Prometheus + Grafana monitoring stack deployed via Kubernetes manifests.

**What makes it stand out:**
- Parallel pipeline stages — frontend & backend built, tested, and scanned simultaneously (faster CI)
- Path-based ingress routing: `/` → React frontend, `/api/*` → Node.js backend
- Multi-stage Docker builds — frontend served via Nginx with non-root user hardening on backend
- Custom Kubernetes monitoring stack (no Helm) — RBAC, scrape configs, Grafana JSON dashboards
- Stripe payment processing integrated end-to-end (React → Express → Stripe API)

**Full pipeline flow:**
```
Code Push
  └─▶ Jenkins (11 stages)
        ├─ Parallel: npm install (frontend + backend)
        ├─ Parallel: tests (frontend + backend)
        ├─ SonarQube Analysis + Quality Gate
        ├─ OWASP Dependency-Check
        ├─ Trivy Filesystem Scan
        ├─ Parallel: Docker Build (frontend + backend)
        ├─ Parallel: Trivy Image Scan (frontend + backend)
        ├─ Push images to ECR
        └─▶ kubectl apply → EKS
              └─▶ Prometheus + Grafana (custom manifests)
```

**Kubernetes workloads:**
```
Namespace: redbus
  ├─ frontend-deployment  (React + Nginx · 2 replicas · port 80)
  ├─ backend-deployment   (Node.js · 2 replicas · port 5000)
  ├─ Nginx Ingress        (path-based: / and /api/*)
  ├─ ConfigMap            (env vars)
  └─ Secrets              (MongoDB URI · Stripe key)

Namespace: monitoring
  ├─ Prometheus           (ClusterRole + ServiceAccount + RBAC)
  ├─ Grafana              (JSON dashboard provisioned)
  ├─ Node Exporter        (DaemonSet · host metrics)
  └─ Kube State Metrics   (K8s object metrics)
```

**Stack:** `Terraform` `AWS EKS` `ECR` `Jenkins` `Docker` `Kubernetes` `Nginx Ingress` `Prometheus` `Grafana` `SonarQube` `Trivy` `OWASP` `React 17` `Node.js` `MongoDB Atlas` `Stripe`

[![GitHub](https://img.shields.io/badge/View_Repo-181717?style=flat-square&logo=github&logoColor=white)](https://github.com/khushalbhavsar)

---

### 🔶 Customer Churn Prediction — MLOps Pipeline *(In Progress)*

> End-to-end MLOps system bridging my DevOps background into ML: DVC for data versioning, MLflow for experiment tracking, FastAPI + BentoML for model serving, Evidently for drift monitoring, Prefect for orchestration — all deployed on AWS SageMaker with GitOps-managed delivery via ArgoCD on EKS.

```
Data (DVC) ──▶ Train (MLflow) ──▶ Serve (FastAPI / BentoML)
                                          │
                                   Monitor (Evidently)
                                          │
                              GitOps Deploy (ArgoCD + EKS)
```

**Stack:** `Python` `DVC` `MLflow` `FastAPI` `BentoML` `Prefect` `Evidently` `SageMaker` `ArgoCD` `EKS`

---

## Skills

| Category | Technologies |
|:---------|:-------------|
| **Cloud & Infra** | `AWS` `EC2` `VPC` `IAM` `S3` `RDS` `EKS` `ECR` `ALB` `Auto Scaling` `Lambda` `CloudWatch` `CloudFormation` |
| **Containers** | `Docker` `Kubernetes` `Amazon EKS` |
| **IaC** | `Terraform` — modules · remote state · multi-environment · workspaces |
| **CI/CD** | `Jenkins` `GitHub Actions` `GitLab CI` `ArgoCD` |
| **DevSecOps** | `SonarQube` `Trivy` `OWASP Dependency-Check` `Kyverno` |
| **Monitoring** | `Prometheus` `Grafana` `Loki` |
| **MLOps** *(building)* | `DVC` `MLflow` `FastAPI` `BentoML` `Prefect` `Evidently` `SageMaker` |
| **Languages** | `Python` `Bash` |
| **OS** | `Linux` — Ubuntu · Amazon Linux |

---

## Certifications

<p>
  <img src="https://img.shields.io/badge/AWS-Solutions_Architect_Associate-FF9900?style=for-the-badge&logo=amazon-aws&logoColor=white"/>
</p>

---

## GitHub Stats

<div align="center">
  <img width="390" src="https://github-readme-stats.vercel.app/api?username=khushalbhavsar&show_icons=true&hide_border=true&bg_color=0d1117&title_color=6e40c9&icon_color=38bdae&text_color=c9d1d9&count_private=true&include_all_commits=true" alt="GitHub Stats"/>
  <img width="390" src="https://github-readme-stats.vercel.app/api/top-langs/?username=khushalbhavsar&layout=compact&hide_border=true&bg_color=0d1117&title_color=6e40c9&text_color=c9d1d9&langs_count=8" alt="Top Languages"/>
</div>

<br/>

<div align="center">
  <img src="https://streak-stats.demolab.com?user=khushalbhavsar&hide_border=true&background=0d1117&stroke=6e40c9&ring=6e40c9&fire=38bdae&currStreakLabel=38bdae&sideLabels=c9d1d9&currStreakNum=ffffff&sideNums=ffffff&dates=8b949e" alt="GitHub Streak"/>
</div>

<br/>

<img src="https://github-readme-activity-graph.vercel.app/graph?username=khushalbhavsar&custom_title=Contribution+Graph&bg_color=0d1117&color=38bdae&line=6e40c9&point=38bdae&area_color=6e40c9&title_color=ffffff&area=true&hide_border=true" width="100%"/>

---

## Currently Building

| Phase | Focus | Status |
|-------|-------|--------|
| 1–3 | Python for ML · NumPy · Pandas | ✅ Done |
| 4–5 | ML fundamentals · Scikit-learn | ✅ Done |
| 6 | DVC · MLflow · Experiment tracking | 🔄 In progress |
| 7 | FastAPI · BentoML · Model serving | ⏳ Upcoming |
| 8 | Prefect orchestration | ⏳ Upcoming |
| 9 | Evidently · Drift monitoring | ⏳ Upcoming |
| 10 | AWS SageMaker | ⏳ Upcoming |
| 11 | Capstone: Churn Prediction System | ⏳ Upcoming |

---

<div align="center">
  <img src="https://quotes-github-readme.vercel.app/api?type=horizontal&theme=tokyonight&animation=grow_out_in" alt="Dev Quote"/>
</div>

---

<div align="center">

<a href="https://www.linkedin.com/in/khushal-bhavsar-/"><img src="https://img.shields.io/badge/LinkedIn-0A66C2?style=for-the-badge&logo=linkedin&logoColor=white"/></a>
<a href="mailto:khushalbhavsar41@gmail.com"><img src="https://img.shields.io/badge/Gmail-EA4335?style=for-the-badge&logo=gmail&logoColor=white"/></a>
<a href="https://github.com/khushalbhavsar"><img src="https://img.shields.io/badge/GitHub-181717?style=for-the-badge&logo=github&logoColor=white"/></a>
<a href="https://portfolio-topaz-omega-46.vercel.app/"><img src="https://img.shields.io/badge/Portfolio-FF5722?style=for-the-badge&logo=google-chrome&logoColor=white"/></a>

<br/><br/>
<sub>If your infra breaks at 2am, I've probably already written a runbook for it.</sub>

</div>

<img width="100%" src="https://capsule-render.vercel.app/api?type=waving&color=0:00d9ff,50:1a1b27,100:0d1117&height=80&section=footer"/>
