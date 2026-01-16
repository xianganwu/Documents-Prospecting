# Visual Diagrams: The Automation Story

## 1. The "Before" State: Manual Chaos (The Problem)
This diagram illustrates the "Swivel Chair" management style. Notice how the Network Engineer is the bottleneck, manually interfacing with disparate tools (Meraki Dashboard, CLI, Cloud Console) with no centralized control.

```mermaid
graph TD
    User[Network Engineer]
    style User fill:#f9f,stroke:#333,stroke-width:2px

    subgraph "The Silos of Pain"
        MD["Meraki Dashboard"]
        CLI["Switch CLI (SSH)"]
        Cloud["AWS/Azure Console"]
        spreadsheet["Excel IP Tracker"]
    end

    User -- "Click, Click, Click (Manual)" --> MD
    User -- "Copy-Paste Configs" --> CLI
    User -- "Manual Route Tables" --> Cloud
    User -- "Update Manually" --> spreadsheet

    MD -- "Config Drift" --> Branch["Branch Office"]
    CLI -- "VLAN Check?" --> Core["Data Center"]
    
    style MD fill:#ffcccc,stroke:#f00
    style CLI fill:#ffcccc,stroke:#f00
    style Cloud fill:#ffcccc,stroke:#f00
    style spreadsheet fill:#ffcccc,stroke:#f00
```

---

## 2. The "After" State: Ansible Unity (The Solution)
Here, AAP becomes the "Single Control Plane." The Engineer defines the "Golden Config" once. AAP handles the complexity of talking to Meraki, Cisco IOS-XE, and Cloud APIs simultaneously.

```mermaid
graph TD
    User[Network Engineer]
    ServiceNow[ServiceNow Ticket]
    
    subgraph "Ansible Automation Platform"
        AAP[Automation Controller]
        Inventory[Dynamic Inventory]
        Playbook[Golden Config Playbook]
    end
    
    subgraph "Managed Infrastructure"
        Meraki["Meraki Cloud"]
        Core["Cisco IOS-XE Core"]
        Cloud["AWS Transit Gateway"]
    end

    User -- "Push Code (Git)" --> AAP
    ServiceNow -- "Trigger Job (Webhook)" --> AAP
    
    AAP -- "API Call" --> Meraki
    AAP -- "SSH/Netconf" --> Core
    AAP -- "API Call" --> Cloud

    Playbook --> AAP
    Inventory --> AAP

    style AAP fill:#ccffcc,stroke:#0f0,stroke-width:2px
    style Meraki fill:#e1f5fe
    style Core fill:#e1f5fe
    style Cloud fill:#e1f5fe
```

---

## 3. The Platform Journey: Long Term Strategy
This Gantt chart visualizes the roadmap. It shows that buying AAP isn't just for today's problem; it's a strategic investment for the next 18 months of transformation.

```mermaid
gantt
    title The Ansible Network Automation Journey
    dateFormat  YYYY-MM-DD
    axisFormat  %Q%Y

    section Phase 1: Quick Wins (Meraki)
    Setup AAP Controller       :done,    des1, 2024-01-01, 30d
    Automate Meraki SSIDs      :active,  des2, 2024-02-01, 45d
    Automate PSK Rotation      :         des3, after des2, 30d

    section Phase 2: Core Network
    Onboard Data Center (NX-OS) :         des4, 2024-04-01, 60d
    Standardize VLANs (IOS-XE)  :         des5, after des4, 60d
    
    section Phase 3: The Platform
    ServiceNow Integration      :         des6, 2024-08-01, 90d
    Self-Service Portal         :         des7, after des6, 60d
    Event-Driven Remediation    :         des8, 2025-01-01, 90d
```
