# AWS Security ↔ GCP Security — Concept Mapping

> For AWS Security Specialty holders studying for GCP Professional Cloud Security Engineer.
> This maps AWS services you already know to their GCP equivalents, and flags where the models differ significantly.

---

## Identity & Access Management

| AWS | GCP | Key Differences |
|-----|-----|-----------------|
| IAM Users | Google Cloud Identity accounts | GCP doesn't have "IAM users" — identities come from Cloud Identity or Google Workspace, not from the cloud platform itself |
| IAM Policies (JSON docs) | IAM Policy Bindings | **Major difference.** AWS attaches policy documents to principals. GCP binds members to roles *on* resources. There's no equivalent of an inline or managed policy you attach to a user |
| IAM Roles (assume role) | IAM Roles (grant to member) | AWS roles are assumed via STS. GCP roles are directly bound to members on resources — no "assume role" step. Service account impersonation is the closest equivalent |
| STS (temporary credentials) | Short-lived credentials / Workload Identity Federation | Same concept. GCP uses OAuth2 tokens, JWTs. Workload Identity Federation is like AWS IRSA / AssumeRoleWithWebIdentity |
| SCPs (Service Control Policies) | Organization Policies | **Different mechanism.** SCPs are IAM-style allow/deny policies. GCP Org Policies are constraint-based (boolean or list constraints on specific behaviors). They don't grant or deny IAM permissions — they restrict *what can be configured* |
| AWS Organizations OUs | GCP Folders | Same concept — organizational grouping. GCP folders can nest arbitrarily |
| Permission Boundaries | IAM Deny Policies | Both set an upper bound on what a principal can do. GCP Deny Policies are relatively new and work alongside allow policies |
| AWS SSO / Identity Center | Cloud Identity + Workforce Identity Federation | Cloud Identity is the directory. Workforce Identity Federation lets external IdPs (Okta, Azure AD) authenticate directly without syncing users |
| AWS Directory Service / AD Connector | Google Cloud Directory Sync (GCDS) | GCDS is a one-way sync tool from on-prem AD to Cloud Identity. It's not a managed AD service |
| Cognito | Identity Platform | For app-level authentication (not admin/infra). Identity Platform supports OIDC, SAML, email/password, phone auth |
| Resource-based policies | IAM bindings on resources | Similar concept. In GCP you set IAM policy on a resource (bucket, dataset, etc.) to grant access to external principals |

### What Will Feel Different
- In AWS you think: "What policies are attached to this role/user?" In GCP you think: "What bindings exist on this resource?"
- GCP has no concept of "policy documents" — it's always `member + role + resource`
- IAM Conditions in GCP work like AWS IAM Conditions but are set on the binding, not in a policy document
- **Privileged Access Manager (PAM)** — no direct AWS equivalent. Provides just-in-time temporary elevation. Closest AWS pattern is a custom solution with STS + Step Functions

---

## Encryption & Key Management

| AWS | GCP | Key Differences |
|-----|-----|-----------------|
| KMS (AWS-managed keys) | Google Default Encryption | Both are automatic, transparent. GCP encrypts everything at rest by default with Google-managed keys |
| KMS (CMK / customer-managed) | CMEK (Cloud KMS) | Nearly identical. You create a key in Cloud KMS, configure services to use it. Key rings → key → key versions |
| KMS with imported key material | CMEK with imported keys | Same concept — you bring your own key material into the cloud KMS |
| CloudHSM | Cloud HSM | Same concept. FIPS 140-2 Level 3 validated. In GCP, Cloud HSM keys are managed *through* Cloud KMS (same API, HSM protection level) |
| KMS with external key store (XKS) | Cloud EKM (External Key Manager) | Same concept — keys stay in your external key manager (Thales, Fortanix). GCP calls it EKM |
| SSE-C (S3 customer-provided keys) | CSEK (Customer-Supplied Encryption Keys) | Same concept — you provide the key with each request, cloud provider doesn't store it |
| Secrets Manager | Secret Manager | Nearly identical. Versioning, rotation, IAM-based access. GCP's is simpler (no built-in Lambda rotation — you wire it yourself) |
| Certificate Manager (ACM) | Certificate Authority Service + Google-managed SSL certs | ACM ≈ Google-managed certs for load balancers. CAS is more like ACM Private CA |
| Nitro Enclaves / Nitro System | Confidential VMs (AMD SEV) | Both encrypt data in use (memory). Different hardware approaches. GCP's is VM-level, not enclave-level |

### What Will Feel Different
- GCP Cloud KMS, Cloud HSM, and Cloud EKM all use the **same API** — you just pick a "protection level" (SOFTWARE, HSM, EXTERNAL). In AWS these are separate services
- CMEK is **heavily tested** on this exam — know exactly which GCP services support it and how key rotation works
- Envelope encryption works the same way conceptually

---

## Network Security

