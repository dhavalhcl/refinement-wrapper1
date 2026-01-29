# IaC Deployment Workflows (Azure & GCP)

This repository uses **separate, simplified GitHub Actions** for Azure and GCP to deploy **dev / qa / prod** without any PR requirements.

## Workflows

- `.github/workflows/azure-deploy.yml` – Manual **plan/apply** for Azure.
- `.github/workflows/gcp-deploy.yml` – Manual **plan/apply** for GCP.
- `.github/workflows/trufflehog-scan.yml` – Background secret scanning (non-blocking).
- `.github/workflows/terratest.yml` – Nightly/unit test style infra tests (optional, non-blocking).

> By default, only **manual** (`workflow_dispatch`) runs are enabled to avoid accidental triggers. If you want automatic deploys on code changes, first segregate paths to `tf/azure/environments/<env>` and `tf/gcp/environments/<env>`, then uncomment the `push:` blocks.

## Directory layout

Recommended split by cloud provider:

```
 tf/
   azure/
     environments/
       dev/   # provider.tf uses azurerm
       qa/
       prod/
   gcp/
     environments/
       dev/   # provider.tf uses google
       qa/
       prod/
```

## Required secrets

### Azure (repository or environment secrets)
- `AZURE_CLIENT_ID`
- `AZURE_TENANT_ID`
- `AZURE_SUBSCRIPTION_ID`
- *(optional)* `AZURE_CREDENTIALS_DEV` for SPN fallback

### GCP
- `GCP_WORKLOAD_IDENTITY_PROVIDER`
- `GCP_SERVICE_ACCOUNT`
- `GCP_PROJECT_ID`

## How to run

1. Go to **Actions** → **Azure Terraform Deploy** (or **GCP Terraform Deploy**).
2. Click **Run workflow** → choose `environment` (dev/qa/prod) and `action` (plan/apply).
3. Download the **Plan** artifact for review if needed; apply when ready.

## Decommission the old pipeline

Remove the legacy combined workflow (e.g., `terraform-deploy.yml`) to avoid duplicate runs and confusion.
