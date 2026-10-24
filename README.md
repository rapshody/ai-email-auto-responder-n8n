# 📧 AI Email Auto-Responder with Memory

A self-hosted, context-aware AI email agent built with **n8n**, **PostgreSQL**, and **Google Gemini**. 

Unlike basic auto-responders, this agent remembers past conversations, classifies incoming emails, prevents duplicate replies, and drafts personalized responses based on historical context.

![Workflow Overview](screenshots/workflow-overview.png)

## 🎯 The Problem

Customer support and sales teams waste hours triaging emails. Basic AI chatbots often lose context—if a customer replies three days later, the bot forgets who they are and what was discussed. Furthermore, most SaaS automation tools charge per step, making high-volume email handling expensive.

## 💡 The Solution

This workflow acts as an autonomous Tier-1 support agent. 

1. **Trigger:** Watches a Gmail inbox for new messages.
2. **Classification:** Uses Google Gemini to classify the email (Sales, Support, Spam) and extract urgency.
3. **Memory Check:** Queries a PostgreSQL database to see if this is a new conversation or an ongoing thread.
4. **Context Retrieval:** If it's an ongoing thread, it pulls the past 10 messages from the database.
5. **Drafting:** Gemini reads the email *and* the historical context to draft a highly accurate, personalized reply.
6. **Dispatch & Logging:** Sends the reply via Gmail and saves the AI's response back to the database for future memory.

## 🏗 Architecture & Memory Logic

![Memory Logic](screenshots/memory-logic.png)

The workflow uses a "Check, Then Insert" pattern:
*   **True Branch (New Email):** Creates a new conversation record in the `conversations` table and saves the user's message.
*   **False Branch (Existing Email):** Saves the user's message to the existing thread and pulls the last 10 messages to provide context to the AI.

## 🛠 Tech Stack

- **n8n (Self-Hosted):** Orchestration and logic.
- **PostgreSQL (Docker):** Relational database for conversation memory and state management.
- **Google Gemini (1.5 Flash):** Natural language classification and reply generation.
- **Gmail API:** Inbox monitoring and message dispatch.

## 🔑 Key Features

- **Idempotency:** Uses SQL `ON CONFLICT DO NOTHING` to ensure that if an email is processed twice, it never creates duplicate records or sends duplicate replies.
- **True AI Memory:** Gives the LLM persistent context across multiple days and email threads.
- **Self-Hosted & Private:** Runs entirely on Docker. No email data is sent to third-party automation platforms like Zapier or Make.
- **Cost-Effective:** Uses Gemini Flash, which is highly capable but incredibly cheap to run at scale.

## 📸 Output Example

![Email Output](screenshots/email-output.png)

## 🚀 How to Replicate

1. Ensure you have **Docker** installed.
2. Spin up the PostgreSQL database (see `docs/setup-guide.md` for the SQL schema).
3. Import the `workflow/ai-email-agent.json` file into your n8n instance.
4. Reconnect your Gmail OAuth credential and your Google Gemini API key.
5. Update the Postgres credential to point to your local Docker container (`postgres-n8n`).
6. Activate the workflow.

## 📈 Business Value

- Replaces hours of manual email triage.
- Reduces response times from hours to seconds.
- Prevents customer frustration by maintaining context across threads.
- Runs on open-source infrastructure, eliminating per-step SaaS fees.

## 👤 Author
**Asuquo Patrick** — AI Automation Specialist

[LinkedIn](YOUR_LINKEDIN_URL) · [GitHub](https://github.com/rapshody)
