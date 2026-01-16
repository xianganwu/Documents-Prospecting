# Sales Cheat Sheet: Qualifying Ansible for Meraki

**Use this guide to quickly determine if a prospect is a good fit for Ansible Automation Platform (AAP).**

---

## 🎯 The "Sweet Spot" (Ideal Customer Profile)
*   **Device Count:** 50+ Meraki Networks (or >100 devices total).
*   **Team Size:** 3+ Network Engineers (OR 1 drowning engineer).
*   **Environment:** Hybrid (Meraki + Data Center/Cloud) is the strongest fit.
*   **Pain:** Complains about "clicking too much," "audit failures," or "slow ticket times."

---

## ✅ Qualifying Questions (Green Flags)

### 1. Scale & Volume
*   "How much time does your team spend on 'maintenance' tasks like VLAN changes or SSID updates vs. new projects?"
    *   *Green Flag:* "Way too much," "It eats our weekends."
*   "When you open a new branch/store, is it a one-click process, or is someone copying-pasting configs?"
    *   *Green Flag:* "Manual copy-paste," "We use a spreadsheet."

### 2. Complexity & Hybrid
*   "Do you just manage Meraki, or do you also have Core Switches, Firewalls, or Cloud connectivity to worry about?"
    *   *Green Flag:* "We have Cisco CAT9ks in the campus," "We have AWS Direct Connect." (AAP bridges these silos).
*   "How do you keep your Meraki config in sync with your ticketing system (ServiceNow/Jira)?"
    *   *Green Flag:* "We don't," "It's manual entry."

### 3. Governance & Team
*   "If someone makes a change in the Meraki Dashboard at 2 AM, how do you know about it?"
    *   *Green Flag:* "We wouldn't know until something breaks." (Need for Drift Detection).
*   "Is the knowledge of how the network runs locked in one person's head (or script)?"
    *   *Green Flag:* "Yeah, if Dave leaves, we're in trouble."

---

## 🛑 Disqualifying Indicators (Red Flags)

### 1. Too Small / Too Simple
*   **< 50 Devices:** "We only have 10 coffee shops."
    *   *Why:* The Meraki Dashboard is perfect for this size. The ROI of AAP isn't there yet.
*   **Static Network:** "We set it up 2 years ago and haven't touched it since."
    *   *Why:* If they don't change configs, they don't feel the pain of manual work.

### 2. The "Solo Cowboy"
*   **The 1-Man Shop (Happy):** "I'm the only IT guy, I have 30 sites, and I love doing it manually because I control everything."
    *   *Why:* Strong resistance to standardization. "I don't need a platform, I know what I'm doing."
    *   *Exception:* If this 1 person is *unhappy* (drowning in work), they are a buyer.

### 3. No Budget / Open Source Only
*   "We use free Ansible AWX and have no need for support or RBAC."
    *   *Why:* They aren't ready to pay for the "Platform" features (Governance, Analytics, Support).

---

## 💡 The "Team Size" Nuance
*   **Team of 1-2:** Sell **Efficiency**. "Clone yourself. Do the work of 10 people."
*   **Team of 5-10:** Sell **Standardization**. "Stop everyone doing it their own way. Create a standard operating procedure."
*   **Team of 50+:** Sell **Governance**. "Audit logs, RBAC, and Compliance."
