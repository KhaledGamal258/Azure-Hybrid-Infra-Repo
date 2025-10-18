# **Azure Cloud Infrastructure and Hybrid Management Project**

![Azure Architecture Diagram](Azure Final Project.drawio3.png)

---

### 🌐 Overview  
This project demonstrates the design and implementation of a **secure, cloud-native Azure infrastructure** seamlessly integrated with **hybrid management** capabilities.  
It mirrors enterprise-grade patterns that align with Microsoft Cloud Adoption Framework (CAF) — focusing on **network segmentation, zero-trust security, resilience, and hybrid extensibility.**

It’s a real-world showcase of skills gained from **AZ-104, AZ-700, AZ-500, AZ-900, and AZ-800**, translating theory into a working, production-like cloud deployment that can scale, self-heal, and operate with confidence.

---

## 🏗️ Architecture Summary  

**Core Components:**
- **VNet2** — logically segmented Azure Virtual Network with multiple subnets (BackSub, AzureFirewallSubnet, GatewaySubnet)
- **Network Security Group (NSG)** — subnet-level policy enforcement for ingress/egress control
- **Application Security Group (ASG)** — resource grouping for scalable access control
- **Azure Firewall** — centralized security appliance with DNAT, application, and network rules
- **Standard Load Balancer** — distributes web traffic for high availability and fault tolerance
- **Private DNS Zone** — internal name resolution for VMs and hybrid systems
- **Azure Files + Service Endpoint** — private, high-speed access to Azure storage over Azure backbone
- **Defender for Cloud** — continuous compliance, vulnerability detection, and security posture management
- **Windows Admin Center (WAC)** — unified hybrid management for Azure and on-prem systems through **VPN Gateway + GatewaySubnet**

**Architecture Layers:**  
Networking • Security • Compute • Storage • Monitoring • Hybrid Management

---

## 🧠 Architectural Rationale & Impact

### **1. Virtual Network Design & Subnet Segmentation**
A well-structured network is the foundation of secure cloud architecture. VNet2 was designed to host specific subnets: 
- **BackSub:** Application tier and internal resources
- **AzureFirewallSubnet:** Dedicated to Azure Firewall for centralized policy enforcement
- **GatewaySubnet:** Reserved for VPN Gateway, enabling secure hybrid connectivity

**Impact:**
- Logical isolation between workloads and management layers
- Enhanced visibility and traffic control
- Seamless extension to on-prem without overlapping IP spaces

---

### **2. NSG + ASG: Intelligent Access Control**
**Why NSG + ASG?** Instead of manually creating NSGs for each VM or IP, we use **ASGs** to group workloads (e.g., web servers). NSGs then reference ASGs to apply role-based security policies.

**Example:** RDP access is allowed **only** to the Web ASG. When adding future VMs (SQL, App, etc.), RDP remains restricted to Web ASG members only.

**Impact:**
- Simplified access management and scalability
- Reduced operational overhead for security updates
- Better adherence to least privilege and zero trust principles

---

### **3. Azure Firewall & Policy Layer**
A dedicated **Azure Firewall Subnet** hosts a next-generation Azure Firewall that governs all outbound and inbound traffic.

**Configuration Highlights:**
- **DNAT Rule:** Redirects inbound traffic from the internet to the Load Balancer
- **Application Rule:** Allows web servers to access only `www.microsoft.com` and `www.google.com`
- **Network Rule:** Enables communication with Private DNS Zone

**Impact:**
- Unified control and audit over traffic
- Prevention of unauthorized external communications
- Supports scalability without additional NSG complexity

---

### **4. Standard Load Balancer & Web Tier Resilience**
The Load Balancer (Standard SKU) provides **high availability** and **scalability** for the web application layer. It monitors health via HTTP probe on port 80 and balances traffic evenly across backend pool members.

**Impact:**
- Zero-downtime updates and failover handling
- Enhanced reliability under load
- Network-level redundancy compliant with Microsoft’s best practices

---

### **5. Private DNS & Controlled Name Resolution**
A **Private DNS Zone** was implemented for internal FQDN resolution (e.g., `web01.backsub.local`). This integrates with the Firewall’s network rules, allowing only defined internal lookups.

**Impact:**
- Prevents data leakage via public DNS queries
- Enables service discovery across hybrid networks
- Critical for scalable internal applications and hybrid identity resolution

---

