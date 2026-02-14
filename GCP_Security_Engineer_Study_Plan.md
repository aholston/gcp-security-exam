# GCP Professional Cloud Security Engineer — Study Plan

---

## Exam Overview

- **Exam length:** 2 hours, 50-60 multiple choice/select questions
- **Passing score:** ~70% (Google does not publish exact thresholds)
- **Cost:** $200 USD
- **Recommended experience:** 3+ years industry experience, 1+ year designing/managing GCP solutions
- **Recertification:** Every 2 years

The exam tests your ability to design and implement secure infrastructure on Google Cloud. All scenarios are practical — expect questions framed around a fictional company ("Cymbal Bank" style).

---

## The 5 Exam Domains

| # | Domain | Approx. Weight |
|---|--------|---------------|
| 1 | Configuring Access | ~20% |
| 2 | Securing Communications & Boundary Protection | ~22% |
| 3 | Ensuring Data Protection | ~22% |
| 4 | Managing Operations | ~18% |
| 5 | Supporting Compliance Requirements | ~18% |

---

## Phase 0: Prerequisites (Days 1–3)

Before diving into security-specific content, make sure you're solid on GCP fundamentals.

- [ ] **Google Cloud Fundamentals: Core Infrastructure** (Coursera / Google Cloud Skills Boost) — skip if you already have GCP experience
- [ ] Be comfortable navigating the Cloud Console, `gcloud` CLI, and Cloud Shell
- [ ] Understand the basics of Compute Engine, Cloud Storage, BigQuery, GKE, Cloud SQL, and App Engine
- [ ] Know how projects, billing accounts, and the resource hierarchy work at a high level

---

## Phase 1: Configuring Access (~20% of exam) — Days 4–10

### What to Study

**Cloud Identity & Authentication**
- [ ] Cloud Identity (free vs. premium), Google Workspace relationship
- [ ] Google Cloud Directory Sync (GCDS) — syncing on-prem Active Directory to Cloud Identity
- [ ] SAML 2.0 SSO with third-party IdPs (Okta, Azure AD, etc.)
- [ ] Session control and reauthentication policies (Admin Console)
- [ ] Multi-factor authentication enforcement

**IAM Deep Dive**
- [ ] IAM policy binding model: member → role → resource
- [ ] Basic vs. predefined vs. custom roles — when to use each
- [ ] IAM Conditions (time-based, resource-based)
- [ ] IAM Deny Policies
- [ ] Policy Inheritance across the resource hierarchy
- [ ] Policy Troubleshooter and Policy Analyzer (Policy Intelligence)
- [ ] Privileged Access Manager (PAM) — just-in-time access

**Service Accounts**
- [ ] Service account types (default, user-managed, Google-managed)
- [ ] Service account keys vs. workload identity federation — **strongly prefer keyless**
- [ ] Short-lived credentials (OAuth2 tokens, JWTs, delegated requests)
- [ ] Org policies: `iam.disableServiceAccountKeyCreation`, `iam.disableCrossProjectServiceAccountUsage`
- [ ] Service account impersonation

**Resource Hierarchy & Org Policies**
- [ ] Organization → Folders → Projects → Resources
- [ ] Designing hierarchies to match org structure
- [ ] Organization Policy Service and boolean/list constraints
- [ ] Policy inheritance and override behavior (`enforced: true/false`, merge with parent)

### Courses
- **Security in Google Cloud** — M2 (Securing Access), M3 (IAM)
- **Managing Security in Google Cloud** — M2, M3

### Hands-On
- [ ] Create a resource hierarchy with folders and apply org policies
- [ ] Set up GCDS (or read the walkthrough if no AD available)
- [ ] Create custom roles and IAM conditions
- [ ] Skill Badge: **Implement Cloud Security Fundamentals on Google Cloud**

---

## Phase 2: Securing Communications & Boundary Protection (~22%) — Days 11–20

### What to Study

**Perimeter Security**
- [ ] Cloud Armor — WAF rules, rate limiting (`--action=throttle` vs `--action=rate-based-ban`), pre-configured rules (OWASP Top 10), adaptive protection
- [ ] Cloud NGFW (Next-Generation Firewall) — L7 inspection, firewall policies vs. VPC firewall rules, hierarchy (org → folder → VPC)
- [ ] Identity-Aware Proxy (IAP) — TCP forwarding for SSH/RDP to private VMs, context-aware access
- [ ] Certificate Authority Service (CAS) and Google-managed SSL certificates
- [ ] Secure Web Proxy
- [ ] Cloud DNS security (DNSSEC, private zones, DNS peering)

