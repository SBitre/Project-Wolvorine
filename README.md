# 🐺 Project Wolverine

**Self-healing Kubernetes infrastructure with AI-powered incident response.**

Like its namesake, this infrastructure regenerates. When things break, it detects the damage, diagnoses the root cause using AI, and heals itself — automatically.

---

## Overview

Project Wolverine is a fully automated Kubernetes-based infrastructure on AWS that detects incidents, uses AI (Claude API) to diagnose issues from logs and metrics, and either auto-remediates or sends actionable Slack alerts with AI-generated incident reports.

## Architecture

```
┌─────────────────────────────────────────────────────────┐
│                      AWS Cloud                          │
│  ┌───────────────────────────────────────────────────┐  │
│  │                  VPC (Multi-AZ)                    │  │
│  │  ┌─────────────────────────────────────────────┐  │  │
│  │  │              EKS Cluster                     │  │  │
│  │  │                                              │  │  │
│  │  │  ┌──────────┐ ┌────────────┐ ┌───────────┐  │  │  │
│  │  │  │   App    │ │ Monitoring │ │ Incident  │  │  │  │
│  │  │  │Namespace │ │ Namespace  │ │ Response  │  │  │  │
│  │  │  │          │ │            │ │ Namespace │  │  │  │
│  │  │  │ API GW   │ │ Prometheus │ │           │  │  │  │
│  │  │  │ User Svc │ │ Grafana    │ │ AI Bot    │  │  │  │
│  │  │  │ Worker   │ │ AlertMgr   │ │ (Claude)  │  │  │  │
│  │  │  └──────────┘ └─────┬──────┘ └─────┬─────┘  │  │  │
│  │  │                     │              │         │  │  │
│  │  │                     └──── alerts ──┘         │  │  │
│  │  └─────────────────────────────────────────────┘  │  │
│  └───────────────────────────────────────────────────┘  │
│                                                         │
│  ┌─────────┐  ┌─────────┐  ┌─────────┐                 │
│  │   ECR   │  │   RDS   │  │   S3    │                 │
│  │ (Images)│  │(Postgres)│  │ (State) │                 │
│  └─────────┘  └─────────┘  └─────────┘                 │
└─────────────────────────────────────────────────────────┘
          │                                    │
          │ CI/CD (GitHub Actions)             │ Notifications
          ▼                                    ▼
    ┌──────────┐                         ┌──────────┐
    │  GitHub   │                         │  Slack   │
    └──────────┘                         └──────────┘
```

## System Layers

| Layer | Components | Skills Demonstrated |
|-------|-----------|-------------------|
| **Infrastructure** | Terraform, AWS VPC, EKS, ECR, S3 | IaC, Cloud Architecture, Networking |
| **Application** | 2–3 microservices in Docker on K8s | Containerization, Orchestration |
| **CI/CD** | GitHub Actions pipeline | Automation, GitOps, Testing |
| **Monitoring** | Prometheus + Grafana + CloudWatch | Observability, Alerting, Dashboards |
| **AI Incident Response** | Python bot + Claude API + Slack | AIOps, Automation, Integration |

## Self-Healing Capabilities

| Alert | Auto-Remediation | Notification |
|-------|-----------------|-------------|
| Pod CrashLoopBackOff | Force restart + log analysis | Root cause + action taken |
| CPU > 80% sustained | Scale replicas up | AI analysis + scaling action |
| OOM Killed | Increase memory limit 25%, restart | Memory analysis + new limits |
| High error rate | Rollback to previous deployment | Error pattern + rollback confirmation |
| Disk pressure | Clean up old logs/images | Storage analysis + cleanup report |

## Tech Stack

| Category | Technology | Why |
|----------|-----------|-----|
| Cloud | AWS (EKS, VPC, ECR, S3, RDS) | Most in-demand cloud platform |
| IaC | Terraform | Industry standard, multi-cloud capable |
| Containers | Docker | Essential DevOps skill |
| Orchestration | Kubernetes (EKS) | Most asked about in interviews |
| CI/CD | GitHub Actions | Free, popular, easy to demo |
| Monitoring | Prometheus + Grafana | Industry standard observability |
| Alerting | AlertManager + CloudWatch | Production alerting patterns |
| AI/AIOps | Claude API (Python) | Differentiator, shows innovation |
| Notifications | Slack Webhooks | Common in real DevOps workflows |
| Language | Python | Scripting, automation, bot logic |

## Project Structure

```
project-wolverine/
├── terraform/
│   ├── modules/
│   │   ├── vpc/            # VPC, subnets, NAT, IGW
│   │   ├── eks/            # EKS cluster, node groups, IAM
│   │   ├── ecr/            # Container registries
│   │   ├── rds/            # PostgreSQL database
│   │   └── s3-backend/     # Terraform state storage
│   └── environments/
│       ├── dev/            # Dev environment config
│       └── prod/           # Prod environment config
├── services/
│   ├── api-gateway/        # API routing + health checks
│   ├── user-service/       # CRUD service + PostgreSQL
│   └── worker-service/     # Background job processing
├── k8s/
│   ├── base/               # Shared K8s manifests
│   ├── namespaces/         # Namespace definitions + quotas
│   └── ingress/            # Ingress controller config
├── monitoring/
│   ├── prometheus/         # Prometheus config + ServiceMonitors
│   ├── grafana/            # Dashboard definitions
│   └── alertmanager/       # Alert rules + webhook config
├── incident-response/
│   ├── bot/                # AI incident response bot (Python)
│   └── chaos-scripts/      # Chaos testing scripts
├── .github/
│   └── workflows/          # CI/CD pipeline definitions
└── docs/                   # Architecture diagrams, decisions
```

## Getting Started

### Prerequisites

- AWS CLI configured with appropriate credentials
- Terraform >= 1.5
- kubectl
- Helm 3
- Docker
- Python 3.11+
- Claude API key ([api.anthropic.com](https://api.anthropic.com))
- Slack workspace with incoming webhook

### Quick Start

```bash
# Clone the repo
git clone https://github.com/<your-username>/project-wolverine.git
cd project-wolverine

# Set up Terraform state backend
cd terraform/modules/s3-backend
terraform init && terraform apply

# Deploy infrastructure
cd ../../environments/dev
terraform init && terraform apply

# Configure kubectl
aws eks update-kubeconfig --name wolverine-cluster --region us-east-1

# Deploy monitoring stack
helm install prometheus prometheus-community/kube-prometheus-stack -n monitoring

# Deploy the AI incident response bot
kubectl apply -f k8s/base/
```

## Chaos Testing (Demo)

Break things on purpose and watch Wolverine heal:

```bash
# Trigger CPU spike → auto-scaling
./incident-response/chaos-scripts/chaos-cpu.sh

# Trigger crash loop → AI diagnosis + restart
./incident-response/chaos-scripts/chaos-crash.sh

# Trigger OOM → memory adjustment
./incident-response/chaos-scripts/chaos-memory.sh

# Trigger error spike → auto-rollback
./incident-response/chaos-scripts/chaos-errors.sh
```

## Build Timeline

| Week | Focus | Status |
|------|-------|--------|
| 1 | Infrastructure & Foundation (Terraform, EKS, K8s setup) | 🔄 In Progress |
| 2 | Application & CI/CD (Microservices, GitHub Actions) | ⬜ Not Started |
| 3 | Monitoring & AI Incident Response (Prometheus, Grafana, Bot) | ⬜ Not Started |
| 4 | Polish, Chaos Testing & Demo | ⬜ Not Started |

## License

MIT License — see [LICENSE](LICENSE) for details.

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines.
