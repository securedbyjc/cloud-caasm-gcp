# Cloud CAASM on GCP 🚀

Public demo project showcasing Google Cloud Architect and CAASM integration using GCP services.
Created by Jeffrey Collins | Eagle Defense Systems LLC



\# Cloud CAASM on GCP

\## Continuous Asset \& Attack Surface Management Platform



\### Project Overview

A production-ready CAASM (Continuous Asset \& Attack Surface Management) platform built entirely on Google Cloud Platform's free tier, demonstrating all six Professional Cloud Architect (PCA) certification domains.



\### Architecture Components



```

┌─────────────────────────────────────────────────────────────────┐

│                     Cloud CAASM Architecture                     │

├─────────────────────────────────────────────────────────────────┤

│                                                                   │

│  ┌──────────────┐      ┌──────────────┐      ┌──────────────┐  │

│  │  Data Sources│──────▶│  GCS Bucket  │      │  Secret Mgr  │  │

│  │ (CSV/API)    │      │ (asset-data) │      │ (API keys)   │  │

│  └──────────────┘      └──────┬───────┘      └──────────────┘  │

│                               │                                  │

│                               ▼                                  │

│                    ┌────────────────────┐                       │

│                    │  Ingestor Service  │                       │

│                    │   (Cloud Run)      │                       │

│                    └──────────┬─────────┘                       │

│                               │                                  │

│                               ▼                                  │

│                    ┌────────────────────┐                       │

│                    │   Pub/Sub Topics   │                       │

│                    │ • asset-events     │                       │

│                    │ • finding-events   │                       │

│                    └──────────┬─────────┘                       │

│                               │                                  │

│                    ┌──────────┴─────────┐                       │

│                    ▼                    ▼                        │

│         ┌────────────────────┐  ┌────────────────────┐         │

│         │ Normalizer Service │  │ Coverage Evaluator │         │

│         │   (Cloud Run)      │  │   (Cloud Run)      │         │

│         └──────────┬─────────┘  └──────────┬─────────┘         │

│                    │                       │                    │

│                    ▼                       ▼                    │

│              ┌─────────────────────────────────┐               │

│              │         BigQuery                │               │

│              │  • assets.inventory             │               │

│              │  • assets.relationships         │               │

│              │  • coverage.findings            │               │

│              │  • coverage.metrics             │               │

│              └─────────────────┬───────────────┘               │

│                                │                                │

│                                ▼                                │

│                    ┌────────────────────┐                       │

│                    │  API Gateway       │                       │

│                    │   (Cloud Run)      │                       │

│                    │  • /metrics        │                       │

│                    │  • /assets         │                       │

│                    │  • /findings       │                       │

│                    └──────────┬─────────┘                       │

│                               │                                  │

│                               ▼                                  │

│              ┌─────────────────────────────────┐               │

│              │   Cloud Monitoring Dashboard    │               │

│              │   • Asset Coverage              │               │

│              │   • Security Findings           │               │

│              │   • Control Validation          │               │

│              └─────────────────────────────────┘               │

│                                                                   │

└─────────────────────────────────────────────────────────────────┘

```



\### Four CAASM Pillars Implementation



1\. \*\*Discovery\*\* (Ingestor Service)

&nbsp;  - Ingests asset data from CSV files and APIs

&nbsp;  - Publishes to Pub/Sub for processing

&nbsp;  - Supports multiple source types



2\. \*\*Relationship Mapping\*\* (Normalizer Service)

&nbsp;  - Creates asset relationships in BigQuery

&nbsp;  - Normalizes data across sources

&nbsp;  - Builds dependency graphs



3\. \*\*Control Validation\*\* (Coverage Evaluator Service)

&nbsp;  - Evaluates security control coverage

&nbsp;  - Generates compliance findings

&nbsp;  - Tracks control effectiveness



4\. \*\*Automated Response\*\* (API Gateway + Cloud Functions)

&nbsp;  - Exposes REST APIs for automation

&nbsp;  - Provides metrics and findings

&nbsp;  - Enables integration with security tools