**Load Balancing & SSL**
- [ ] External vs. internal Application Load Balancers
- [ ] SSL certificate types (Google-managed, self-managed) and when to use each
- [ ] HTTPS proxy, URL maps, forwarding rules — the full LB stack
- [ ] SSL policies (TLS version enforcement)

**Network Segmentation**
- [ ] VPC design — custom mode vs. auto mode, subnets, IP ranges
- [ ] VPC Peering vs. Shared VPC — trade-offs and use cases
- [ ] Firewall rules: priority, direction, target (tags vs. service accounts), protocols
- [ ] Service account-based firewall rules for N-tier isolation
- [ ] **VPC Service Controls** — service perimeters, access levels, perimeter bridges, ingress/egress rules, dry-run mode

**Private Connectivity**
- [ ] Private Google Access (PGA) — for VMs without external IPs
- [ ] Private Google Access for on-premises hosts
- [ ] Private Service Connect (PSC) — private endpoints for Google APIs and services
- [ ] Restricted Google Access (`restricted.googleapis.com`)
- [ ] Cloud VPN (HA-VPN) — IKEv2, BGP routing, 99.99% SLA
- [ ] Cloud Interconnect — Dedicated vs. Partner, when to use each
- [ ] Cloud NAT — outbound-only internet access for private VMs/GKE
- [ ] Serverless VPC Access connectors (App Engine, Cloud Run, Cloud Functions connecting to VPC)

### Courses
- **Security in Google Cloud** — M4, M5, M7, M8, M9
- **Networking in Google Cloud** — All modules (M1–M14) — this is critical for this domain

### Hands-On
- [ ] Set up a Shared VPC with firewall rules using service accounts
- [ ] Configure Cloud Armor with rate limiting on an Application LB
- [ ] Set up IAP TCP forwarding to SSH into a private VM
- [ ] Create a VPC Service Controls perimeter
- [ ] Configure Cloud NAT for a GKE cluster
- [ ] Skill Badge: **Build and Secure Networks in Google Cloud**

---

## Phase 3: Ensuring Data Protection (~22%) — Days 21–30

### What to Study

**Sensitive Data Protection (DLP)**
- [ ] Sensitive Data Protection API (formerly Cloud DLP)
- [ ] InfoTypes — built-in (CREDIT_CARD_NUMBER, EMAIL_ADDRESS, etc.) vs. custom (regex, dictionary)
- [ ] De-identification techniques: redaction, masking, date shifting, bucketing, pseudonymization, format-preserving encryption (FPE)
- [ ] Image redaction for PII
- [ ] Integration with Cloud Storage, BigQuery, Datastore

**Access Control for Data Services**
- [ ] BigQuery — authorized views, authorized datasets, column-level security, row-level security
- [ ] Cloud Storage — IAM vs. ACLs, uniform bucket-level access, signed URLs
- [ ] Cloud SQL — IAM database authentication, authorized networks, private IP

**Secrets Management**
- [ ] Secret Manager — creating, versioning, rotating secrets
- [ ] IAM roles for Secret Manager (secretmanager.secretAccessor, etc.)
- [ ] Secret rotation and expiry policies
- [ ] When to use Secret Manager vs. environment variables vs. KMS-encrypted values

**Encryption**
- [ ] Google Default Encryption (at rest) — AES-256, automatic, no config needed
- [ ] Customer-Managed Encryption Keys (CMEK) — Cloud KMS, key rings, crypto keys
- [ ] Customer-Supplied Encryption Keys (CSEK) — you provide the key, Google doesn't store it
- [ ] Cloud External Key Manager (Cloud EKM) — keys stored outside Google
- [ ] Cloud HSM — FIPS 140-2 Level 3 hardware-backed keys
- [ ] Key rotation policies and key version management
- [ ] Envelope encryption concept
- [ ] Object lifecycle policies in Cloud Storage (Nearline → Coldline → Archive transitions, deletion)

**Confidential Computing**
- [ ] Confidential VMs — AMD SEV, data encrypted in use
- [ ] Attestation reports (`sevLaunchAttestationReportEvent`)
- [ ] When to use Confidential Computing vs. standard encryption

**AI/ML Security**
- [ ] Vertex AI security controls
- [ ] IaaS vs. PaaS security model for ML training
- [ ] Protecting training data and models
- [ ] Sensitive Data Protection for GenAI workloads

### Courses
- **Security in Google Cloud** — M2, M5, M6, M8, M10, M11
- **Security Best Practices in Google Cloud** — M1, M2, M4