### **6. Azure Files with Service Endpoint**
An **Azure Storage Account** hosts file shares accessed privately through Service Endpoints (`Microsoft.Storage`). The drive `S:` is mapped to web servers using PowerShell automation.

**Impact:**
- Eliminates internet exposure to storage
- Delivers low-latency data access
- Simplifies shared content management between web servers

---

### **7. Defender for Cloud & Security Posture Management**
Defender for Cloud continuously evaluates configurations and surfaces actionable security recommendations.

**Impact:**
- Identifies misconfigurations before exploitation
- Enhances governance and compliance tracking
- Strengthens security baseline per CIS and NIST frameworks

---

### **8. Hybrid Management via VPN Gateway & WAC**
A **GatewaySubnet** was added for the **VPN Gateway**, linking on-prem ADM1 and Azure for hybrid control. WAC was registered in Azure via Entra ID, enabling secure, centralized management.

**Benefits of WAC Integration:**
- Remote administration of Azure and on-prem servers
- Single pane of glass for patching, monitoring, and performance tuning
- Hybrid-aware management without public IP dependencies

**Impact:**
- Bridges the operational gap between legacy infrastructure and Azure
- Ensures secure management aligned with least-privilege access
- Real-world reflection of AZ-800 hybrid management capabilities

---

## 🧩 Key Technical Highlights

| Category | Service | Key Skills Demonstrated | Business Impact |
|-----------|----------|--------------------------|-----------------|
| Networking | VNets, Subnets | Segmentation, address planning | Predictable traffic flow and control |
| Access | NSG, ASG | Role-based isolation | Reduces exposure and complexity |
| Security | Azure Firewall | DNAT, Application Rules | Centralized visibility and control |
| Compute | VMs + Load Balancer | HA configuration | Fault tolerance and scalability |
| Storage | Azure Files + Endpoint | Secure private access | Zero data exfiltration risk |
| Monitoring | Defender for Cloud | Threat detection | Continuous improvement loop |
| Management | WAC + VPN Gateway | Hybrid orchestration | Unified management experience |

---

## 🔍 Validation & Testing

| Test Scenario | Expected Outcome | Result |
|----------------|------------------|---------|
| Load Balancer Health Probe | Balanced HTTP responses | ✅ Success |
| Firewall DNAT | Internet → Load Balancer redirection | ✅ Success |
| FQDN Filtering | Outbound limited to Microsoft/Google | ✅ Success |
| Private DNS Lookup | Resolved internal FQDNs only | ✅ Success |
| File Share Access | Accessible via Service Endpoint | ✅ Success |
| WAC Remote Management | Secure VPN-based connectivity | ✅ Success |

All logs aggregated into **Azure Monitor** for correlation and alerting.

---

## 💡 Professional Reflection

This project exemplifies **architecture-driven thinking** — not just deploying resources, but orchestrating them to behave predictably, securely, and intelligently.  
Each Azure component was selected for *purpose and impact*:

- **ASG + NSG:** Simplified access governance at enterprise scale.
- **Azure Firewall:** Policy-driven control and monitoring, centralizing trust.
- **Private DNS + Service Endpoint:** Data kept private by design.
- **WAC + VPN Gateway:** Operational excellence bridging cloud and on-prem.

> The real success of a Cloud Engineer isn’t in provisioning — it’s in designing systems that thrive under change.  
This project proves the mindset of a **Cloud Architect who builds for resilience, manages for clarity, and secures for tomorrow.**

---

## 📂 Repository Structure
```
docs/
  Azure_Implementation_Plan.docx
  architecture-diagram.drawio
  screenshots/
scripts/
  map-drive.ps1
  connectivity-tests.ps1
README.md
```

---

## 🧰 Tech Stack
**Azure IaaS**, **NSG**, **ASG**, **Azure Firewall**, **Standard Load Balancer**, **Private DNS**, **Azure Files**, **Service Endpoints**, **Defender for Cloud**, **VPN Gateway**, **Windows Admin Center (WAC)**, **Entra ID**

---

## 👨‍💻 Author
**Khaled Gamal Osman**  
Azure & Systems Administrator | Cloud Infrastructure Engineer  
[LinkedIn](https://www.linkedin.com/in/khaled-gamal-osman) | [Wuzzuf](https://wuzzuf.net/me/Khaled-Gamal-c7eafed4c6)  

---

> *"Cloud isn’t about servers or VMs — it’s about engineering trust, visibility, and evolution into every packet."*