\### PCA Domain Mapping



| Component | PCA Domain | Percentage | Key Topics |

|-----------|------------|------------|------------|

| Terraform Infrastructure | Domain 1: Design \& Planning | 25% | Architecture design, resource planning |

| Cloud Run Services | Domain 2: Managing Infrastructure | 17.5% | Serverless compute, orchestration |

| IAM + Secret Manager | Domain 3: Security \& Compliance | 17.5% | Identity management, encryption |

| BigQuery Analytics | Domain 4: Technical Process Analysis | 15% | Data analysis, optimization |

| Cloud Build CI/CD | Domain 5: Managing Implementation | 12.5% | Deployment automation |

| Cloud Monitoring | Domain 6: Operations Excellence | 12.5% | Observability, alerting |



\### Prerequisites



\- Google Cloud account with billing enabled

\- `gcloud` CLI installed

\- Terraform >= 1.5.0

\- Python 3.11+

\- Git



\### Quick Start



```bash

\# 1. Clone the repository

git clone <repository-url>

cd cloud-caasm-gcp



\# 2. Set up environment

export PROJECT\_ID="your-gcp-project-id"

export REGION="us-central1"



gcloud config set project $PROJECT\_ID

gcloud services enable \\

&nbsp; run.googleapis.com \\

&nbsp; cloudbuild.googleapis.com \\

&nbsp; pubsub.googleapis.com \\

&nbsp; bigquery.googleapis.com \\

&nbsp; secretmanager.googleapis.com \\

&nbsp; monitoring.googleapis.com



\# 3. Deploy infrastructure

cd infra

terraform init

terraform plan -var="project\_id=$PROJECT\_ID" -var="region=$REGION"

terraform apply -var="project\_id=$PROJECT\_ID" -var="region=$REGION" -auto-approve



\# 4. Build and deploy services

cd ../services

./deploy.sh



\# 5. Load sample data

cd ../data

./load\_sample\_data.sh



\# 6. Verify deployment

cd ../scripts

./verify\_deployment.sh

```



\### Project Structure



```

cloud-caasm-gcp/

├── README.md                        # This file

├── infra/                           # Terraform infrastructure

│   ├── main.tf                      # Core infrastructure

│   ├── variables.tf                 # Input variables

│   ├── outputs.tf                   # Output values

│   ├── bigquery.tf                  # BigQuery datasets/tables

│   ├── pubsub.tf                    # Pub/Sub topics/subscriptions

│   ├── storage.tf                   # Cloud Storage buckets

│   ├── iam.tf                       # IAM roles and service accounts

│   └── monitoring.tf                # Cloud Monitoring resources

├── services/                        # Microservices

│   ├── ingestor/                    # Asset ingestion service

│   │   ├── main.py

│   │   ├── requirements.txt

│   │   ├── Dockerfile

│   │   └── cloudbuild.yaml

│   ├── normalizer/                  # Data normalization service

│   │   ├── main.py

│   │   ├── requirements.txt

│   │   ├── Dockerfile

│   │   └── cloudbuild.yaml

│   ├── coverage-evaluator/          # Coverage evaluation service

│   │   ├── main.py

│   │   ├── requirements.txt

│   │   ├── Dockerfile

│   │   └── cloudbuild.yaml

│   ├── api-gateway/                 # REST API gateway

│   │   ├── main.py

│   │   ├── requirements.txt

│   │   ├── Dockerfile

│   │   └── cloudbuild.yaml

│   └── deploy.sh                    # Deployment script

├── data/                            # Sample data and schemas

│   ├── sample\_assets.csv

│   ├── sample\_findings.csv

│   ├── bigquery\_schemas/

│   │   ├── assets\_inventory.json

│   │   ├── assets\_relationships.json

│   │   ├── coverage\_findings.json

│   │   └── coverage\_metrics.json

│   └── load\_sample\_data.sh

├── dashboards/                      # Cloud Monitoring dashboards

│   ├── caasm\_dashboard.json

│   ├── alert\_policies.json

│   └── README.md

├── governance/                      # Compliance documentation

│   ├── iso42001\_mapping.md

│   ├── pca\_domain\_mapping.md

│   └── security\_controls.md

├── docs/                            # Documentation

│   ├── architecture.md

│   ├── deployment\_guide.md

│   ├── api\_reference.md

│   └── sprint\_plan.md

└── scripts/                         # Utility scripts

&nbsp;   ├── verify\_deployment.sh

&nbsp;   ├── cleanup.sh

&nbsp;   └── cost\_estimate.sh

```



