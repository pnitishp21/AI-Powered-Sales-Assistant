[README.md](https://github.com/user-attachments/files/26121277/README.md)
# AI-Powered Sales Assistant
> Automates lead intake, qualification, and product recommendations using Gmail, OpenAI, Google Sheets, and n8n — reducing manual sales effort and improving response time.

## Problem
A furniture company's sales team manually read emails, extracted lead information into Excel, and hand-wrote product recommendations. This led to:

- Slow response times
- Inconsistent quality
- Human error
- Lost revenue opportunities

## Solution

An **AI Sales Assistant** that monitors incoming leads and automatically sends personalised product recommendations — with zero manual intervention.

## Workflow Overview

```
Incoming Email (Gmail)
        │
        ▼
[1] Gmail Trigger — detects new lead email
        │
        ▼
[2] Normalize Email (JS) — strips noise, extracts sender / subject / body
        │
        ├──────────────────────────────────────┐
        ▼                                      ▼
[3a] Structure for Logging (JS)         [3b] OpenAI LLM
     Formats data for Google Sheets          Qualifies lead + generates
                                             product recommendation
        │                                      │
        ▼                                      ▼
[4a] Append to Google Sheets           [4b] Parse LLM Output (JS)
     Logs lead record                        Extracts structured reply
                                                    │
                                                    ▼
                                         [5] Send Reply (Gmail)
                                             Sends personalised email
                                                    │
                                                    ▼
                                         [6] Lead receives recommendation
                                             Fast · consistent · automated
```

## Tech Stack

| Tool | Purpose |
|------|---------|
| **n8n** | Workflow automation platform |
| **Gmail (trigger)** | Monitors incoming lead emails |
| **JavaScript nodes** | Normalise, structure, and parse data |
| **OpenAI API** | LLM for lead qualification and recommendation generation |
| **Google Sheets** | Lead logging and tracking |
| **Gmail (send)** | Automated reply with product recommendations |


## MVP Scope

- Automated lead intake from Gmail
- Email normalisation and data structuring
- AI-generated product recommendations via OpenAI
- Lead logging to Google Sheets
- Automated personalised email reply

## Post-MVP Vision

- Lead scoring and qualification tiers
- Budget and intent-based personalisation
- Leadership-level reporting and insights
- Multi-channel support (WhatsApp, web chat, etc.)
- CRM integration

---

## Interview Talking Points

### 1. What problem does this solve?
The company was losing revenue because sales reps spent hours reading and responding to emails manually. Response time was slow, quality was inconsistent, and there was no way to scale without hiring more staff.

### 2. Why this approach?
Instead of building a full custom application, I used n8n — a low-code automation platform — to wire together existing tools (Gmail, OpenAI, Google Sheets). This let us prove the concept quickly without heavy engineering investment.

### 3. What is the AI actually doing?
The OpenAI LLM receives a structured version of the email (sender, subject, body) and returns a qualified assessment of the lead plus a tailored product recommendation. The prompt is designed with a clear user role and specific output structure so the JS parser can reliably extract and format the reply.

### 4. How would you measure success?
Key metrics:
- **Efficiency** — Time from lead email to reply (target: under 2 minutes)
- **Adoption** — % of incoming leads handled by the assistant vs manually
- **Quality** — Customer satisfaction or reply-to-rate on AI-generated emails
- **Trust & Safety** — Rate of incorrect recommendations flagged by the team

### 5. What are the risks and how do you handle them?
- **Deduplication** — Same lead emailing twice should not get two replies. Robust dedup logic in the JS node prevents this.
- **Edge cases** — Emails that don't match expected structure (e.g. spam, auto-replies) are filtered before reaching the LLM.
- **Prompt quality** — The LLM output is only as good as the prompt. This needs iteration with real lead data.
- **Data privacy** — Lead data stored in Google Sheets must comply with company data handling policy.

### 6. How does this scale?
The MVP handles Gmail. Post-MVP, the same core logic can be triggered from WhatsApp, a web contact form, or any other channel — n8n supports all of these. The LLM and recommendation logic stays the same; only the trigger and send nodes change.


## Project Structure

```
/
├── README.md                  ← You are here
├── workflow/
│   └── ai-sales-assistant.json  ← n8n workflow export (import directly into n8n)
├── prompts/
│   └── lead-qualification.md    ← LLM system prompt used in OpenAI node
└── docs/
    └── workflow-diagram.png     ← Visual workflow diagram
```

## Setup Instructions

1. **Import the workflow** — Open n8n, go to Workflows → Import, and upload `workflow/ai-sales-assistant.json`
2. **Connect Gmail** — Authenticate your Gmail account in n8n credentials
3. **Connect OpenAI** — Add your OpenAI API key in n8n credentials
4. **Connect Google Sheets** — Authenticate and point the Sheets node to your target spreadsheet
5. **Configure the Gmail trigger** — Set the label or filter for incoming lead emails
6. **Test with a sample email** — Send a test lead email and verify the full workflow runs end-to-end

## Author

Built as a Product Manager portfolio project demonstrating end-to-end AI automation thinking: from problem identification, MVP scoping, workflow design, to structured delivery planning.
