[![Terraform](https://img.shields.io/badge/Terraform-≥1.5-blue?logo=terraform)](https://www.terraform.io/)
[![AWS](https://img.shields.io/badge/AWS-Cloud%20Intelligence-orange?logo=amazon-aws&logoColor=white)](https://aws.amazon.com/solutions/cudos/)
[![QuickSight](https://img.shields.io/badge/Dashboards-QuickSight-232F3E?logo=amazon-aws)](https://aws.amazon.com/quicksight/)
[![Athena](https://img.shields.io/badge/Data-Athena-0073bb?logo=amazon-aws)](https://aws.amazon.com/athena/)
[![Glue](https://img.shields.io/badge/ETL-Glue-7D3C98?logo=amazon-aws)](https://aws.amazon.com/glue/)
[![FinOps](https://img.shields.io/badge/Focus-FinOps-blue)](https://www.finops.org/)
[![CI](https://img.shields.io/github/actions/workflow/status/lauj0rge/terraform-aws-cudos/ci.yml?label=CI%20Build)](https://github.com/lauj0rge/terraform-aws-cudos/actions)


# terraform-aws-cudos

A Terraform module that deploys the **AWS Cloud Intelligence Dashboards (CUDOS)** framework — a collection of **AWS QuickSight dashboards** that visualize cost, usage, and operational insights across your AWS environment.

---

## Overview

This module automates deployment of the **Cloud Intelligence Dashboards (CID)** solution, including:

* **CUDOS Dashboard** – overall AWS cost and usage insights
* **Cost Intelligence Dashboard** – breakdown of spend by service and account
* **KPI Dashboard** – high-level financial and operational metrics
* **Compute Optimizer Dashboard** – cost efficiency and right-sizing recommendations

The dashboards are built on **AWS QuickSight** and query **Athena views** over your **Cost and Usage Reports (CUR)**.

---

## Example Usage

```hcl
module "cudos_framework" {
  source = "github.com/lauj0rge/terraform-aws-cudos"

  dashboard_bucket_name              = "my-cudos-bucket"
  enable_compute_optimizer_dashboard = true
  enable_cost_intelligence_dashboard = true
  enable_cudos_dashboard             = true
  enable_kpi_dashboard               = true
  enable_sso                         = true
  saml_metadata                      = file("${path.module}/assets/saml-metadata.xml")
  quicksight_dashboard_owner         = "myuser@example.com"

  providers = {
    aws.management              = aws.management
    aws.management_us_east_1    = aws.management_us_east_1
    aws.cost_analysis           = aws.cost_analysis
    aws.cost_analysis_us_east_1 = aws.cost_analysis_us_east_1
  }
}
```

---

## Architecture

```
AWS CUR (S3)
     ↓
AWS Glue → Athena → QuickSight Dashboards
```

* **S3:** stores the Cost and Usage Reports (CUR)
* **Glue:** creates tables and metadata for Athena
* **Athena:** queries cost and usage data
* **QuickSight:** visualizes cost, usage, and efficiency trends

---

## Requirements

* AWS CUR 2.0 configured and accessible
* AWS QuickSight Enterprise edition enabled
* Terraform ≥ 1.5
* IAM permissions to deploy CloudFormation and QuickSight resources

---

## Deployment Steps

1. Configure your CUR and Athena views.
2. Run:

   ```bash
   terraform init
   terraform apply
   ```
3. Open **AWS QuickSight → Dashboards** to view CUDOS dashboards.

---

## Learn More

* [Cloud Intelligence Dashboards (CID)](https://github.com/awslabs/cid-framework)
* [CUDOS Framework](https://aws.amazon.com/solutions/cudos/)
* [QuickSight Pricing](https://aws.amazon.com/quicksight/pricing/)



