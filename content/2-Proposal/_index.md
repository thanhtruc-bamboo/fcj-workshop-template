---
title: "Proposal"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

# WORKSHOP SOLUTION PROPOSAL: BUILDING AN AUTOMATED SERVERLESS DATA PIPELINE USING AWS SERVICES

---

### Part 1: Problem Overview & Proposed Solution
In the era of Cloud Computing and Big Data, automated ingestion and cleansing of incoming data play a critical role in enterprise operations. The challenge is to construct an automated, highly available, secure, and cost-optimized data processing pipeline without managing physical servers or complex infrastructure.

**Proposed Solution:** Build a **Serverless Data Pipeline** on Amazon Web Services (AWS). The system automatically triggers data processing whenever a user or external service uploads raw datasets (CSV/JSON) to storage, cleanses and normalizes the data, saves the output to a target destination, and logs execution details.

---

### Part 2: Objectives & Project Scope

#### Objectives
- Successfully implement an automated data processing pipeline using a **Serverless & Event-Driven** architecture.
- Strictly enforce the **Least Privilege** principle under the Security Pillar of the AWS Well-Architected Framework.
- Monitor and audit the complete data processing flow via centralized logging and automated alerts.
- Produce a reproducible, step-by-step technical lab guide for the community.

#### Project Scope
- **In-Scope:**
  - Create and configure 2 Amazon S3 Buckets (`raw-data-bucket` and `processed-data-bucket`).
  - Configure IAM Roles and Policies with least-privilege permissions for AWS Lambda.
  - Develop an AWS Lambda function (Python/Node.js) for data processing and cleansing.
  - Configure Amazon S3 Event Notifications to automatically trigger Lambda execution.
  - Integrate Amazon CloudWatch Logs and establish CloudWatch Alarms with Amazon SNS notifications on failure.
  - Build a detailed cost estimation using the **AWS Pricing Calculator** and write a resource clean-up guide.
- **Out-of-Scope:**
  - Deploying complex Machine Learning models on the data.
  - Building complex Frontend user interfaces.

---

### Part 3: Key Technical Issues to Solve

1. **Manual Bottlenecks & Latency:** Traditional data processing relies on scheduled cron jobs or manual intervention, causing significant latency and human error.
2. **Infrastructure Overhead:** Running 24/7 EC2 server instances leads to unnecessary financial costs during idle periods with fluctuating data traffic.
3. **Security Risks:** Over-privileged IAM policies introduce security vulnerabilities and potential sensitive data leaks.
4. **Lack of Observability:** Absence of centralized monitoring makes it difficult to track logs and detect pipeline failures in real time.

---
### Part 4: Implementation Roadmap & Timeline

The project is structured into phases spanning **Week 1 through Week 12**:

| Week | Tasks Description | Expected Deliverables |
| :---: | :--- | :--- |
| **Weeks 1 - 2** | Explore AWS CLI, set up AWS account, create S3 Buckets for raw and processed data. | AWS account configured, 2 operational S3 Buckets. |
| **Weeks 3 - 4** | Configure IAM Roles and Policies using Least Privilege between Lambda, S3, and CloudWatch. | IAM Roles created with strict security policies. |
| **Weeks 5 - 6** | Write Lambda transformation code and configure S3 Event Notifications. | Complete Lambda codebase, automated event triggers functional. |
| **Weeks 7 - 8** | Integrate Amazon CloudWatch Logs, setup custom Metrics and SNS Alarms. | Automated log ingestion and failure alerts configured. |
| **Weeks 9 - 10** | Perform End-to-End System Testing, optimize Lambda execution duration and memory. | Test report generated, system operating stably. |
| **Weeks 11 - 12**| Finalize bilingual technical documentation, cost estimation, demo video/link, and clean-up guide. | Published Workshop website on GitHub Pages. |

---

### Part 5: Budget & Cost Estimation

Estimated using the **AWS Pricing Calculator** based on an assumed workload of 10,000 files/month:

| AWS Service | Estimated Usage | Monthly Cost (USD) |
| :--- | :--- | :---: |
| **Amazon S3** | 5 GB Storage (Standard Class) + 10,000 PUT/GET Requests | $0.00 (AWS Free Tier) / ~$0.15 |
| **AWS Lambda** | 10,000 Executions (~500ms duration, 128MB Memory) | $0.00 (1M Free Requests/month) |
| **Amazon CloudWatch** | 1 GB Log Ingestion + 1 Alarm | $0.00 (AWS Free Tier) / ~$0.50 |
| **Amazon SNS** | < 1,000 Email Notifications | $0.00 (1,000 Free Emails) |
| **Total Estimated Cost** | **Approx. $0.00 / month (Utilizing AWS Free Tier)** | **~$0.65 / month (Post Free Tier)** |

> ⚠️ **Note:** The system is optimized on serverless infrastructure, resulting in near $0 USD costs for testing and evaluation.

---

### Part 6: Risks & Mitigation Strategies

| Identified Risk | Severity | Mitigation Strategy |
| :--- | :---: | :--- |
| **Cost Overrun** | Medium | - Set up AWS Budgets to alert via email if expenses exceed $1.<br>- Provide a step-by-step Clean-up Guide to destroy resources after testing. |
| **Credential Leakage** | High | - Never hard-code AWS Access Keys/Secret Keys in GitHub source code.<br>- Enforce IAM Least Privilege and enable MFA on the AWS account. |
| **Infinite Event Loop** | High | - Ensure Lambda does not output processed files into the same source S3 Bucket triggering the function. |
| **Invalid Input Data Format** | Low | - Implement robust try-catch exception handling in Lambda code to log errors to CloudWatch. |

---

### References

- [AWS Well-Architected Framework](https://aws.amazon.com/architecture/well-architected/)
- [Amazon S3 Developer Guide](https://docs.aws.amazon.com/s3/)
- [AWS Lambda Developer Guide](https://docs.aws.amazon.com/lambda/)
- [AWS Pricing Calculator](https://calculator.aws/)
- [Workshop Regulations - HCM Rules FCAJ](https://hcm-rules.awsfcaj.com/3-project/#23-proposal-đề-xuất-giải-pháp-workshop)
---
### Part 7: System Architecture Diagram

The system architecture consists of the following primary components:

```text
[User / System] 
      │
      ▼ (Upload Raw File)
[Amazon S3: Raw Bucket] ──(S3 Event Trigger)──► [AWS Lambda Function]
                                                         │
                                        ┌────────────────┴────────────────┐
                                        ▼                                 ▼
                          [Amazon S3: Processed Bucket]       [Amazon CloudWatch Logs]
                                                                          │
                                                                          ▼ (If Error)
                                                                 [Amazon SNS Alert]