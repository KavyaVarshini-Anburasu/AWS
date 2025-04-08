# Building Security in the Cloud with AWS Write-Up

## *“Security in the cloud isn’t magic — it’s methodical.”*

I completed a 4-phase project called **Cloud Security Builder** using AWS. Each phase simulated real-world security challenges ranging from data protection to VPC-level control, encryption, monitoring, and automated compliance. Here’s how I secured the cloud, step by step.

## Phase 1: Locking Down S3 Buckets
Objective: Secure data in S3 and monitor access
### Key Steps:
- Configured a private **data-bucket** with fine-grained **IAM policies**
- Enabled **versioning** and **object-level logging**
- Activated **S3 Inventory** to track metadata
- Used **Athena** to query S3 access logs and validate authorization/denial
**Result:** Unauthorized users (like *mary*) were blocked, while valid access (e.g., *paulo*) succeeded and was logged. Versioning worked across uploads. Athena confirmed secure access and visibility.

## Phase 2: Network Security with VPC & Firewall
Objective: Create layered defenses in AWS networking
### Key Steps:
- Reviewed VPC flow and created **VPC Flow Logs**
- Secured ports with **Security Groups** and **Network ACLs**
- Created and deployed **AWS Network Firewall** with route tables
- Set up **CloudWatch** for logging dropped packets
- Created **stateful rule groups** to allow ports 22, 80, 443, and ICMP, while denying 8080
**Result:** Confirmed secure HTTP/SSH access and saw blocked port 8080 traffic logged in CloudWatch — proving effective firewall policies.

---

## Phase 3: End-to-End Encryption with AWS KMS
Objective: Protect data at rest using AWS KMS
### Key Steps:
- Created **MyKMSKey** with **auto key rotation**
- Updated **key policies** to authorize specific IAM users (e.g., *sofia*)
- Encrypted S3 uploads with **SSE-KMS**
- Encrypted EC2 root volume during instance creation
- Performed **envelope encryption** using AWS CLI + OpenSSL on a live instance
**Result:** Only authorized users could upload/read encrypted content. EC2 instance volumes and custom data were secured using proper key scopes and least-privilege policies.

---

## Phase 4: Real-Time Monitoring and Compliance
Objective: Establish audit, alert, and remediation pipelines
### Key Steps:
- Used **CloudTrail** to track S3 API activity
- Installed **CloudWatch Agent** on EC2 to capture SSH login attempts
- Created **CloudWatch Alarms** for successful SSH access
- Deployed **AWS Config** with `s3-bucket-logging-enabled` rule
- Used **Systems Manager** to automate remediation for non-compliant buckets
**Result:** A fully observable system that logs events, detects misconfigurations, and enforces compliance automatically.

---

## 🔄 Challenges Faced

|  Problem |  Fix |
|-----------|--------|
| IAM permission conflicts | Revised trust & access policies |
| KMS access denial | Adjusted principal permissions in the key policy |
| CloudWatch log filters not triggering | Fine-tuned log pattern matching |
| Config compliance issues | Used Systems Manager for auto-remediation |

---

## Key Takeaways

- **Defense in Depth** works best with **visibility + control**
- KMS, when combined with IAM policies, can enforce powerful encryption workflows
- Logging without alarms = noise; automated response is key
- S3 versioning + Athena = cloud-native forensics

---

This project made me appreciate how intricate yet modular AWS security can be. With each layer, whether it’s IAM, firewall rules, encryption, or monitoring — we get closer to a **zero-trust, resilient cloud environment**.
Feel free to clone this structure, fork the approach, or ping me if you’re building something similar!
