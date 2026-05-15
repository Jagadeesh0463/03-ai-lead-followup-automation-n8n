# AI Lead Follow-up Automation using n8n

An AI-powered lead management workflow built using **n8n**, **Groq LLM**, **Gmail**, and **Google Sheets**.

The system automatically captures leads, stores them, sends acknowledgment emails, performs timed follow-ups, checks lead status dynamically, and escalates inactive leads.

---

# Problem Statement

Leads are often not followed up quickly enough.

Delayed responses can reduce:

- meeting bookings
- conversion rates
- sales opportunities
- customer engagement

This workflow solves that problem by automating:

Lead capture → acknowledgment → follow-up → escalation

---

# Overview

This workflow automatically:

✅ Captures new leads through a webhook  
✅ Stores lead data in Google Sheets (lightweight CRM)  
✅ Sends AI-generated acknowledgment emails  
✅ Waits 1 hour before checking lead status  
✅ Sends AI-generated follow-up reminders  
✅ Waits 24 hours before escalation  
✅ Rechecks latest lead status dynamically  
✅ Sends escalation alerts for inactive leads  

Designed for:

- Sales teams
- Agencies
- Freelancers
- Consultants
- Real estate businesses
- Coaching businesses
- Customer onboarding systems

---

# Features

- Lead capture automation
- AI acknowledgment email generation
- CRM tracking using Google Sheets
- Timed follow-up sequences
- Dynamic state-aware automation
- Escalation workflows
- Internal alerts
- Groq LLM email generation
- End-to-end automation

---

# Workflow Architecture

```text
Lead Capture Webhook
        ↓
Set Lead Fields
        ↓
Store Lead in CRM
        ↓
Generate Acknowledgment Email
        ↓
Send Acknowledgment Email
        ↓
Wait 1 Hour Before Follow-up
        ↓
Check Lead Status
        ↓
Did Customer Respond?
     ├── YES → END
     │
     └── NO
            ↓
     Generate Follow-up Email
            ↓
     Send Follow-up Reminder
            ↓
     Wait 24 Hours Before Escalation
            ↓
     Check Lead Status After 24 Hours
            ↓
     Did Customer Respond?
          ├── YES → END
          │
          └── NO
                 ↓
          Send Escalation Alert
```

---

# Workflow Screenshot

## Full Workflow

![Workflow](screenshots/lead-followup-workflow.png)

---

# Technologies Used

| Tool | Purpose |
|------|----------|
| n8n | Workflow automation |
| Groq LLM | AI email generation |
| Gmail API | Sending emails |
| Google Sheets | Lead tracking CRM |
| Webhook | Lead capture endpoint |

---

# AI Processing

Groq LLM generates:

- acknowledgment emails
- follow-up emails

Model used:

```text
groq/compound-mini
```

Email generation requirements:

- Professional tone
- Under 80 words
- Friendly response
- Context aware

---

# Google Sheets CRM Structure

Create sheet:

```text
Lead Tracker
```

Columns:

```text
name
email
service
status
created_at
```

Recommended additional columns:

```text
follow_up_sent
escalated
last_contacted
```

---

# Lead States

Possible status values:

```text
new
responded
closed
```

Workflow checks:

```text
status != responded
```

before sending reminders.

---

# Email Sequence

## 1) Acknowledgment Email

Sent immediately:

```text
Thanks for contacting us
```

Purpose:

- confirm lead received
- build trust
- reduce response delay

---

## 2) Follow-up Reminder

Sent after:

```text
1 hour
```

if:

```text
status != responded
```

---

## 3) Escalation Alert

Sent after:

```text
24 hours
```

if customer remains inactive.

Escalation includes:

- lead name
- email
- requested service
- submission timestamp

---

# Setup Instructions

## 1. Clone Repository

```bash
git clone https://github.com/your-username/ai-lead-followup-automation-n8n.git
cd ai-lead-followup-automation-n8n
```

---

## 2. Import Workflow

Open n8n:

```text
Workflows
↓
Import
↓
Select workflow JSON
```

Import:

```text
github_ready_lead_followup_workflow.json
```

---

## 3. Configure Credentials

Connect:

- Gmail OAuth
- Google Sheets OAuth
- Groq API

---

## 4. Replace Placeholders

Update:

```text
{{YOUR_EMAIL}}

{{DOCUMENTID}}

{{SHEETNAME}}

{{WEBHOOK_PATH}}

{{GMAILOAUTH2_ID}}

{{GOOGLESHEETSOAUTH2API_ID}}

{{GROQAPI_ID}}
```

---

## 5. Create Google Sheet

Required columns:

```text
name
email
service
status
created_at
```

---

## 6. Activate Workflow

Run:

```bash
n8n start
```

Activate workflow.

---

# Example Webhook Payload

```json
{
  "name": "Bhagya",
  "email": "example@gmail.com",
  "service": "AI Automation"
}
```

---

# Testing Scenarios

## Scenario 1 — Customer does not respond

Expected:

✅ acknowledgment email  
✅ follow-up reminder  
✅ escalation alert  

---

## Scenario 2 — Customer responds

Update sheet:

```text
status = responded
```

Expected:

Workflow stops.

No:

- reminder
- escalation

---

# Important Limitation

Current workflow requires manually updating:

```text
status = responded
```

Without updating status:

all leads eventually escalate.

Future version:

Automatic Gmail reply detection.

---

# Security Notes

Never commit:

- Gmail credentials
- Google Sheets credentials
- Groq API keys
- Personal email addresses

Use placeholders in public repositories.

`.gitignore` included to protect secrets.

---

# Future Improvements

- Gmail reply detection
- Slack alerts
- Airtable CRM
- HubSpot integration
- Analytics dashboard
- Lead scoring
- WhatsApp notifications
- Retry mechanisms
- Customer segmentation

---

# Repository Structure

```text
.
├── github_ready_lead_followup_workflow.json
├── README.md
├── .gitignore
└── screenshots/
      └── lead-followup-workflow.png
```

---

# Repository Name

```text
ai-lead-followup-automation-n8n
```

---

# Author

**Jagadeesh S**

Built using:

n8n + Groq + Gmail + Google Sheets
