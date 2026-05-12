AI-Powered Lead Follow-up Automation using n8n
Overview
This project is a state-aware lead follow-up automation workflow built using:
* n8n
* Groq LLM
* Gmail
* Google Sheets
The workflow automatically:
1. Captures incoming leads from a webhook
2. Stores lead data in Google Sheets
3. Generates AI-powered acknowledgment emails
4. Sends acknowledgment emails automatically
5. Waits before checking customer response status
6. Sends AI-generated follow-up emails if there is no response
7. Waits again before escalation
8. Rechecks latest customer status dynamically
9. Sends escalation alerts internally if the customer still does not respond
This workflow is designed for:
* Sales teams
* Freelancers
* AI consultants
* Agencies
* Real estate businesses
* Training institutes
* Customer onboarding systems

Problem Statement
Leads are often not followed up quickly enough.
Delays in lead response reduce:
* conversion rates
* meeting bookings
* revenue opportunities
This workflow solves that problem by automating:
* acknowledgment emails
* timed follow-ups
* escalation reminders
using AI and workflow automation.

Tech Stack
Tool	Purpose
n8n	Workflow automation
Groq LLM	AI email generation
Gmail	Sending emails
Google Sheets	CRM / lead tracking
Webhook	Lead capture endpoint
Workflow Architecture
Lead Capture Webhook
 ↓
Prepare Lead Data
 ↓
Store Lead in CRM
 ↓
Generate Acknowledgment Email
 ↓
Send Acknowledgment Email
 ↓
Wait 1 Hour Before Follow-up
 ↓
Check Lead Status After 1 Hour
 ↓
Did Customer Respond After 1 Hour?
 ├── YES → END
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
   Did Customer Respond After 24 Hours?
       ├── YES → END
       └── NO
              ↓
        Send Escalation Alert

Features
Lead Capture
The workflow starts using an n8n webhook.
Incoming lead details include:
* name
* email
* service request

Lead Storage
All incoming leads are stored in Google Sheets.
The Google Sheet acts as a lightweight CRM.
Tracked fields:
Field
name
email
service
status
created_at
AI Acknowledgment Email
The workflow uses Groq LLM to generate a short professional acknowledgment email.
The acknowledgment email:
* thanks the customer
* confirms request receipt
* mentions future contact
* maintains professional tone

Timed Follow-up Logic
After sending the acknowledgment email:
* workflow waits
* rechecks latest lead status from Google Sheets
If:
status != responded
then:
* follow-up email is generated
* reminder email is sent automatically

Dynamic State Rechecking
The workflow does NOT blindly send follow-up emails.
Instead:
* latest lead state is fetched again from Google Sheets
* decision is made using current lead status
This is an important real-world automation architecture pattern.

Escalation System
If the customer still does not respond after the follow-up reminder:
* workflow waits again
* rechecks latest lead status
* sends escalation alert internally
The escalation email contains:
* lead name
* email
* requested service
* submission timestamp

Node-by-Node Explanation
1. Lead Capture Webhook
Purpose
Captures incoming lead data.
Method
POST
Webhook Path
/lead-capture
Example Payload
{
  "name": "Bhagya",
  "email": "example@gmail.com",
  "service": "AI Automation"
}

2. Set Lead Fields
Purpose
Prepares structured lead data.
Fields Generated
Field	Description
name	customer name
email	customer email
service	requested service
status	initial lead status
created_at	timestamp
Default Status
new

3. Store Lead in CRM
Purpose
Stores lead details in Google Sheets.
Google Sheet Columns
name
email
service
status
created_at

4. Generate Acknowledgment Email
Purpose
Uses Groq LLM to generate AI acknowledgment email content.
Model Used
groq/compound-mini
Email Requirements
* professional tone
* short message
* under 80 words
* acknowledgment confirmation

5. Send Acknowledgment Email
Purpose
Sends acknowledgment email to the customer.
Subject
Thanks for contacting us

6. Wait 1 Hour Before Follow-up
Purpose
Waits before checking customer response.
Testing Configuration
1 minute
Production Configuration
1 hour

7. Check Lead Status
Purpose
Re-fetches latest customer status from Google Sheets.
Filter Used
email
This ensures:
* latest state is checked
* follow-up emails are not sent unnecessarily

8. Did Customer Respond?
Purpose
Checks whether customer responded.
Condition
status != responded
TRUE
Customer did NOT respond.
Workflow continues to follow-up email.
FALSE
Customer already responded.
Workflow stops automatically.

9. Generate Follow-up Email
Purpose
Uses Groq LLM to generate AI follow-up reminder email.
Follow-up Includes
* polite reminder
* service reference
* assistance offer
* professional tone

10. Send Follow-up Reminder
Purpose
Sends follow-up reminder email to customer.
Subject
Following up on your request

11. Wait 24 Hours Before Escalation
Purpose
Waits before escalation check.
Testing Configuration
1 minute
Production Configuration
24 hours

12. Check Lead Status After 24 Hours
Purpose
Rechecks latest lead state after follow-up reminder.
Filter Used
email
This ensures:
* escalation only happens for inactive leads

13. Did Customer Respond After 24 Hours?
Purpose
Checks whether customer responded after follow-up reminder.
Condition
status != responded
TRUE
Customer still inactive.
Escalation alert is triggered.
FALSE
Customer responded.
Workflow ends.

14. Send Escalation Alert
Purpose
Sends internal escalation email.
Sent To
Internal/admin email.
Includes
* customer name
* customer email
* requested service
* submission timestamp
Purpose
Allows manual human follow-up for inactive leads.

Google Sheets Setup
Create Google Sheet
Create a sheet named:
Lead Tracker
Sheet Columns
name	email	service	status	created_at
Gmail Setup
Connect Gmail OAuth credentials in n8n.
Used for:
* acknowledgment emails
* follow-up emails
* escalation alerts

Groq Setup
Model Used
groq/compound-mini
Required
* Groq API key
* Groq credentials configured in n8n

Running the Workflow
Step 1
Run n8n locally.
n8n start

Step 2
Activate workflow.

Step 3
Use webhook URL.
Example:
http://localhost:5678/webhook-test/lead-capture

Step 4
Send POST request.
curl -X POST http://localhost:5678/webhook-test/lead-capture \
-H "Content-Type: application/json" \
-d '{
  "name": "Bhagya",
  "email": "example@gmail.com",
  "service": "AI Automation"
}'

Testing Scenarios
Scenario 1 — No Customer Response
Expected
* acknowledgment email sent
* follow-up reminder sent
* escalation alert triggered

Scenario 2 — Customer Responds
Before wait completes
Update Google Sheet:
status = responded
Expected
Workflow stops.
No:
* follow-up email
* escalation email

Important Architecture Concept
This workflow uses:
state-aware automation
Meaning:
* workflow dynamically rechecks latest lead state
* decisions are based on updated CRM data
* prevents unnecessary reminders
This is a real-world workflow engineering pattern.

Future Improvements
Possible upgrades:
* automatic Gmail reply detection
* Slack escalation alerts
* Airtable CRM integration
* HubSpot integration
* analytics dashboard
* lead scoring
* multi-channel notifications
* WhatsApp integration
* retry mechanisms
* customer segmentation

Repository Structure
.
├── workflow.json
├── workflow-screenshot.png
└── README.md

Repository Name
ai-lead-followup-automation-n8n

Repository Description
AI-powered lead follow-up automation built with n8n, Groq LLM, Gmail, and Google Sheets. Automatically captures leads, sends acknowledgment emails, performs timed follow-ups, dynamically rechecks lead status, and escalates inactive leads.

Author
Jagadeesh
