# ☁️ AWS Cloud Architecture – Scalable Digital Media Platform

A production-ready **AWS cloud infrastructure** design for a scalable digital media platform — built with security, availability and cost efficiency as core requirements from day one.

---

## 📌 What It Does

This project designs and documents a full AWS infrastructure capable of hosting a digital media platform at scale, including:

- Serving and storing media files reliably to large numbers of concurrent users
- Securing access at every layer using IAM and network controls
- Monitoring system health and performance in real time via CloudWatch
- Controlling and optimising cloud spend with AWS Cost Explorer
- Surviving availability zone failures without downtime

---

## 🛠️ Tech Stack

| Service | Purpose |
|---|---|
| **Amazon EC2** | Compute — application and web servers |
| **Amazon S3** | Object storage — media file storage and delivery |
| **Amazon VPC** | Network — isolated, segmented cloud environment |
| **AWS IAM** | Identity — least-privilege access control |
| **Amazon CloudWatch** | Observability — metrics, alarms and logging |
| **AWS Cost Explorer** | FinOps — spend tracking and optimisation |
| **Elastic Load Balancer** | Traffic distribution across availability zones |
| **Auto Scaling Group** | Dynamic compute scaling under load |

---

## 🏗️ Architecture Overview

```
                        Internet
                           │
                    ┌──────▼──────┐
                    │  CloudFront  │  ← CDN / edge caching
                    └──────┬──────┘
                           │
                    ┌──────▼──────┐
                    │     ELB     │  ← Load balancer (public subnet)
                    └──────┬──────┘
                           │
          ┌────────────────┼────────────────┐
          │                │                │
   ┌──────▼──────┐  ┌──────▼──────┐        │
   │  EC2 (AZ-1) │  │  EC2 (AZ-2) │        │  ← Auto Scaling Group
   └──────┬──────┘  └──────┬──────┘        │    (private subnets)
          │                │               ─┘
          └────────┬───────┘
                   │
            ┌──────▼──────┐
            │   Amazon S3  │  ← Media storage (private, bucket policy)
            └─────────────┘

   CloudWatch ──── monitors all layers ────▶ Alarms & Dashboards
   IAM        ──── controls all access ────▶ Roles, Policies, MFA
   Cost Explorer ─ tracks all spend ──────▶ Budgets & Alerts
```

---

## 🔐 Security Design

Security is applied at every layer — not bolted on at the end.

**Network segmentation:**
- Public subnets: load balancer only
- Private subnets: EC2 instances — no direct internet access
- S3 bucket: no public read; accessed via IAM roles only

**IAM (Identity & Access Management):**
- Least-privilege principle throughout — every role has only the permissions it needs
- EC2 instances use instance profiles — no access keys stored on servers
- MFA enforced for all console access
- S3 bucket policies restrict access to authorised roles only

**In transit & at rest:**
- HTTPS enforced at the load balancer (ACM certificate)
- S3 server-side encryption enabled (SSE-S3)
- CloudWatch log data encrypted

---

## 📊 Observability

CloudWatch is configured to monitor:

| Metric | Alarm Threshold |
|---|---|
| EC2 CPU Utilisation | > 80% for 5 mins → scale out |
| ELB 5XX Error Rate | > 1% → alert |
| S3 Request Errors | > 0.5% → alert |
| Estimated Charges | > budget threshold → alert |

All alarms route to SNS for email/SMS notification.

---

## 💰 Cost Optimisation

AWS Cost Explorer was used throughout to:

- Identify the most cost-effective EC2 instance types for the expected workload
- Set budget alerts to prevent unexpected spend
- Analyse S3 storage class options (Standard vs Infrequent Access)
- Model reserved instance savings vs on-demand pricing

---

## 🚀 Getting Started

### Prerequisites

- AWS account with appropriate IAM permissions
- AWS CLI installed and configured (`aws configure`)
- Terraform installed (if deploying via IaC)

### Deploying the Infrastructure

