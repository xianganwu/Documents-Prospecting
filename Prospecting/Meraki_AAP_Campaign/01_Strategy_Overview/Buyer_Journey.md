# Buyer Journey: From Meraki Islands to Enterprise Automation

## Executive Summary
This document maps the customer journey for a Cisco Meraki shop transitioning to Ansible Automation Platform (AAP). The goal is to move them from "ClickOps" (manual dashboard management) to automated, scalable network operations that bridge the gap between Edge, Campus, and Data Center.

---

## 1. The Persona Landscape

### 🦖 The Network Engineer (Ops)
*   **Current State:** Loves the Meraki Dashboard for its simplicity but hates the repetitive tasks (e.g., updating 500 branch firewalls).
*   **Sentiment:** "Automation is scary/hard" or "Why do I need Ansible if I have the Meraki Dashboard?"
*   **Goal:** Save time, avoid weekend work windows, stop being the bottleneck.

### 💼 The IT Director / CIO (Strategy)
*   **Current State:** Worried about security compliance (Audit failures), "Shadow IT" configurations, and vendor lock-in.
*   **Sentiment:** "We need a standard operating environment." "How do we govern this scaling network?"
*   **Goal:** Compliance, Speed to Market, Cost reduction (Opex), Unified view.

### 🚀 The DevOps Lead (Cloud/Speed)
*   **Current State:** Frustrated that Network Ops is slow and tickets take days.
*   **Sentiment:** "Everything else is code, why is the network manual?"
*   **Goal:** Network as Code, CI/CD integration, Self-service portals.

---

## 2. The Journey Stages

### Stage 1: Awareness (The Scalability Trap)
**Trigger:** The specific moment "Easy" becomes "Hard".
*   **Context:** The organization grows from 5 key sites to 50 or 500. The "simple" Meraki Dashboard is now a burden where they click the same button 500 times.
*   **Symptoms:**
    *   **Configuration Drift:** "Why is the guest Wi-Fi enabled in the London office but blocked in NY?"
    *   **Security Gaps:** Pre-Shared Keys (PSKs) haven't been rotated in 2 years because it’s "too hard."
    *   **Siloed Operations:** The Data Center team uses CLI, the Edge team uses Meraki Dashboard. No single source of truth.
*   **AAP Hook:** "Stop 'ClickOps'. Manage 1,000 networks as easily as one."

### Stage 2: Consideration (The Search for Standards)
**Trigger:** A security audit failure or a failed manual rollout.
*   **Context:** They are looking for scripting solutions. They might be dabbling with Python scripts or random GitHub actions.
*   **Key Questions:**
    *   "How do we standardize VLAN layouts across all retail stores?"
    *   "Can we integrate Meraki alerts with ServiceNow?"
    *   "How do we manage our non-Meraki gear (Cisco IOS-XE, Juniper, Arista) alongside Meraki?"
*   **AAP Hook:** "Don't build technical debt with loose scripts. Build a platform. Use Ansible to orchestrate the *process*, not just the *device*."

### Stage 3: Decision (The Enterprise Transformation)
**Trigger:** Budget planning or a major digital transformation initiative.
*   **Context:** Evaluating AAP vs. DIY or other commercial tools.
*   **Buying Criteria:**
    *   **Scalability:** Can it handle 10k devices?
    *   **Governance:** Who changed what and when? (RBAC & Logging)
    *   **Hybrid Support:** We need to manage Meraki AND our legacy Data Center.
*   **AAP Hook:** "Ansible is the glue. It connects ServiceNow to Meraki, Meraki to the Security Operations Center (SOC), and the Edge to the Cloud. It provides a single pane of glass for *automation*."

---

## 3. The "Better Together" Story (Meraki + AAP)

| Feature | Meraki Native (Good) | Meraki + AAP (Better) | Business Value |
| :--- | :--- | :--- | :--- |
| **Configuration** | Dashboard GUI | Infrastructure as Code (Playbooks) | Version control, rollback capability, peer review. |
| **Scale** | Templates (Rigid) | Jinja2 Templates (Dynamic) | Infinite flexibility based on site logic (e.g., Store size, Region). |
| **Processes** | Manual API calls | ITSM Integration (ServiceNow) | Zero-touch ticketing, auto-approval workflows. |
| **Scope** | Just Meraki | **Everything** (Cisco, Cloud, Linux) | one team, one language, unbroken workflows. |

---

## 4. Key Takeaway
**"Meraki made the network simple. Ansible makes your operations scalable."**