| AWS | GCP | Key Differences |
|-----|-----|-----------------|
| Security Groups | Firewall Rules (VPC-level) | **Major difference.** AWS SGs are stateful, per-ENI. GCP firewall rules are VPC-wide with targets (tags or service accounts). There are no "security groups" you attach to instances |
| NACLs | Hierarchical Firewall Policies | GCP firewall policies can be applied at org, folder, or VPC level — like NACLs but more flexible. Evaluated by priority |
| AWS WAF | Cloud Armor | Cloud Armor combines WAF + DDoS protection. No separate "Shield" product needed |
| AWS Shield / Shield Advanced | Cloud Armor (included) | GCP bundles DDoS protection into Cloud Armor. Adaptive Protection ≈ Shield Advanced's automatic mitigations |
| AWS Network Firewall | Cloud NGFW (Next-Gen Firewall) | Both do L7 inspection. Cloud NGFW integrates with firewall policies |
| VPC Endpoints (Gateway) | Private Google Access | For accessing Google APIs (S3 ≈ Cloud Storage) from private subnets without internet |
| VPC Endpoints (Interface / PrivateLink) | Private Service Connect | Private endpoints for specific services. PSC creates a private IP in your VPC for the service |
| No direct equivalent | **VPC Service Controls** | **This has no AWS equivalent.** VPC-SC creates a security perimeter around GCP services (BigQuery, Cloud Storage, etc.) that restricts data exfiltration even if IAM allows access. Think of it as a data exfiltration boundary. Closest AWS analog is a combination of VPC Endpoint Policies + S3 bucket policies + resource policies, but VPC-SC is more comprehensive and centralized |
| VPN Gateway | Cloud VPN (HA-VPN) | Same concept. HA-VPN gives 99.99% SLA with BGP |
| Direct Connect | Cloud Interconnect | Dedicated Interconnect ≈ Direct Connect. Partner Interconnect ≈ Direct Connect via partner |
| NAT Gateway | Cloud NAT | Same concept. Cloud NAT is software-defined (no NAT instance/gateway to manage) |
| Transit Gateway | Network Connectivity Center (NCC) | Hub-and-spoke model. Less commonly tested |
| RAM (Resource Access Manager) | Shared VPC | Both share network resources across accounts/projects. Shared VPC shares a VPC from a host project to service projects |
| ALB / NLB | Application LB / Network LB | Similar split. GCP has more LB types (internal/external, regional/global, proxy/passthrough) — know the matrix |
| AWS Config Rules (network) | VPC Flow Logs + Cloud NGFW Logs | For network monitoring and compliance |

### What Will Feel Different
- **No security groups.** This is the biggest mental shift. GCP firewall rules are VPC-wide, matched by target tags or target service accounts, evaluated by priority number
- **VPC Service Controls is critical.** Learn this deeply — it's tested heavily and has no clean AWS mapping. Think: "even if IAM says yes, VPC-SC can say no based on *where the request comes from*"
- GCP firewall rules use **priority numbers** (lower = higher priority, 0-65535). AWS NACLs use rule numbers similarly, but security groups don't
- **Service account-based firewall targets** are preferred over tag-based (tags can be set by anyone with instance admin, service accounts are IAM-controlled)
- IAP TCP Forwarding for SSH replaces the need for bastion hosts — no AWS equivalent that's this clean

---

## Logging, Monitoring & Threat Detection

| AWS | GCP | Key Differences |
|-----|-----|-----------------|
| CloudTrail | Cloud Audit Logs | Same concept. Admin Activity ≈ management events (always on, free). Data Access ≈ data events (must enable, except BigQuery) |
| CloudWatch Logs | Cloud Logging | Same concept. GCP uses log sinks (≈ subscription filters) to route logs to Storage, BigQuery, or Pub/Sub |
| GuardDuty | Event Threat Detection (part of SCC) | Both detect threats from log analysis. Event Threat Detection requires SCC Premium |
| Security Hub | **Security Command Center (SCC)** | Central security dashboard. SCC has tiers: Standard (free), Premium, Enterprise. Premium includes threat detection and compliance monitoring |
| AWS Config | Security Health Analytics + Asset Inventory (part of SCC) | Config rules ≈ SHA detectors. Both check for misconfigurations |
| Inspector | Artifact Analysis (for containers) + Web Security Scanner | Inspector for EC2 ≈ Web Security Scanner. Inspector for ECR ≈ Artifact Analysis |
| Macie | **Sensitive Data Protection (SDP / DLP API)** | Both discover and classify sensitive data. GCP's DLP API is more hands-on (you configure inspection jobs, de-identification templates). Macie is more automated/managed |
| Detective | Chronicle / Google Security Operations | SIEM and investigation. Chronicle is Google's security analytics platform |
| CloudWatch Metrics + Alarms | Cloud Monitoring + Alerting Policies | Same concept |
| VPC Flow Logs | VPC Flow Logs | Same concept, same name |
| Traffic Mirroring | Packet Mirroring | Same concept |
| Network Firewall logs | Cloud NGFW Logs + Cloud IDS | Cloud IDS is a managed IDS powered by Palo Alto |