### Hands-On
- [ ] Use the DLP API to inspect and de-identify a sample dataset
- [ ] Create a CMEK key in Cloud KMS and encrypt a Cloud Storage bucket
- [ ] Set up a BigQuery authorized view
- [ ] Store and retrieve a secret in Secret Manager
- [ ] Skill Badge: **Implement Cloud Security Fundamentals on Google Cloud** (if not done already)

---

## Phase 4: Managing Operations (~18%) — Days 31–38

### What to Study

**CI/CD Security**
- [ ] Artifact Registry + Artifact Analysis (formerly Container Analysis) — automated CVE scanning
- [ ] Binary Authorization — attestation policies for GKE/Cloud Run deployments
- [ ] Cloud Build — build configs, triggers, deploying to GKE
- [ ] Container image signing and verification

**VM & Image Security**
- [ ] Shielded VMs — Secure Boot, vTPM, Integrity Monitoring
- [ ] Creating hardened custom images (`gcloud compute images create`)
- [ ] Image sharing within organizations
- [ ] OS Patch Management (VM Manager)
- [ ] Org policies for Shielded VMs (`constraints/compute.requireShieldedVm`)

**Logging & Monitoring**
- [ ] Cloud Audit Logs — Admin Activity, Data Access, System Event, Policy Denied
  - Admin Activity: always on, free, 400-day retention
  - Data Access: must be enabled (except BigQuery — on by default), 30-day retention
- [ ] VPC Flow Logs — sampling, metadata, use cases
- [ ] Cloud NGFW logs
- [ ] Packet Mirroring and Cloud IDS
- [ ] Log sinks and aggregated sinks — routing to Cloud Storage, BigQuery, Pub/Sub, Splunk
- [ ] Log Analytics (BigQuery-linked datasets)
- [ ] Log-based metrics and alerts

**Security Command Center (SCC)**
- [ ] SCC tiers: Standard vs. Premium vs. Enterprise
- [ ] Built-in services: Security Health Analytics, Event Threat Detection, Container Threat Detection, Web Security Scanner
- [ ] Anomaly Detection (e.g., cryptomining detection)
- [ ] Findings, assets, attack paths
- [ ] Real-time notifications via Pub/Sub + Cloud Run functions
- [ ] Custom modules for Security Health Analytics
- [ ] Security posture management and drift detection

**Incident Response**
- [ ] GKE forensics — snapshot, isolate, analyze
- [ ] Exporting logs for forensic analysis (BigQuery for querying, Cloud Storage for archival)
- [ ] Chronicle / Google Security Operations integration

### Courses
- **Security in Google Cloud** — M5, M8, M11
- **Mitigating Security Vulnerabilities on Google Cloud** — M3

### Hands-On
- [ ] Set up Artifact Analysis scanning in a Cloud Build pipeline
- [ ] Create a Shielded VM and review integrity monitoring
- [ ] Configure Data Access audit logs and export them to BigQuery
- [ ] Explore Security Command Center and review findings
- [ ] Create a log sink to route logs to Cloud Storage with lifecycle rules
- [ ] Skill Badge: **Google Kubernetes Engine Best Practices: Security**

---

## Phase 5: Supporting Compliance Requirements (~18%) — Days 39–44

### What to Study

**Compliance Frameworks**
- [ ] Know which frameworks apply to which scenarios:
  - **PCI-DSS** — payment card data (isolate cardholder data environments, network segmentation)
  - **HIPAA** — healthcare data (BAA, Cloud Healthcare API, de-identification)
  - **SOX** — financial reporting controls (audit trails, separation of duties)
  - **GDPR** — EU data privacy (data residency, right to erasure)
  - **FIPS 140-2** — cryptographic module validation (Cloud HSM)
  - **ISO 27018** — PII in the cloud
  - **FedRAMP** — US government workloads

**Shared Responsibility Model**
- [ ] What Google secures (physical, hardware, firmware, low-level infra)
- [ ] What you secure (data, IAM, network config, application code, encryption keys)
- [ ] How responsibility shifts across IaaS vs. PaaS vs. SaaS

**Compliance Controls on GCP**
- [ ] Assured Workloads — creating compliant environments (FedRAMP, IL4, CJIS, ITAR, etc.)
- [ ] Access Transparency — logs of Google staff accessing your data
- [ ] Access Approval — approve/deny Google staff access
- [ ] Data residency — regional constraints, org policies for resource location
- [ ] VPC Service Controls for compliance boundaries
- [ ] Organization Policy constraints for compliance enforcement

