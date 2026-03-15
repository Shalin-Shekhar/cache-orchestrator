# Terraform

Cloud infrastructure definitions for Cache Orchestrator environments.

## Structure
```
terraform/
├── modules/
│   ├── vpc/           ← Network topology
│   ├── rds/           ← Postgres (Model Hub metadata)
│   ├── clickhouse/    ← Analytics time-series store
│   └── eks/           ← Kubernetes cluster (control plane)
├── envs/
│   ├── dev/
│   ├── staging/
│   └── prod/
```

## Usage
```bash
cd infra/terraform/envs/staging
terraform init && terraform plan
```