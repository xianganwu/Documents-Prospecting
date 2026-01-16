# Cold Call / Discovery Script: Ansible for Meraki

**Persona:** Network Engineer / IT Manager
**Goal:** Uncover "ClickOps" pain and book a technical demo.

---

## 1. The Opening (Permission & Hook)
"Hi [Name], this is [Your Name] from Red Hat. I know I'm catching you in the middle of your day, but can I take 30 seconds to tell you why I'm calling? If it's not relevant, you can hang up."

**(If yes):**
"Great. I speak with a lot of Meraki users who love the dashboard but struggle when they have to make the same change across 50 or 100 sites. They call it 'ClickOps fatigue.' Is that something you guys run into, or is everything fully automated?"

---

## 2. Discovery (Digging for Pain)
*If they say "Yeah, it's manual":*
"I hear that a lot. Just so I understand the scale—how many separate networks or sites are you managing right now?"

*   **Drill Down Questions:**
    *   "When you launch a new site, is it a copy-paste job, or do you have to manually configure each one?"
    *   "How do you handle security updates, like rotating Wi-Fi passwords? Do you do that regularly, or is it too much hassle?"
    *   "Do you have non-Meraki gear in the mix too, like in the Data Center? How do those talk to the Meraki environment?"

---

## 3. The Pivot (The "Aha" Moment)
"It sounds like you're spending a lot of skilled engineering time on repetitive admin work. The reason I'm calling is that **Ansible Automation Platform** connects directly to the Meraki API to remove that manual work."

"Instead of clicking through the dashboard for every site, you define your 'Golden Config' once, and Ansible ensures every single site matches it instantly."

---

## 4. The Ask (Closing for Next Step)
"I'm not trying to sell you anything right now. But I would love to show you a 15-minute demo of Ansible literally configuring a Meraki network in seconds. It might give you some hours back in your week."

"Do you have time next Tuesday or Wednesday morning?"

---

## 5. Objection Handling

**Objection:** "Meraki is already easy. We don't need automation."
**Response:** "I agree, the dashboard is great for *visibility*. But what about *consistency*? How do you ensure that a change made in the London office allows the same policy as the New York office without checking manually? Ansible gives you that audit trail and consistency."

**Objection:** "We just use Python scripts for that."
**Response:** "That's awesome—you're ahead of the curve. The challenge with scripts is usually scaling them and governance—who ran the script, and what did it change? Ansible takes your Python logic but wraps it in enterprise permissions, logging, and scheduling. It turns your scripts into a platform."

**Objection:** "We don't have the budget."
**Response:** "Totally understand. For now, I'd just like to share the *art of the possible* so you know what's out there. No pressure to buy. Would you be open to just seeing how the 'Drift Detection' works?"
