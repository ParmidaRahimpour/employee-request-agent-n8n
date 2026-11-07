# Employee Request Management Agent

An n8n workflow that receives employee requests, analyzes them with an AI Agent
(using RAG over company policies), and routes them to the responsible department.

![Workflow](images/workflow-canvas.png)

## Features
- Request intake via web form with unique ticket ID
- AI classification: category, priority, urgency (SLA), department
- RAG over company policy documents (vector store)
- Structured JSON output for reliable automation
- Automatic routing; ambiguous/sensitive requests flagged for human review
- Logging to Google Sheets, email notifications, manager escalation for High/Critical

## Tech Stack
n8n · OpenAI (GPT-4o-mini + Embeddings) · Vector Store · Google Sheets · Gmail

## Setup
1. Import `workflow/employee-request-agent.json` into n8n (Workflows → Import from File).
2. Create credentials: OpenAI, Google Sheets (OAuth2), Gmail (OAuth2).
3. Create a Google Sheet with headers:
   `ticketId, submittedAt, employeeName, employeeEmail, title, category, priority, urgencyHours, department, status, summary, confidence`
4. Replace the placeholder emails in the **Build Ticket Record** and **Escalate to Manager** nodes.
5. Run the document-loading branch once (Manual Trigger) to fill the vector store.
6. Activate the workflow and open the form's Production URL.

## Test scenarios
| Input | Expected |
|---|---|
| VPN down for the whole sales team | IT, Critical, manager escalation |
| 5-day leave request | HR, normal priority |
| Request for a new laptop | Procurement |

## Notes
- The in-memory vector store is cleared on restart; use Qdrant/Supabase in production.
- Sample policy documents are included in the Code node for demonstration.
