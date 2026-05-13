# AWS Assignments Portfolio

This repository contains four hands‑on AWS projects that progressively build core cloud skills: networking, high availability, static hosting, and serverless APIs. Each assignment follows production‑grade practices and least‑privilege IAM protocols

## 🗺️ Assignments Overview

| Assignment | Focus Area | Key AWS Services | Production Concepts |
|------------|------------|------------------|----------------------|
| **#1** | Custom VPC with public & private subnets | VPC, EC2, IGW, NAT Gateway, Bastion, CloudWatch | Network segmentation, least‑privilege security groups, outbound NAT |
| **#2** | Load balancing & auto scaling | ALB, Auto Scaling Groups, Launch Templates, EC2 | High availability, health checks, dynamic scaling |
| **#3** | Static website hosting | S3, CloudFront, ACM | CDN, custom domain, SSL/TLS, origin access control |
| **#4** | Serverless REST API | API Gateway, Lambda, DynamoDB, IAM, WAF, ACM | Least‑privilege IAM, rate limiting, API keys, custom domain, CloudWatch logs |

---

## Assignment 1 – Custom VPC with Public & Private Subnets

**Goal:** Build a secure, two‑tier VPC from scratch with public and private subnets, a bastion host, and a private EC2 instance that can access the internet via a NAT Gateway.

**Components:**
- VPC `10.0.0.0/16`
- Public subnet `10.0.0.0/24` with Internet Gateway
- Private subnet `10.0.1.0/24`
- NAT Gateway (public subnet) with Elastic IP
- Route tables: public → IGW, private → NAT
- Security groups: public SG allows SSH only from home IP; private SG allows SSH only from bastion SG
- Bastion host (public EC2)
- Private EC2 (no public IP)
- CloudWatch monitoring + agent for memory metrics


📂 *See `Assignment 1/README.md` for step‑by‑step, screenshots, and challenges.*

---

## Assignment 2 – Load Balancer + Auto Scaling Group (ASG)

**Goal:** Deploy a highly available web application using an Application Load Balancer (ALB) and an Auto Scaling Group that launches EC2 instances across multiple Availability Zones.

**Components:**
- Launch Template with Amazon Linux 2023, user‑data installing a simple web server
- Auto Scaling Group (min 2, max 4, desired 2) across two AZs
- Application Load Balancer (internet‑facing) with listener on port 80
- Target group registered to ASG
- Security groups: ALB allows HTTP from anywhere; instances allow HTTP only from ALB
- CloudWatch alarms for scaling policies (e.g., CPU > 70%)


📂 *See `Assignment 2/README.md` for details.*

---

## Assignment 3 – Static Website Hosting with S3 + CloudFront + Cloudflare

**Goal:** Host a static website (HTML/CSS/JS) on Amazon S3, serve it globally via CloudFront CDN, and attach a custom domain with HTTPS using ACM and Cloudflare.

**Components:**
- S3 bucket for website content (publicly readable, static hosting enabled)
- CloudFront distribution with S3 as origin, custom error pages, cache policy
- ACM certificate for custom domain (e.g., `www.yourdomain.com`)
- Cloudflare – A record (alias) pointing to CloudFront
- Origin Access Control (OAC) to restrict S3 bucket access to CloudFront only


📂 *See `Assignment 3/README.md` for full steps.*

---

## Assignment 4 – Serverless API with API Gateway, Lambda, DynamoDB 

**Goal:** Build a serverless REST API that accepts POST requests, stores data in DynamoDB, and retrieves it via GET – following least‑privilege IAM and adding production features.

**Components:**
- DynamoDB `students` table (partition key `id`, on‑demand)
- Lambda (Python) with UUID generation, timestamp, DynamoDB `PutItem` (POST) and `Scan` (GET)
- API Gateway REST API: `POST /submit`, `GET /` with Lambda proxy integration, CORS
- IAM role: only `dynamodb:PutItem`, `dynamodb:Scan`, and CloudWatch logs
- Bonus: API key + usage plan (5 req/sec, burst 10, 1000/day)
- Bonus: AWS WAF rate‑based rule (2000/5min per IP) attached to API stage
- Bonus: Custom domain via ACM (`api.zain-amer.co.uk`) and Cloudflare DNS


📂 *See `Assignment 4/README.md` for code, IAM policy, and troubleshooting.*

---

## 🧠 Common Lessons Across All Assignments

- **Least‑privilege IAM** is essential. Always scope actions (`PutItem`, `Scan`) to specific resources.
- **Document as you build**. Screenshots and a step‑by‑step README would of saved me hours of debugging later.
- **Draw the architecture first**. A diagram reveals missing components (e.g., NAT Gateway, route table associations) before you touch the console.
- **DNS and SSL take patience**. Custom domains require ACM validation, correct CNAME records, and waiting for propagation. Cloudflare proxies often need to be disabled for API Gateway.


## 🛠️ Technologies Used Across All Assignments

- **AWS**: VPC, EC2, IGW, NAT Gateway, ALB, ASG, S3, CloudFront, Route53, ACM, WAF, API Gateway, Lambda, DynamoDB, CloudWatch, IAM
- **Local tools**: AWS CLI, `curl`, OpenSSL, Git, draw.io
- **Operating Systems**: Ubuntu 
- **DNS providers**: Cloudflare 


Each subfolder contains its own detailed `README.md`, architecture diagram, and a `Screenshots/` directory with evidence of the main steps.

---

*This portfolio demonstrates a journey from foundational networking to modern serverless architectures – all following AWS Well‑Architected principles.*