# AWS EKS with Terraform

Production-ready Amazon EKS cluster provisioned with Terraform — including VPC, node groups, IAM roles, and remote state backend.

---

## Architecture

```
                        ┌─────────────────────────────────┐
                        │            AWS Region            │
                        │                                  │
                        │   ┌─────────────────────────┐   │
                        │   │          VPC             │   │
                        │   │                          │   │
                        │   │  ┌─────────┐ ┌────────┐ │   │
                        │   │  │Public   │ │Private │ │   │
                        │   │  │Subnets  │ │Subnets │ │   │
                        │   │  └────┬────┘ └───┬────┘ │   │
                        │   │       │           │      │   │
                        │   │  ┌────▼───────────▼────┐ │   │
                        │   │  │      EKS Cluster     │ │   │
                        │   │  │  ┌───────────────┐   │ │   │
                        │   │  │  │  Node Group   │   │ │   │
                        │   │  │  │  (EC2 x 2-4)  │   │ │   │
                        │   │  │  └───────────────┘   │ │   │
                        │   │  └─────────────────────┘ │   │
                        │   └─────────────────────────┘   │
                        └─────────────────────────────────┘
```

## Tools Used

- **Terraform** — Infrastructure as Code
- **AWS EKS** — Managed Kubernetes
- **AWS VPC** — Networking
- **AWS IAM** — Roles and policies
- **S3 + DynamoDB** — Remote state backend

## Project Structure

```
aws-eks-terraform/
├── modules/
│   ├── vpc/
│   │   ├── main.tf
│   │   ├── variables.tf
│   │   └── outputs.tf
│   ├── eks/
│   │   ├── main.tf
│   │   ├── variables.tf
│   │   └── outputs.tf
│   └── iam/
│       ├── main.tf
│       └── outputs.tf
├── main.tf
├── variables.tf
├── outputs.tf
├── backend.tf
└── terraform.tfvars.example
```

## Prerequisites

- AWS CLI configured (`aws configure`)
- Terraform >= 1.5
- kubectl installed
- Sufficient IAM permissions (EKS, VPC, IAM)

## Setup Instructions

```bash
# 1. Clone the repo
git clone https://github.com/deploydockz/aws-eks-terraform
cd aws-eks-terraform

# 2. Copy and edit variables
cp terraform.tfvars.example terraform.tfvars
# Edit terraform.tfvars with your values

# 3. Initialize Terraform
terraform init

# 4. Plan
terraform plan

# 5. Apply
terraform apply
```

## Deployment Steps

```bash
# After apply, configure kubectl
aws eks update-kubeconfig --name my-eks-cluster --region us-east-1

# Verify nodes
kubectl get nodes
```

## Expected Output

```
NAME                          STATUS   ROLES    AGE   VERSION
ip-10-0-1-10.ec2.internal     Ready    <none>   2m    v1.29.x
ip-10-0-2-15.ec2.internal     Ready    <none>   2m    v1.29.x
```

## Future Improvements

- Add Cluster Autoscaler
- Add AWS Load Balancer Controller
- Add IRSA (IAM Roles for Service Accounts)
- Add Karpenter for node provisioning
