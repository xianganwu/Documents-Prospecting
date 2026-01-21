# EPAM Discovery Guide

## Purpose
To help the matrix team uncover high-value opportunities by asking the right questions to the right people. Focus on **Business Outcomes** (margin, speed, risk), not just technical features.

## 1. Discovery for Client Delivery Leaders (VPs, Practice Leads)
*Goal: Identify opportunities to embed AAP into their service offerings.*

*   "How do you currently handle day-zero setup for new client environments? Is that a manual effort for each billable engineer, or is it automated?"
*   "Are you seeing pressure from clients to speed up 'time-to-first-feature'? How is your infrastructure provisioning process affecting that delivery timeline?"
*   "Do you have a standard 'Landing Zone' or 'Golden Image' that you deploy for every new project? How do you maintain it?"
*   "How are you handling compliance evidence for your enterprise clients? Is it a scramble at audit time, or is it continuous?"

## 2. Discovery for Platform/IT Leaders (Internal)
*Goal: Identify opportunities to improve EPAM's own efficiency.*

*   "How much time do your senior architects spend on 'keeping the lights on' vs. designing new solutions?"
*   "With the variety of clouds and tools you support (AWS, Azure, VMWare), how do you get a single view of what's running where?"
*   "Are you facing disparate automation islands (e.g., one team using Terraform, another using Python scripts)? How does that impact your ability to scale resources?"
*   "What is your strategy for self-service IT? Can a developer request a new environment and get it automatically, or is there a ticket queue?"

## 3. Objection Handling

**"We already use the free version of Ansible."**
*   *Response:* "That's great! It means your team knows the language. AAP isn't about replacing the language; it's about adding the **enterprise control plane**—RBAC, Analytics, and trusted content—that allows you to scale that usage safely across hundreds of clients without adding more headcount."

**"Our clients choose the tools; we just use what they have."**
*   *Response:* "Understood. However, many SIs find that bringing their own 'Automation Toolkit' allows them to deliver faster and with higher quality, which is a competitive advantage. Plus, AAP can integrate with whatever tools your clients have, acting as the orchestrator."

**"We are heavy on Terraform/Jenkins."**
*   *Response:* "AAP plays well with others! We can orchestrate Terraform plans or be triggered by Jenkins pipelines. Think of AAP as the 'last mile' automation that does the configuration management and OS-level work that those tools often miss."
