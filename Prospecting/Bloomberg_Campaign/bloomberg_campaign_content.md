# Bloomberg Campaign: The "Orchestrator of Orchestrators"

## Target Personas

### 1. The "Cloud Acceleration" Lead (B-PIPE/Market Data)
**Role:** Head of Market Data Infrastructure, Cloud Architect, B-PIPE Engineering Lead.
**Context:** Responsible for moving high-speed market data infrastructure to AWS/Azure (Private Link).
**Pain Points:**
- Legacy tools (Bladelogic/Chef) are too slow or rigid for dynamic cloud auto-scaling.
- Needs to bridge on-prem ticking plants with cloud regions seamlessly.
**Messaging Hook:** "Event-Driven Automation" – reacting to cloud events (e.g., AWS CloudWatch alerts) to trigger Ansible remediation.

### 2. The "Platform Engineering" Manager
**Role:** Engineering Manager - Infrastructure, Head of SRE.
**Context:** Managing a fragmented team where some know Chef (Ruby), others know Salt (Python), and others know Bladelogic.
**Pain Points:**
- **Onboarding Tax**: Takes months to teach a new hire the "Bloomberg way" of mixing 4 tools.
- **Silos**: The Chef team can't help the Bladelogic team during an outage.
**Messaging Hook:** "Standardization" – Use Ansible as the common language (YAML) to orchestrate the others, lowering the barrier to entry.

### 3. The Security & Compliance Director (CISO Office)
**Role:** Director of InfoSec, Compliance Officer.
**Context:** Financial constraints. Needs absolute proof of who changed what, when, and where.
**Pain Points:**
- **Audit Hell**: Correlating logs from Bladelogic, Chef server, and random SSH scripts.
- **Role-Based Access**: "Why does the junior dev have root access via Salt?"
**Messaging Hook:** "Unified Governance" – One control plane (AAP) to enforce policy across all underlying tools.

---

## Email Templates

### Sequence A: The "Unification" Play (Target: Platform Manager)
**Subject:** Managing Chef, Salt, and Bladelogic... involves too much overhead?

> Hi [Name],
>
> I talk to many financial services engineering leads who feel "trapped" by tool sprawl. You have Chef for Linux, Bladelogic for legacy compliance, and Salt for specific event-driven tasks.
>
> The overhead of maintaining expertise in *all* of these is a tax on your team's velocity.
>
> We aren't suggesting you "rip and replace" everything tomorrow. Instead, Bloomberg can use the **Ansible Automation Platform** as an "Orchestrator of Orchestrators."
>
> *   **Uniform Interface**: A single API/UI to launch Chef runs, Bladelogic jobs, and Ansible playbooks.
> *   **Single Audit Trail**: One place to see *who* ran *what*, regardless of the underlying tool.
> *   **Democratized Access**: Allow L1 support to safely run complex Chef cleanups without needing root access (or Ruby knowledge).
>
> Do you have 15 minutes to see how this "overlay" approach works?
>
> Best,
> [Your Name]

### Sequence B: The "Cloud Agility" Play (Target: Cloud/B-PIPE Lead)
**Subject:** Automation speed for B-PIPE's cloud expansion

> Hi [Name],
>
> I see Bloomberg is aggressively expanding B-PIPE connectivity into AWS and Azure via Private Link.
>
> Traditional agents like Bladelogic or Chef often struggle with the ephemeral nature of cloud resources. Waiting for a convergence run can be too slow when market data latency is on the line.
>
> **Ansible Automation Platform** is helping firms bridge this gap with **Event-Driven Ansible**.
>
> Imagine:
> 1.  AWS CloudWatch detects a latency spike.
> 2.  It triggers an Ansible Rulebook.
> 3.  Ansible instantly remediates the configuration or scales the node—no human intervention, no waiting for a Chef client check-in.
>
> Worth a brief conversation on how this fits your cloud roadmap?
>
> Best,
> [Your Name]

### Sequence C: The "Governance" Play (Target: Security/Compliance)
**Subject:** Auditing your "mixed bag" of automation tools

> Hi [Name],
>
> In a complex environment like Bloomberg's, "who changed that setting?" is often the hardest question to answer.
>
> With a mix of Chef, Salt, and ad-hoc scripts, your audit logs are fragmented. This creates risk.
>
> **Ansible Automation Platform** provides a centralized "Governance Plane" that sits above your technical execution tools.
>
> *   **RBAC**: Enforce strict controls on *who* can trigger automation, even if the underlying script is a sudo-level shell script.
> *   **Policy as Code**: Scan environments for compliance (CIS benchmarks) and auto-remediate drift.
> *   **Central Logging**: One dashboard for Internal Audit.
>
> Can we schedule a brief demo of the Governance/Analytics dashboard?
>
> Best,
> [Your Name]

---

## LinkedIn InMail Scripts

### Script 1: To the "Chef/Salt" Expert
"Hi [Name], impressive background with [Chef/Salt]. I'm seeing a trend where teams keep those tools but wrap them in Ansible Automation Platform to allow self-service for non-experts. Would love to share how that reduces 'ticket ops' for your team."

### Script 2: To the Engineering Manager
"Hi [Name], managing a team across Bladelogic, Chef, and Salt sounds like a hiring challenge. We're helping teams use Ansible as a 'common language' layer to simplify onboarding and operations. Open to swapping ideas?"

### Script 3: To the Cloud Architect
"Hi [Name], saw your work on the B-PIPE cloud migration. Are you finding legacy agent-based tools (Chef/Bladelogic) fast enough for your AWS auto-scaling groups? We have a new Event-Driven solution that might interest you."
