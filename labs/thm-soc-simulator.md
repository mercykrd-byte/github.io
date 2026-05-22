# Lab Writeup: TryHackMe SOC Simulator — Phishing Alert Triage
**Platform:** TryHackMe SOC Immersive Simulator  
**Date:** May 1, 2026  
**Category:** Phishing Analysis / Alert Triage  
**Difficulty:** Beginner–Intermediate  
**Tools Used:** TryHackMe SOC Sim (Alert Queue, SIEM, Case Reports, Analyst VM)

---

## Objective
Simulate a real SOC Tier 1 analyst workflow: review an incoming alert queue, investigate phishing-related alerts, correctly classify each as true positive or false positive, and write formal incident reports.

---

## What I Did

### Step 1 — Alert Queue Review
Opened the SOC Simulator alert queue and identified incoming phishing alerts. Prioritised alerts by severity (Medium → Low) before assigning myself to investigate.

### Step 2 — Alert 8814: Inbound Email with Suspicious External Link (True Positive)
**Alert details:**
- Subject: *"Action Required: Finalize Your Onboarding Profile"*
- Sender: `onboarding@hrconnex.thm`
- Recipient: `j.garcia@thetrydaily.thm`
- Link in body: `https://hrconnex.thm/onboarding/15400654060/j.garcia`
- Attachment: None

**Investigation steps:**
- Reviewed email content — impersonated an HR onboarding workflow to lure recipient into clicking a link
- Cross-referenced firewall/proxy logs via SIEM — URL was **blacklisted and blocked** when accessed
- Confirmed the recipient (j.garcia) had clicked the link; endpoint may have been exposed

**Classification: ✅ True Positive**

**Incident Report Summary:**
- Affected entity: `j.garcia@thetrydaily.thm`
- IOCs: sender domain `hrconnex.thm`, phishing URL `https://hrconnex.thm/onboarding/15400654060`
- Escalation: Yes — user clicked the link; endpoint investigation required
- Recommended actions: Block sender domain, scan j.garcia's endpoint, reset credentials, warn user

---

### Step 3 — Alert 1000: Suspicious Email from External Domain (False Positive)
**Alert details:**
- Subject: *"Inheritance Alert: Unknown Billionaire Relative Left You Their Hat Fortunes"*
- Sender: `eileen@trendymillineryco.me`
- Recipient: `support@tryhatme.com`
- Attachment: None
- Content: Classic advance-fee/inheritance scam narrative

**Investigation steps:**
- Reviewed email content — obvious social engineering scam, but no malicious links or attachments
- Alert note from SOC Lead indicated the detection rule still needed fine-tuning (unusual TLD trigger)
- No credential harvesting mechanism present; email did not meet threshold for confirmed threat

**Classification: ⚠️ False Positive**

**Closure Rationale:**
Alert triggered due to unusual `.me` TLD sender domain. Upon investigation, email contained no malicious links, no attachments, and no active credential harvesting attempt. While the content is a social engineering scam, it does not constitute a confirmed threat under current policy.

---

### Step 4 — Additional Alerts (1001–1004): Suspicious Email / Suspicious Parent-Child Relationship
Continued triaging the alert queue across multiple phishing and process-based alerts:
- Alerts 1001–1004 reviewed individually
- Applied consistent triage methodology: check datasource, sender, content, attachment, SIEM correlation
- Correctly classified and closed alerts as false positives where no active threat was confirmed

---

## Outcome
- **132 alerts closed** across the scenario
- **Mean time to resolve:** 25 minutes
- **Mean dwell time:** 30 minutes
- **Leaderboard position:** 3rd
- **Score:** 3,500 pts

---

## Key Skills Demonstrated
- Alert triage in a simulated SOC environment
- Phishing email analysis (sender, subject, link, content review)
- True positive vs. false positive classification with documented rationale
- Incident report writing (IOCs, affected entities, escalation decisions, remediation)
- SIEM log correlation to confirm URL blocking
- Working within a structured alert queue workflow

---