\### Cost Estimation (Free Tier)



All services stay within Google Cloud's Always Free tier:



\- \*\*Cloud Run\*\*: 2M requests/month free

\- \*\*Pub/Sub\*\*: 10GB messages/month free

\- \*\*BigQuery\*\*: 1TB queries/month, 10GB storage free

\- \*\*Cloud Storage\*\*: 5GB standard storage free

\- \*\*Cloud Build\*\*: 120 build-minutes/day free

\- \*\*Secret Manager\*\*: 6 secrets free

\- \*\*Cloud Monitoring\*\*: Free tier metrics



\*\*Estimated monthly cost: $0.00\*\* (within free tier limits)



\### API Endpoints



```bash

\# Get API Gateway URL

export API\_URL=$(gcloud run services describe api-gateway --region=us-central1 --format='value(status.url)')



\# Fetch metrics

curl -X GET "$API\_URL/metrics" \\

&nbsp; -H "Authorization: Bearer $(gcloud auth print-identity-token)"



\# List assets

curl -X GET "$API\_URL/assets?limit=10" \\

&nbsp; -H "Authorization: Bearer $(gcloud auth print-identity-token)"



\# Get findings

curl -X GET "$API\_URL/findings?severity=HIGH" \\

&nbsp; -H "Authorization: Bearer $(gcloud auth print-identity-token)"

```



\### Monitoring Dashboard



Access the Cloud Monitoring dashboard:



```bash

\# Import dashboard

gcloud monitoring dashboards create --config-from-file=dashboards/caasm\_dashboard.json

```



View in Console: https://console.cloud.google.com/monitoring/dashboards



\### 6-Week Sprint Plan



Detailed sprint breakdown available in `docs/sprint\_plan.md`:



\- \*\*Sprint 1\*\*: Infrastructure \& Design (Domain 1)

\- \*\*Sprint 2\*\*: Service Deployment (Domain 2)

\- \*\*Sprint 3\*\*: Security Implementation (Domain 3)

\- \*\*Sprint 4\*\*: Analytics \& Optimization (Domain 4)

\- \*\*Sprint 5\*\*: CI/CD Pipeline (Domain 5)

\- \*\*Sprint 6\*\*: Observability \& Operations (Domain 6)



\### Testing



```bash

\# Run unit tests

cd services

pytest tests/



\# Run integration tests

./scripts/integration\_test.sh



\# Load test

./scripts/load\_test.sh

```



\### Cleanup



```bash

\# Remove all resources

cd infra

terraform destroy -var="project\_id=$PROJECT\_ID" -var="region=$REGION" -auto-approve



\# Or use cleanup script

cd ../scripts

./cleanup.sh

```



\### Troubleshooting



See `docs/troubleshooting.md` for common issues and solutions.



\### Contributing



See `CONTRIBUTING.md` for contribution guidelines.



\### License



MIT License - See `LICENSE` file



\### Support



\- GitHub Issues: <repository-url>/issues

\- Documentation: `docs/`

\- PCA Exam Guide: See uploaded PDF reference



\### References



\- \[Google Cloud Well-Architected Framework](https://cloud.google.com/architecture/framework)

\- \[Professional Cloud Architect Exam Guide](https://cloud.google.com/certification/cloud-architect)

\- \[ISO 42001 AI Management System](https://www.iso.org/standard/81230.html)

\- \[CAASM Best Practices](https://www.linkedin.com/learning/cyber-asset-management-securing-digital-resources-in-the-modern-enterprise)

