# Why Ansible Automation Platform for Bloomberg?
## The Unifying "Orchestrator of Orchestrators"

### The Challenge: Tool Sprawl & Operational Tax
Bloomberg's infrastructure is currently managed by a fragmented set of tools: **Bladelogic** (Legacy/Compliance), **Chef** (Linux/Config), **Salt** (Event-Driven/Speed), and **Ansible Core** (Ad-hoc).

This fragmentation creates an **"Operational Tax"**:
*   **Siloed Teams**: Chef experts can't debug Bladelogic failures.
*   **Fragmented Audit**: No single "source of truth" for who changed what across the fleet.
*   **Hiring Friction**: Finding engineers who know *all* these specific tools (plus their languages: Ruby, Python, NSH) is difficult.

### The Solution: Unified Orchestration NOT "Rip and Replace"
We don't propose replacing every Chef recipe or Salt state tomorrow. We propose **Ansible Automation Platform (AAP)** as the **Single Control Plane** that sits *above* these tools.

| Feature | Legacy Approach (Chef/Salt/Bladelogic) | Ansible Automation Platform Approach |
| :--- | :--- | :--- |
| **Execution** | **Disconnected**: Separate CLIs, separate consoles. | **Unified**: One API/UI to trigger Chef client runs, Bladelogic jobs, or Ansible playbooks. |
| **Governance** | **Fragmented**: 4 different audit logs. | **Centralized**: A single "System of Record" for all automation activity. |
| **Language** | **High Barrier**: Requires Ruby (Chef), Python (Salt), or NSH (Bladelogic). | **Democratized**: YAML is readable by Ops, NetEng, and Security teams alike. |
| **Cloud** | **Agent-Based**: Struggled with ephemeral cloud scaling (B-PIPE use cases). | **Agentless & Event-Driven**: Native AWS/Azure integration; auto-remediates based on events. |

### Key Business Outcomes

#### 1. Zero-Trust Automation (Security)
Instead of giving sudo access to junior engineers so they can run Chef knife commands, give them **Role-Based Access (RBAC)** in AAP. They can push a button to "Remediate Server," but they can't log in or change the underlying code.
**Result**: Reduced insider threat and accidental outages.

#### 2. Acceleration of B-PIPE to Cloud (Agility)
Agents break in the cloud. AAP's **Event-Driven Ansible** (EDA) connects natively to AWS CloudWatch and Azure Monitor. When a market data feed experiences latency, EDA can trigger remediation within milliseconds—without waiting for a polling interval.
**Result**: Higher uptime for critical market data feeds.

#### 3. Operational Efficiency (Cost)
Stop maintaining 4 different management servers. Consolidate the *invocation* layer. Let Ansible handle the "Day 2" interactions, while legacy tools fade into the background until they can be retired naturally.
**Result**: Reduced licensing costs and consolidated engineering focus.