**Data Classification & Protection**
- [ ] Automating data classification with Sensitive Data Protection + Cloud Storage triggers
- [ ] DLP scanning pipelines for PCI-DSS (real-time credit card masking)
- [ ] Cloud Healthcare API — DICOM, FHIR, HL7v2 de-identification

### Courses
- **Security in Google Cloud** — M1, M6, M10
- **Managing Security in Google Cloud** — M1

### Hands-On
- [ ] Review the Google Cloud compliance offerings page (cloud.google.com/security/compliance)
- [ ] Set up an Assured Workloads folder (or walkthrough)
- [ ] Enable Access Transparency logs
- [ ] Practice a PCI-DSS-style project isolation setup

---

## Phase 6: Practice & Review — Days 45–52

### Practice Exams
- [ ] **Google's Official Practice Exam** — free on the certification page
- [ ] **Google Cloud Skills Boost** — practice questions within each course
- [ ] **Whizlabs GCP Professional Cloud Security Engineer** — practice tests
- [ ] **Tutorials Dojo** — GCP Security Engineer practice exams
- [ ] Review all diagnostic questions from the 6 PDF workbooks in this folder

### Review Strategy
- [ ] For each wrong answer, trace back to the documentation and understand *why*
- [ ] Focus extra time on your weakest domain(s) — aim for 80%+ on practice exams
- [ ] Re-read Google's official exam guide and make sure every bullet point is covered

### Key Concepts to Drill
- [ ] VPC Service Controls vs. IAM vs. Firewall Rules — when to use which
- [ ] CMEK vs. CSEK vs. Cloud EKM vs. Default Encryption — quick decision matrix
- [ ] Cloud Armor vs. Cloud NGFW vs. IAP — perimeter defense layers
- [ ] Audit Logs — which are on by default, which need enabling, retention periods
- [ ] Service accounts — keyless auth, workload identity, short-lived credentials
- [ ] Shared VPC vs. VPC Peering — connectivity and security trade-offs

---

## Phase 7: Exam Day — Day 53+

### Exam Tips
1. **Read the full question** — GCP exams love burying the key constraint at the end
2. **Eliminate wrong answers** — look for red flags: unencrypted channels, overly broad roles, wrong protocols
3. **"Principle of least privilege"** — if an answer gives more access than needed, it's probably wrong
4. **"Google-recommended"** — favor managed services, keyless auth, Google-managed certs over self-managed
5. **Watch for org policies** — many answers hinge on applying constraints at the right hierarchy level
6. **Flag and return** — don't get stuck on one question, flag it and come back
7. **Time management** — ~2 min per question, 50-60 questions in 120 minutes

---

## Recommended Resources (Beyond the Course Workbooks)

### Documentation (Bookmark These)
- [IAM Overview](https://cloud.google.com/iam/docs/overview)
- [VPC Service Controls](https://cloud.google.com/vpc-service-controls/docs/overview)
- [Cloud KMS](https://cloud.google.com/kms/docs)
- [Security Command Center](https://cloud.google.com/security-command-center/docs)
- [Cloud Armor](https://cloud.google.com/armor/docs)
- [Sensitive Data Protection](https://cloud.google.com/sensitive-data-protection/docs)
- [Best Practices for Enterprise Organizations](https://cloud.google.com/docs/enterprise/best-practices-for-enterprise-organizations)
- [GCP Security Foundations Blueprint](https://cloud.google.com/architecture/security-foundations)

### Video / Community
- Google Cloud Tech YouTube channel — security playlist
- Cloud Security Podcast by Google
- r/googlecloud subreddit — search for "security engineer" exam threads

### Quick Reference
- [Official Exam Guide](https://cloud.google.com/learn/certification/cloud-security-engineer)
- [Google Cloud Security Whitepapers](https://cloud.google.com/security/overview/whitepaper)

---

## Weekly Schedule Summary

| Week | Phase | Focus |
|------|-------|-------|
| 1 | 0 + 1 | Prerequisites + Configuring Access |
| 2 | 1 + 2 | Finish Access + Start Network Security |
| 3 | 2 | Network Security & Boundary Protection |
| 4 | 3 | Data Protection & Encryption |
| 5 | 4 | Operations, Logging, SCC |
| 6 | 5 | Compliance + Start Practice Exams |
| 7 | 6 | Practice Exams & Weak Area Review |
| 8 | 7 | Final Review & Exam |

---

*Good luck! Consistent daily study (1-2 hours) over 7-8 weeks is the sweet spot for most people passing this exam on the first attempt.*