### What Will Feel Different
- **SCC is the hub for everything** — threat detection, misconfiguration scanning, compliance, asset inventory. It's more tightly integrated than Security Hub
- Cloud Audit Logs have 4 types (Admin Activity, Data Access, System Event, Policy Denied) — know which are on by default and retention periods
- Log sinks are how you export logs — they're more flexible than CloudWatch subscription filters (you can aggregate across org/folders)
- **BigQuery Data Access logs are enabled by default** — this is a common exam question

---

## CI/CD & Compute Security

| AWS | GCP | Key Differences |
|-----|-----|-----------------|
| ECR image scanning | Artifact Analysis (in Artifact Registry) | Same concept — automated CVE scanning for container images |
| No direct equivalent | **Binary Authorization** | Enforces deploy-time attestation for GKE/Cloud Run. Closest AWS pattern is a custom solution with CodePipeline + image signing. GCP has this built in |
| Nitro TPM / Secure Boot | Shielded VMs (vTPM + Secure Boot + Integrity Monitoring) | Same concepts. GCP bundles them as "Shielded VM" |
| Systems Manager Patch Manager | OS Patch Management (VM Manager) | Same concept |
| EC2 Image Builder | Custom image creation (`gcloud compute images create`) | Less automated in GCP — typically you create an instance, harden it, then create an image from it |
| CodePipeline / CodeBuild | Cloud Build | Cloud Build ≈ CodeBuild. No exact CodePipeline equivalent — Cloud Build triggers fill that role |
| EKS Pod Identity / IRSA | GKE Workload Identity | Same concept — pods get IAM identity without service account keys |

---

## Compliance & Governance

| AWS | GCP | Key Differences |
|-----|-----|-----------------|
| Control Tower | Assured Workloads | Both create guardrailed environments for compliance (FedRAMP, etc.). Assured Workloads is simpler — it creates a folder with org policies applied |
| AWS Artifact (compliance reports) | Compliance Reports Manager / [cloud.google.com/security/compliance](https://cloud.google.com/security/compliance/) | Where you download SOC reports, ISO certs, etc. |
| CloudTrail Lake / Org Trail | Aggregated Log Sinks | Export audit logs org-wide for compliance retention |
| No direct equivalent | **Access Transparency** | Logs when Google staff access your data. AWS doesn't expose this |
| No direct equivalent | **Access Approval** | You approve/deny Google staff access to your data. No AWS equivalent |
| AWS Audit Manager | SCC compliance monitoring | Both track compliance posture |

### What Will Feel Different
- **Access Transparency and Access Approval have no AWS equivalents** — these are unique GCP features and are tested on the exam
- Assured Workloads is simpler than Control Tower — it's just a folder with compliance-specific org policies
- The compliance frameworks tested are the same ones you know from AWS Security Specialty (PCI-DSS, HIPAA, SOX, GDPR, FIPS 140-2)

---

## Quick Translation Cheat Sheet

When the exam says... | Think of the AWS equivalent...
---|---
Cloud Identity | AWS SSO / Identity Center (the directory part)
IAM Binding | IAM Policy attachment (but reversed — bind member to role on resource)
Organization Policy | SCP (but constraint-based, not IAM-based)
VPC Service Controls | VPC Endpoint Policies + resource policies (but much more powerful)
Cloud Armor | WAF + Shield combined
Cloud NGFW | AWS Network Firewall
IAP | No clean equivalent (SSM Session Manager is closest for SSH)
CMEK | CMK / customer-managed KMS key
Cloud HSM | CloudHSM (but same API as KMS, just a protection level)
SCC Premium | Security Hub + GuardDuty + Config bundled
Sensitive Data Protection | Macie (but more configurable)
Binary Authorization | Custom image signing pipeline (no AWS native equivalent)
Shared VPC | RAM-shared VPC subnets
Private Google Access | Gateway VPC Endpoint
Private Service Connect | Interface VPC Endpoint / PrivateLink
Access Transparency | No AWS equivalent
Assured Workloads | Control Tower

---

## The 5 Concepts That Will Trip You Up Coming From AWS

1. **No security groups.** Firewall rules are VPC-wide with targets. Use service accounts as targets, not tags.

2. **VPC Service Controls.** This is new territory. It's a data exfiltration perimeter around GCP API services. Even if IAM says "allowed," VPC-SC can block the request based on where it originates. Study this deeply.

3. **IAM model is inverted.** You don't attach policies to users. You set bindings (member + role) on resources. Permissions flow *down* the hierarchy (org → folder → project → resource).

4. **Organization Policies ≠ SCPs.** Org policies restrict *configurations* (e.g., "no external IPs on VMs," "no service account key creation"). They don't grant or deny IAM permissions. They're a separate control plane.

5. **CMEK is everywhere.** AWS has CMKs but you don't get tested on which services support them nearly as much. GCP's exam loves asking "how do you encrypt X with CMEK?" — know the integration list.
