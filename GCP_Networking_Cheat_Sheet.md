# GCP Networking - Architecture & Security Cheat Sheet

---

## 1. VPC Fundamentals

- VPCs are globally scoped
- Subnets are regional
- Routing inside a VPC is automatic and global
- Different VPCs cannot communicate via internal IP unless:
  - VPC Peering
  - VPN
  - Interconnect
  - Multi-NIC routing
- **Isolation is enforced at the VPC level** — not subnet, region, or zone

---

## 2. Firewall Model (Critical Difference from AWS)

- Firewall rules are defined at the VPC level
- Applied selectively via:
  - Network tags
  - Service accounts (preferred)
- Stateful
- Evaluated by priority (lower number = higher priority)
- Implied rules:
  - Allow egress all
  - Deny ingress all

**Best Practice:**
- Use service accounts, not tags
- Avoid broad RFC1918 CIDR rules
- Enable firewall logging
- Use hierarchical firewall policies for guardrails

---

## 3. Routing Model

### Route Types

| Type | Created By | Purpose |
|------|-----------|---------|
| Subnet routes | Automatic | Internal VPC connectivity |
| Custom static routes | You | Force traffic to specific next hop |
| Dynamic routes | Cloud Router (BGP) | Hybrid connectivity |

### Route Selection Rules

- Most specific prefix wins
- Local subnet routes preferred
- Default route = 0.0.0.0/0
- **Default routes are NOT exported over peering**
- Peering never exports 0.0.0.0/0

---

## 4. VPC Peering

**What It Is:**
- Private RFC1918 connectivity between VPCs
- Decentralized control
- Non-transitive
- No overlapping CIDRs allowed

**Automatically Shared:** Subnet routes

**Optional:** Custom routes, Dynamic routes

**Risks:**
- Expands internal trust boundary
- Hybrid routes can leak reachability
- Firewall drift across peers

**Use When:**
- Two autonomous environments need connectivity
- Cross-organization private connectivity
- M&A integration
- Producer-consumer VPC model

---

## 5. Shared VPC

**What It Is:**
- Centralized networking model
- Host project owns network
- Service projects deploy workloads

**Governance Model:**
- Org Admin -> grants XPN Admin
- Shared VPC Admin -> attaches service projects
- Service Project Admin -> needs `compute.networkUser`

**Benefits:**
- Central firewall control
- Central NAT
- Central hybrid connectivity
- Central logging
- Separation of duties

**Risk:**
- Single routing domain
- Segmentation depends on firewall rules

> Shared VPC = governance | Peering = connectivity

---

## 6. Multi-NIC VMs

**What It Does:**
- Connects a VM to multiple VPCs
- Each NIC attaches to different VPC
- Only directly connected subnets get routes
- Only one default route (nic0)

**Does NOT Automatically:**
- Provide transit routing
- Provide policy routing
- Bridge networks

**Used For:**
- Network Virtual Appliances
- IDS/IPS
- Transit architectures
- Control plane vs data plane separation

**Risk:**
- Moves enforcement into guest OS
- High blast radius if compromised
- Increases compliance scope

---

## 7. Network Service Tiers

### Premium (Default)
- Global Anycast IP
- Uses Google backbone
- Supports global load balancing
- 99.99% SLA
- Required for Cloud CDN & global LB

### Standard
- Regional external IP only
- Traffic stays longer on ISP networks
- 99.9% SLA
- Lower cost

> Tier affects external IP internet traffic only. Not affected: internal VPC traffic, VPN, Interconnect, Peering.

---

## 8. Global Load Balancing

- Single global anycast IP
- Multi-region backend support
- Edge termination
- Automatic failover
- Requires Premium tier

**Traffic path:** User -> Closest Google POP -> Google backbone -> Backend

**Security Advantage:**
- Cloud Armor at edge
- Global DDoS absorption
- Central TLS termination

---

## 9. VM Network Migration

- Must stop VM (cold migration)
- Cannot migrate MIG instances
- Cannot migrate to legacy network
- MAC address changes
- Internal IP changes if subnet CIDR differs
- External IP can be retained (with permissions)

> Architectural lesson: Immutable infrastructure > migrating instances.

---

## 10. Hybrid Connectivity

### VPN
- Encrypted over internet
- HA VPN recommended
- Uses Cloud Router (BGP)

### Interconnect
- Private circuit
- High throughput
- Lower latency

**Hybrid Risk:**
- Route export over peering can expand trust
- CIDR overlap breaks routing
- On-prem flat networks expand attack surface

---

## 11. Default Route Behavior

- Default route: `0.0.0.0/0`
- Matches all IPv4
- Used when no more specific route matches
- **Not exported over peering**
- Can be overridden with custom route

---

## 12. Common Exam Signals

| Question Mentions | Likely Answer |
|-------------------|---------------|
| Central governance | Shared VPC |
| Cross-org connectivity | VPC Peering |
| Single global IP | Global LB (Premium) |
| Lowest latency global users | Premium tier |
| Force egress inspection | Custom route + NVA |
| Multi-team enterprise | Shared VPC |
| Transit between VPCs | Not peering alone |
| Reduce cost, regional only | Standard tier |

---

## Mental Models

- **VPC** = hard routing boundary
- **Peering** = expands reachability
- **Shared VPC** = centralizes governance
- **Multi-NIC** = shifts enforcement into OS
- **Premium tier** = path control
- **Default route** = least specific
- **Hybrid** = trust boundary expansion
- **Most specific route always wins**

---

## Security Architecture Principles

- Prefer control-plane enforcement over appliance enforcement
- Avoid trusting all RFC1918 ranges
- Plan CIDRs early for hybrid
- Use service accounts for segmentation
- Separate prod and non-prod routing domains where possible
- Be explicit about route export/import
- Watch for blast radius expansion when peering
