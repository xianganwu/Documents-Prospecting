# Campaign Content: Dick's Sporting Goods (AWX -> AAP)

## Target Personas

### 1. The Cloud Scalability Architect
**Role:** Cloud Architect, Principal DevOps Engineer, Azure Solutions Architect.
**Context:** Dick's Sporting Goods is heavily invested in **Microsoft Azure**, **Azure Arc**, and **AKS**. This person cares about consistent configuration across hybrid environments (cloud + retail stores).
**Likely Pain Points:**
- Managing "configuration drift" across thousands of retail locations.
- Complexity of stitching together Azure native tools with open source scripts.
**Value Proposition:**
- **AAP on Azure**: Native integration.
- **Automation Mesh**: Execute automation locally at retail stores while managing centrally.

### 2. The Automation Practitioner (Current AWX User)
**Role:** Senior Systems Engineer, DevOps Engineer, Site Reliability Engineer.
**Context:** Job descriptions confirm usage of **Ansible** and **Jenkins**. Likely managing an internal AWX instance that needs constant maintenance.
**Likely Pain Points:**
- "Who fixes AWX when it breaks?" (No support).
- Upgrading AWX is painful and breaks integrations.
- Lack of granular RBAC for different teams (Network vs. Cloud).
**Value Proposition:**
- **Certified Content**: Red Hat supported collections for Azure, Infoblox, ServiceNow.
- **Support**: 24/7 enterprise support instead of relying on forums.

### 3. The Executive & Security Leader
**Role:** CTO (Vlad Rak), VP of Infrastructure, Director of IT Operations, CISO.
**Context:** Focused on "Omnichannel Data" and "Security". Needs to ensure that automation scripts don't become a security vulnerability.
**Likely Pain Points:**
- **Audit Trails**: "Who changed that firewall rule?"
- **Compliance**: PCI-DSS compliance in retail stores.
**Value Proposition:**
- **Hardened Security**: System-hardened automation platform.
- **Policy as Code**: Enforce compliance standards automatically.

---

## Email Sequence

### Touch 1: The "Azure Scale" Approach
**Subject:** Scaling Ansible across your Azure & Retail footprint
**Target:** Cloud Architect / Automation Lead

> Hi [Name],
>
> I noticed Dick's Sporting Goods' impressive work with Azure Arc and your move toward a more adaptive cloud environment.
>
> Many retail enterprises utilizing Azure and Ansible run into a "Day 2" challenge: managing configuration consistency across thousands of distributed store locations without massive overhead.
>
> We work with retailers to transition from ad-hoc Ansible/AWX to the **Ansible Automation Platform on Azure**. It aligns your hybrid cloud strategy by:
> 1.  ** Decentralized Execution**: Run automation locally at stores via Automation Mesh, managed centrally.
> 2.  ** Event-Driven Response**: Trigger auto-remediation from your Confluent/Kafka data streams.
>
> Do you have 15 minutes next Tuesday to discuss how this fits your Azure roadmap?
>
> Best,
> [Your Name]

### Touch 2: The "AWX Risk" Approach
**Subject:** Is maintaining AWX slowing down your team?
**Target:** DevOps Lead / Manager

> Hi [Name],
>
> I know many teams start their automation journey with AWX. It’s a great tool, but eventually, the "free" cost is outweighed by the engineering time spent maintaining it.
>
> If your team is spending time fixing AWX upgrades or building custom integrations for Azure instead of automating core business value, it might be time to look at the **Red Hat Ansible Automation Platform**.
>
> We provide:
> *   **Long-term Support**: No more breaking changes on upgrades.
> *   **Granular RBAC**: Safely extend automation to non-IT/Store Operations teams.
> *   **Azure Certified Content**: Pre-built, supported modules for your cloud stack.
>
> Open to a brief chat to see the difference?
>
> Best,
> [Your Name]

### Touch 3: The "Executive Value" Approach
**Subject:** Reducing operational risk in your automation
**Target:** Director / VP Level

> Hi [Name],
>
> As Dick's Sporting Goods continues to innovate with data-driven retail experiences, the reliability of the underlying automation becomes critical.
>
> Reliance on community-supported tools (like open source Ansible AWX) for enterprise-critical infrastructure introduces operational risk—lack of support, security hardening, and auditability.
>
> **Ansible Automation Platform** offers the governance and security required for PCI-compliant retail environments, while accelerating your Azure adoption.
>
> Are you open to a high-level overview of how other major retailers are securing their automation supply chain?
>
> Best,
> [Your Name]