```bash
git clone https://github.com/YOUR-USERNAME/aws-cloud-architecture.git
cd aws-cloud-architecture

# Review and update variables
cp terraform.tfvars.example terraform.tfvars

# Initialise and deploy
terraform init
terraform plan
terraform apply
```

---

## 📂 Project Structure

```
aws-cloud-architecture/
│
├── architecture/
│   └── diagram.png         # Full architecture diagram
├── terraform/
│   ├── main.tf             # Core infrastructure definition
│   ├── vpc.tf              # VPC, subnets, routing
│   ├── ec2.tf              # Compute and auto scaling
│   ├── s3.tf               # Storage and bucket policies
│   ├── iam.tf              # Roles, policies, instance profiles
│   ├── cloudwatch.tf       # Metrics, alarms, dashboards
│   └── variables.tf        # Configurable parameters
├── terraform.tfvars.example
└── README.md
```

---

## 📈 Future Improvements

- [ ] Add Amazon RDS (managed database) with Multi-AZ failover
- [ ] Integrate AWS WAF for application-layer protection
- [ ] Add CloudFront distribution for global media delivery
- [ ] Implement S3 lifecycle policies to move old media to Glacier
- [ ] Add VPC Flow Logs for network traffic auditing

---

## 💡 Why This Project

Designing infrastructure for a media platform means handling real trade-offs: availability vs cost, security vs convenience, flexibility vs simplicity. This project was built to practise making those decisions deliberately — choosing each service for a reason and documenting the thinking behind it.

---

## 👤 Author

**Uriel Djantou Fanja**
📧 urieldjantou@gmail.com
🔗 [GitHub](https://github.com/YOUR-USERNAME)

---

## 📄 Licence

MIT — free to use, modify and distribute.


<img width="412" height="227" alt="08_query_staff_manager" src="https://github.com/user-attachments/assets/4e58c057-dc92-4455-b609-ca35206ec070" /><img width="519" height="579" alt="03_furniture_table" src="https://github.com/user-attachments/assets/42a31b44-5dab-41ea-ab37-9ed286b69140" />
<img width="524" height="182" alt="09_query_furniture_by_branch" src="https://github.com/user-attachments/assets/bdf84916-9580-4b31-885f-7fc8b11dc973" />
<img width="519" height="336" alt="05_rentals_table" src="https://github.com/user-attachments/assets/e441eb0c-594c-4b09-b6dc-259d0e8d25ad" />
<img width="686" height="374" alt="04_members_table" src="https://github.com/user-attachments/assets/5e495ecd-be9a-42a9-97ef-d42675784fb0" />
<img width="787" height="789" alt="06_staff_table" src="https://github.com/user-attachments/assets/88c186d0-ed9f-4782-8142-f49c4a818b7f" />
<img width="528" height="602" alt="12_query_decode" src="https://github.com/user-attachments/assets/45ad63af-9537-4d4d-b25a-f1aca0992ef9" />
<img width="628" height="399" alt="11_query_rank" src="https://github.com/user-attachments/assets/4aa1d7e5-790f-4a16-b612-a8a7825c3d1b" />
<img width="524" height="317" alt="01_relational_model" src="https://github.com/user-attachments/assets/436030b7-7286-445e-96a9-f3d1c65d6c19" />
<img width="623" height="273" alt="13_query_decode_results" src="https://github.com/user-attachments/assets/8e41436a-f29b-491e-b353-88436e77e51a" />
<img width="673" height="514" alt="02_branch_table" src="https://github.com/user-attachments/assets/86a6b371-738d-43ea-ac48-1b10bfbd5d96" />
<img width="641" height="563" alt="14_query_rental_price_results" src="https://github.com/user-attachments/assets/1601f114-adb1-4f49-bc06-4d09ea08716f" />



![07_query_rental_price](https://github.com/user-attachments/assets/68bb2b76-fca8-4d97-97de-68a4b3cabf91)
