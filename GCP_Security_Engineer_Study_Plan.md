# GCP Professional Cloud Security Engineer — Study Plan

---

## Exam Overview (Updated)

- **Questions:** 40 multiple choice/select
- **Time:** 2 hours
- **Passing score:** 70%
- **Cost:** $200 USD
- **Recertification:** Every 2 years
- **Proctoring note:** Google now requires dual camera (webcam + phone) for remote exams

The exam is scenario-based — expect questions framed around a fictional company. The exam was significantly updated in 2025 with **more Vertex AI security, CMEK, and Google Workspace security** content than previous versions.

---

## The 5 Exam Domains

| # | Domain | Weight |
|---|--------|--------|
| 1 | Configuring Access | 25% |
| 2 | Ensuring Data Protection | 23% |
| 3 | Securing Communications & Perimeter Protection | 22% |
| 4 | Managing Security Operations | 19% |
| 5 | Supporting Compliance | 11% |

---

## Where to Study — Resource Tiers

Not all resources are equal. Here's how to prioritize:

### Tier 1: Primary Resources (use these as your backbone)

| Resource | Cost | Why |
|----------|------|-----|
| [Google Cloud Skills Boost — Security Engineer Learning Path](https://www.cloudskillsboost.google/paths/15) | Free (some labs need ~$29/mo subscription) | Official Google path with hands-on labs. The labs are critical — this exam is scenario-heavy |
| [Coursera — Preparing for Google Cloud Certification: Cloud Security Engineer](https://www.coursera.org/professional-certificates/google-cloud-security) | ~$49/mo (Coursera Plus) | Best structured video content available. Includes: Managing Security in GCP, Security Best Practices, Mitigating Security Vulnerabilities, and hands-on labs |
| [Official Exam Guide](https://cloud.google.com/learn/certification/guides/cloud-security-engineer) | Free | Your checklist. Every bullet point is testable. Print this and check items off |
| Google Cloud Documentation (linked per domain below) | Free | The exam tests documentation-level knowledge. You *will* need to read docs, not just watch videos |

### Tier 2: Supplementary Learning

| Resource | Cost | Why |
|----------|------|-----|
| [Coursera — Networking in Google Cloud Specialization](https://www.coursera.org/specializations/networking-google-cloud-platform) | Coursera Plus | Domain 3 (22% of exam) is heavily networking. If networking is a weak area, this is essential |
| [Official Packt Study Guide Book](https://www.amazon.com/Official-Certified-Professional-Security-Engineer/dp/1835468861) | ~$35-45 | The official book. Good for deep reading on topics where videos feel shallow |
| [Pluralsight — Google Cloud Security Engineer Path](https://www.pluralsight.com/paths/google-cloud-professional-security-engineer) | ~$29/mo | Alternative to Coursera if you already have a subscription |
| [Ammett Williams — "Prepare for the PCSE Exam 2025+"](https://medium.com/google-cloud/prepare-for-the-google-professional-cloud-security-engineer-exam-2025-42ec3743f97a) | Free | Best blog post on the current exam. Covers what changed, what's heavily tested now |

### Tier 3: Practice Exams (do these in Phase 6)

| Resource | Cost | Notes |
|----------|------|-------|
| [Google Official Practice Exam](https://cloud.google.com/learn/certification/cloud-security-engineer) | Free | Do this first (baseline) and last (validation). Small question pool but accurate style |
| [Whizlabs Practice Tests](https://www.whizlabs.com/google-cloud-certified-professional-cloud-security-engineer/) | ~$20-30 | 250+ questions with explanations. Good quality |
| [Udemy — 360 Practice Questions (2026)](https://www.udemy.com/course/gcp-professional-cloud-security-engineer-practice-exams-s/) | ~$15 on sale | 6 full-length practice exams. Most current Udemy option |
| [ExamTopics — Free Questions](https://www.examtopics.com/exams/google/professional-cloud-security-engineer/) | Free | **Use with caution** — many "correct" answers are wrong. Always read the community discussion comments to find the real answer |
| Diagnostic questions in the 6 PDF workbooks (in this folder) | You have these | ~60 questions across all domains with references |

### What About YouTube?

Unlike other GCP certs (like Associate Cloud Engineer), there is **no single comprehensive free YouTube course** for this exam. Use YouTube for:
- Specific topics you're struggling with (search "GCP VPC Service Controls tutorial", etc.)
- [Google Cloud Tech channel](https://www.youtube.com/googlecloudtech) security playlist
- [Cloud Security Podcast by Google](https://cloud.withgoogle.com/cloudsecurity/podcast/) for context

---

## Phase 0: Prerequisites (Days 1–3)

**Goal:** Make sure GCP fundamentals are solid before diving into security.

### If you're new to GCP:
- [ ] [Google Cloud Fundamentals: Core Infrastructure](https://www.coursera.org/learn/gcp-fundamentals) on Coursera (~8 hours)
- [ ] Get comfortable with Cloud Console and `gcloud` CLI in Cloud Shell

### If you already use GCP:
- [ ] Skim the [GCP Security Foundations Blueprint](https://cloud.google.com/architecture/security-foundations) — this is Google's opinionated reference architecture for security and it frames how the exam thinks
- [ ] Read the [Official Exam Guide](https://cloud.google.com/learn/certification/guides/cloud-security-engineer) end-to-end and honestly assess which domains are your weakest

---

## Phase 1: Configuring Access (25% of exam) — Days 4–12

> This is the **highest-weighted domain**. Nail this.

### What to Study

**Cloud Identity & Authentication**
- [ ] Cloud Identity (free vs. premium), Google Workspace relationship
- [ ] Google Cloud Directory Sync (GCDS) — syncing on-prem AD to Cloud Identity
- [ ] SAML 2.0 SSO with third-party IdPs (Okta, Azure AD)
- [ ] Session control and reauthentication policies (Admin Console)
- [ ] Multi-factor authentication enforcement
- [ ] Workforce Identity Federation (external IdPs without syncing)

**IAM Deep Dive**
- [ ] IAM policy binding model: member → role → resource
- [ ] Basic vs. predefined vs. custom roles — when to use each
- [ ] IAM Conditions (time-based, resource-based)
- [ ] IAM Deny Policies
- [ ] Policy inheritance across the resource hierarchy
- [ ] Policy Troubleshooter and Policy Analyzer (Policy Intelligence)
- [ ] Privileged Access Manager (PAM) — just-in-time elevated access

**Service Accounts**
- [ ] Service account types (default, user-managed, Google-managed)
- [ ] Service account keys vs. Workload Identity Federation — **strongly prefer keyless**
- [ ] Short-lived credentials (OAuth2 tokens, JWTs, delegated requests)
- [ ] Org policies: `iam.disableServiceAccountKeyCreation`, `iam.disableCrossProjectServiceAccountUsage`
- [ ] Service account impersonation

**Resource Hierarchy & Org Policies**
- [ ] Organization → Folders → Projects → Resources
- [ ] Designing hierarchies to match org structure
- [ ] Organization Policy Service — boolean and list constraints
- [ ] Policy inheritance and override behavior (`enforced: true/false`, merge with parent)

### Where to Learn This

| Topic | Best Resource | Type |
|-------|---------------|------|
| Cloud Identity & GCDS | [Active Directory provisioning docs](https://cloud.google.com/architecture/identity/provisioning-overview) | Read |
| SAML SSO | [Set up SSO via third-party IdP](https://support.google.com/a/answer/60224) | Read |
| IAM fundamentals | Coursera — Managing Security in Google Cloud, M3 | Watch + Lab |
| IAM Conditions & Deny | [IAM Conditions docs](https://cloud.google.com/iam/docs/conditions-overview) + [Deny policies](https://cloud.google.com/iam/docs/deny-overview) | Read |
| Service accounts | [Best practices for securing service accounts](https://cloud.google.com/iam/docs/best-practices-for-securing-service-accounts) | Read |
| Resource hierarchy | [Best practices for enterprise orgs](https://cloud.google.com/docs/enterprise/best-practices-for-enterprise-organizations) | Read |
| Org policies | [Understanding hierarchy evaluation](https://cloud.google.com/resource-manager/docs/organization-policy/understanding-hierarchy) | Read |
| Privileged Access Manager | [PAM overview](https://cloud.google.com/iam/docs/pam-overview) | Read |
| Hands-on | Skills Boost — **Implement Cloud Security Fundamentals on Google Cloud** skill badge | Lab |

### Key Exam Traps
- Questions will try to get you to assign roles at too broad a level (org when project is enough)
- Know the difference between `roles/editor` (basic) vs. predefined roles — the exam almost always wants predefined or custom
- Service account key questions: the answer is almost always "don't use keys" — use Workload Identity or short-lived tokens

---

## Phase 2: Ensuring Data Protection (23% of exam) — Days 13–22

> Second highest domain. CMEK is **heavily tested** on the current exam.

### What to Study

**Sensitive Data Protection (DLP)**
- [ ] Sensitive Data Protection API (formerly Cloud DLP)
- [ ] InfoTypes — built-in vs. custom (regex, dictionary)
- [ ] De-identification: redaction, masking, date shifting, bucketing, pseudonymization, format-preserving encryption
- [ ] Image redaction for PII
- [ ] Integration with Cloud Storage, BigQuery, Datastore

**Data Access Controls**
- [ ] BigQuery — authorized views, authorized datasets, column-level security, row-level security
- [ ] Cloud Storage — IAM vs. ACLs, uniform bucket-level access, signed URLs
- [ ] Cloud SQL — IAM database authentication, authorized networks, private IP

**Secrets Management**
- [ ] Secret Manager — creating, versioning, rotating secrets
- [ ] IAM roles (`secretmanager.secretAccessor`, etc.)
- [ ] When to use Secret Manager vs. environment variables vs. KMS-encrypted values

**Encryption (study this hard)**
- [ ] Google Default Encryption — AES-256, automatic
- [ ] **CMEK** — Cloud KMS, key rings, crypto keys, which services support it
- [ ] CSEK — you provide the key, Google doesn't store it
- [ ] Cloud EKM — keys stored outside Google (Thales, Fortanix, etc.)
- [ ] Cloud HSM — FIPS 140-2 Level 3 hardware-backed keys
- [ ] Key rotation, key versions, key destruction schedule
- [ ] Envelope encryption concept

**Confidential Computing**
- [ ] Confidential VMs — AMD SEV, data encrypted in use
- [ ] Attestation reports
- [ ] When Confidential Computing is the answer vs. standard encryption

**AI/ML Security (new and heavily tested)**
- [ ] Vertex AI security controls — VPC Service Controls for Vertex AI
- [ ] Private endpoints vs. public endpoints for Vertex AI
- [ ] Protecting training data and models
- [ ] Sensitive Data Protection for GenAI workloads
- [ ] IaaS vs. PaaS security responsibilities for ML

### Where to Learn This

| Topic | Best Resource | Type |
|-------|---------------|------|
| DLP / Sensitive Data Protection | [SDP overview](https://cloud.google.com/sensitive-data-protection/docs/concepts-overview) + [Interactive demo](https://cloud.google.com/dlp/demo/#!/) | Read + Try |
| De-identification techniques | [De-identification concepts](https://cloud.google.com/sensitive-data-protection/docs/pseudonymization) | Read |
| BigQuery security | [Authorized views](https://cloud.google.com/bigquery/docs/authorized-views) + [Row-level security](https://cloud.google.com/bigquery/docs/row-level-security-intro) | Read |
| Secret Manager | [Secret Manager quickstart](https://cloud.google.com/secret-manager/docs/quickstart) | Read + Lab |
| CMEK (critical) | [CMEK overview](https://cloud.google.com/kms/docs/cmek) + [Which services support CMEK](https://cloud.google.com/kms/docs/using-other-products#cmek_integrations) | Read |
| CSEK vs CMEK vs EKM | [Cloud Storage encryption options](https://cloud.google.com/storage/docs/encryption) | Read |
| Key rotation | [Rotating keys](https://cloud.google.com/kms/docs/rotating-keys) | Read |
| Confidential VMs | [About Confidential VMs](https://cloud.google.com/compute/confidential-vm/docs/about-cvm) | Read |
| Vertex AI security | [VPC-SC with Vertex AI](https://cloud.google.com/vertex-ai/docs/general/vpc-service-controls) + [SDP for GenAI](https://cloud.google.com/blog/products/identity-security/how-sensitive-data-protection-can-help-secure-generative-ai-workloads) | Read |
| Full domain coverage | Coursera — Security Best Practices in Google Cloud, M2 (Securing Cloud Data) | Watch + Lab |
| Hands-on | Skills Boost — **Implement Cloud Security Fundamentals on Google Cloud** | Lab |

### Key Exam Traps
- Know exactly when to choose CMEK vs. CSEK vs. EKM — the deciding factor is usually *who controls the key* and *where is it stored*
- CMEK questions are everywhere on the current exam — know which services support it and how key rotation works
- DLP questions will test whether you know the right de-identification method for a scenario (date shifting for analytics, FPE for preserving format, etc.)
- Vertex AI security is ~25% of the current exam — don't skip it

---

## Phase 3: Securing Communications & Perimeter Protection (22%) — Days 23–32

> This is the networking-heavy domain. If networking is your weak spot, spend extra time here.

### What to Study

**Perimeter Security**
- [ ] Cloud Armor — WAF rules, rate limiting (`--action=throttle` vs `--action=rate-based-ban`), OWASP pre-configured rules, adaptive protection
- [ ] Cloud NGFW — L7 inspection, firewall policies at org/folder/VPC level
- [ ] Identity-Aware Proxy (IAP) — TCP forwarding for SSH/RDP to private VMs, context-aware access
- [ ] Certificate Authority Service and Google-managed SSL certificates
- [ ] Secure Web Proxy
- [ ] Cloud DNS security (DNSSEC, private zones, DNS peering)

**Load Balancing & TLS**
- [ ] External vs. internal Application Load Balancers
- [ ] SSL certificate types (Google-managed vs. self-managed)
- [ ] HTTPS proxy, URL maps, forwarding rules
- [ ] SSL policies (minimum TLS version)

**Network Segmentation**
- [ ] VPC design — custom mode vs. auto mode
- [ ] VPC Peering vs. Shared VPC — when to use which
- [ ] Firewall rules: priority, direction, targets (tags vs. service accounts)
- [ ] Service account-based firewall rules for N-tier isolation
- [ ] **VPC Service Controls** — perimeters, access levels, bridges, ingress/egress rules, dry-run mode

**Private Connectivity**
- [ ] Private Google Access (PGA) — VMs without external IPs
- [ ] Private Google Access for on-premises hosts
- [ ] Private Service Connect (PSC) — private endpoints for Google APIs
- [ ] Restricted Google Access (`restricted.googleapis.com`)
- [ ] HA-VPN — IKEv2, BGP, 99.99% SLA
- [ ] Cloud Interconnect — Dedicated vs. Partner
- [ ] Cloud NAT — outbound internet for private VMs/GKE
- [ ] Serverless VPC Access connectors

### Where to Learn This

| Topic | Best Resource | Type |
|-------|---------------|------|
| Networking foundations | [Coursera — Networking in Google Cloud Specialization](https://www.coursera.org/specializations/networking-google-cloud-platform) | Watch + Lab |
| Cloud Armor | [Cloud Armor docs](https://cloud.google.com/armor/docs) + [gcloud security-policies reference](https://cloud.google.com/sdk/gcloud/reference/compute/security-policies) | Read |
| IAP TCP forwarding | [IAP TCP forwarding setup](https://cloud.google.com/iap/docs/using-tcp-forwarding) | Read + Lab |
| VPC firewall rules | [Firewall rules overview](https://cloud.google.com/firewall/docs/about-firewalls) + [Service account firewalls](https://cloud.google.com/vpc/docs/using-firewalls#serviceaccounts) | Read |
| VPC Service Controls | [VPC-SC overview](https://cloud.google.com/vpc-service-controls/docs/overview) — **read the entire overview page** | Read |
| Shared VPC | [Shared VPC overview](https://cloud.google.com/vpc/docs/shared-vpc) + [App Engine with Shared VPC](https://cloud.google.com/appengine/docs/standard/python3/connecting-shared-vpc) | Read |
| Private connectivity | [Choose a networking product](https://cloud.google.com/network-connectivity/docs/how-to/choose-product) | Read |
| Private Google Access | [PGA overview](https://cloud.google.com/vpc/docs/private-google-access) + [PGA for on-prem](https://cloud.google.com/vpc/docs/private-google-access-hybrid) | Read |
| Cloud NAT | [Cloud NAT overview](https://cloud.google.com/nat/docs/overview) + [GKE example](https://cloud.google.com/nat/docs/gke-example) | Read + Lab |
| VPC best practices | [VPC design best practices](https://cloud.google.com/architecture/best-practices-vpc-design) | Read |
| Full domain coverage | Coursera — Managing Security in Google Cloud, M4 (VPC Isolation) | Watch + Lab |
| Hands-on | Skills Boost — **Build and Secure Networks in Google Cloud** | Lab |

### Key Exam Traps
- Cloud Armor `throttle` returns 429 (servers busy), `rate-based-ban` returns 403 (forbidden) — know the difference
- VPC Service Controls questions are common — understand the difference between perimeters, access levels, and ingress/egress rules
- For SSH to private VMs, the answer is almost always IAP TCP forwarding (not bastion hosts, not opening port 22)
- Shared VPC + serverless services (App Engine, Cloud Run) requires a Serverless VPC Access connector

---

## Phase 4: Managing Security Operations (19%) — Days 33–40

### What to Study

**CI/CD Security**
- [ ] Artifact Registry + Artifact Analysis — automated CVE scanning
- [ ] Binary Authorization — attestation policies for GKE/Cloud Run
- [ ] Cloud Build — build configs, triggers, deploying to GKE
- [ ] Container image signing and verification

**VM & Image Security**
- [ ] Shielded VMs — Secure Boot, vTPM, Integrity Monitoring
- [ ] Creating hardened custom images
- [ ] OS Patch Management (VM Manager)
- [ ] Org policies: `constraints/compute.requireShieldedVm`

**Logging & Monitoring**
- [ ] Cloud Audit Logs — Admin Activity, Data Access, System Event, Policy Denied
  - Admin Activity: always on, free, 400-day retention
  - Data Access: must be enabled (except BigQuery — on by default), 30-day retention
- [ ] VPC Flow Logs, Cloud NGFW logs
- [ ] Packet Mirroring and Cloud IDS
- [ ] Log sinks and aggregated sinks — routing to Storage, BigQuery, Pub/Sub
- [ ] Log Analytics (BigQuery-linked datasets)
- [ ] Log-based metrics and alerts

**Security Command Center (SCC)**
- [ ] Tiers: Standard vs. Premium vs. Enterprise
- [ ] Built-in services: Security Health Analytics, Event Threat Detection, Container Threat Detection, Web Security Scanner
- [ ] Anomaly Detection (cryptomining, etc.)
- [ ] Findings, assets, attack paths
- [ ] Real-time notifications via Pub/Sub + Cloud Run functions
- [ ] Custom modules for Security Health Analytics

**Incident Response**
- [ ] GKE forensics — snapshot, isolate, analyze
- [ ] Exporting logs for forensic analysis
- [ ] Chronicle / Google Security Operations

### Where to Learn This

| Topic | Best Resource | Type |
|-------|---------------|------|
| Artifact Analysis | [Container scanning overview](https://cloud.google.com/container-analysis/docs/container-scanning-overview) | Read |
| Binary Authorization | [Binary Authorization overview](https://cloud.google.com/binary-authorization/docs/overview) | Read |
| Shielded VMs | [Shielded VM docs](https://cloud.google.com/compute/shielded-vm/docs/shielded-vm) + [Integrity monitoring](https://cloud.google.com/compute/shielded-vm/docs/integrity-monitoring) | Read |
| Cloud Audit Logs | [Audit logging overview](https://cloud.google.com/logging/docs/audit) + [Configure Data Access logs](https://cloud.google.com/logging/docs/audit/configure-data-access) | Read |
| Log exports | [Export logs for security analytics](https://cloud.google.com/architecture/exporting-stackdriver-logging-for-security-and-access-analytics) | Read |
| Security Command Center | [SCC overview](https://cloud.google.com/security-command-center/docs/overview) + [Configure SCC](https://cloud.google.com/security-command-center/docs/how-to-configure-security-command-center) | Read |
| SCC notifications | [Real-time notifications](https://cloud.google.com/security-command-center/docs/how-to-enable-real-time-notifications) | Read |
| GKE forensics | [Security controls for GKE apps](https://cloud.google.com/architecture/security-controls-and-forensic-analysis-for-GKE-apps) | Read |
| Full domain coverage | Coursera — Mitigating Security Vulnerabilities on Google Cloud, M3 | Watch + Lab |
| Hands-on | Skills Boost — **Google Kubernetes Engine Best Practices: Security** | Lab |

### Key Exam Traps
- BigQuery Data Access logs are enabled **by default** — Cloud Storage and most other services require manual enablement
- For cost-effective long-term log storage: export to Cloud Storage with lifecycle rules (not BigQuery)
- For querying/forensics: export to BigQuery
- SCC Premium is required for Event Threat Detection and Container Threat Detection

---

## Phase 5: Supporting Compliance (11% of exam) — Days 41–44

> Lowest weight, but some questions have **no coverage in training materials** — you must self-study from docs.

### What to Study

**Compliance Frameworks (know which is which)**
- [ ] **PCI-DSS** — payment card data, network segmentation, cardholder data isolation
- [ ] **HIPAA** — healthcare data, BAA, Cloud Healthcare API, de-identification
- [ ] **SOX** — financial reporting controls, audit trails, separation of duties
- [ ] **GDPR** — EU data privacy, data residency, right to erasure
- [ ] **FIPS 140-2** — cryptographic module validation (Cloud HSM)
- [ ] **ISO 27018** — PII protection in the cloud
- [ ] **FedRAMP** — US government workloads

**Shared Responsibility Model**
- [ ] What Google secures vs. what you secure
- [ ] How responsibility shifts across IaaS / PaaS / SaaS

**Compliance Controls**
- [ ] Assured Workloads — compliant environments (FedRAMP, IL4, CJIS, ITAR)
- [ ] Access Transparency — logs of Google staff accessing your data
- [ ] Access Approval — approve/deny Google staff access
- [ ] Data residency — org policies for resource location
- [ ] VPC Service Controls for compliance boundaries

### Where to Learn This

| Topic | Best Resource | Type |
|-------|---------------|------|
| Compliance overview | [Google Cloud compliance offerings](https://cloud.google.com/security/compliance/) | Read |
| Shared responsibility | [Shared responsibilities and shared fate](https://cloud.google.com/architecture/framework/security/shared-responsibility-shared-fate) | Read |
| Assured Workloads | [Assured Workloads overview](https://cloud.google.com/assured-workloads/docs/overview) | Read |
| Access Transparency | [Access Transparency docs](https://cloud.google.com/cloud-provider-access-management/access-transparency/docs/overview) | Read |
| HIPAA on GCP | [HIPAA overview guide](https://cloud.google.com/files/gcp-hipaa-overview-guide.pdf) + [HIPAA-aligned project setup](https://cloud.google.com/architecture/setting-up-a-hipaa-aligned-project) | Read |
| PCI-DSS on GCP | [PCI-DSS compliance in GCP](https://cloud.google.com/architecture/pci-dss-compliance-in-gcp) | Read |
| Encryption for compliance | [Cloud Storage encryption options](https://cloud.google.com/storage/docs/encryption) + [CSEK](https://cloud.google.com/storage/docs/encryption/customer-supplied-keys) | Read |
| Full domain coverage | Coursera — Security in Google Cloud, M1 (Foundations) | Watch |

### Key Exam Traps
- Know the difference between Access Transparency (logging) and Access Approval (gating)
- CSEK gives *you* full key control (ISO 27018 scenario) — CMEK keeps the key in Google's KMS
- SOX and HIPAA questions often aren't covered in courses — study the architecture guides directly

---

## Phase 6: Practice & Review — Days 45–52

### Practice Exam Order
1. [ ] **Google Official Practice Exam** — take this first as a baseline. Note your score per domain
2. [ ] **Whizlabs or Udemy practice tests** — work through 2-3 full exams
3. [ ] **Diagnostic questions from PDF workbooks** (in this folder) — ~60 questions with doc references
4. [ ] **ExamTopics** — use for additional exposure, but verify answers in comments
5. [ ] **Google Official Practice Exam again** — retake as final validation

### Review Strategy
- [ ] For every wrong answer, read the specific Google documentation page — not just the explanation
- [ ] Focus extra time on your 2 weakest domains
- [ ] Aim for **80%+ on practice exams** before scheduling the real exam

### Decision Matrices to Memorize

**Encryption:**
| Scenario | Answer |
|----------|--------|
| Default, no special requirements | Google Default Encryption |
| You need to control key lifecycle | CMEK (Cloud KMS) |
| You need to provide your own key, Google can't store it | CSEK |
| Keys must stay outside Google entirely | Cloud EKM |
| Need FIPS 140-2 Level 3 | Cloud HSM |
| Encrypt data in use (memory) | Confidential VMs |

**Private Connectivity:**
| Scenario | Answer |
|----------|--------|
| VM without external IP needs Google APIs | Private Google Access |
| On-prem needs private access to Google APIs | PGA for on-prem hosts |
| Private endpoint for a specific Google service | Private Service Connect |
| Restrict to only VPC-SC supported APIs | restricted.googleapis.com |
| On-prem to GCP, encrypted, < 10 Gbps | HA-VPN |
| On-prem to GCP, high bandwidth, co-located at PoP | Dedicated Interconnect |
| On-prem to GCP, not at a PoP | Partner Interconnect |
| Private GKE nodes need outbound internet | Cloud NAT |

**Perimeter Security:**
| Scenario | Answer |
|----------|--------|
| DDoS + WAF + rate limiting | Cloud Armor |
| L7 deep packet inspection | Cloud NGFW |
| SSH to private VM | IAP TCP forwarding |
| Restrict API access to authorized VPCs/projects | VPC Service Controls |
| Context-aware access to web apps | IAP + Access Context Manager |

---

## Phase 7: Exam Day — Day 53+

### Tips
1. **Read the full question** — the key constraint is often in the last sentence
2. **Eliminate wrong answers first** — look for: unencrypted channels, overly broad roles, wrong service
3. **"Least privilege"** — if an answer gives more access than needed, eliminate it
4. **"Google-recommended"** — favor managed services, keyless auth, Google-managed certs
5. **Org policies** — many answers hinge on applying constraints at the right hierarchy level
6. **Flag and return** — don't spend 5 min on one question. Flag it and come back
7. **40 questions, 120 minutes** — you have 3 min per question, use it

---

## Weekly Schedule Summary

| Week | Phase | Focus | Primary Resource |
|------|-------|-------|-----------------|
| 1 | 0 + 1 | Prerequisites + Configuring Access | Coursera Cloud Security cert + IAM docs |
| 2 | 1 + 2 | Finish Access + Start Data Protection | Coursera + Cloud KMS/DLP docs |
| 3 | 2 | Data Protection + Vertex AI Security | Docs heavy — CMEK, DLP, Vertex AI |
| 4 | 3 | Network Security & Boundary Protection | Coursera Networking + VPC-SC docs |
| 5 | 3 + 4 | Finish Networking + Operations | Coursera + SCC/Logging docs |
| 6 | 4 + 5 | Finish Operations + Compliance | Architecture guides (HIPAA, PCI-DSS) |
| 7 | 6 | Practice Exams & Weak Area Review | Whizlabs + Udemy + Official practice |
| 8 | 7 | Final Review & Exam | Retake official practice, review matrices |

---

*Target: 1-2 hours daily. If you're scoring 80%+ on practice exams consistently, you're ready to schedule.*
