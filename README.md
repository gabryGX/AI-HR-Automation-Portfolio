# GBRY_N8 — AI HR & Career Automation Suite

**GBRY_N8** is an end-to-end HR and career automation system built with **n8n**, **Google Gemini**, **Groq**, and **Google Workspace**. It automates job intake, AI candidate screening with OCR fallback, HR policy Q&A via RAG, multi-agent career coaching, and production error monitoring.

🔗 **Live Interactive Portfolio:** [https://gabryGX.github.io/AI-HR-Automation-Portfolio/](https://gabryGX.github.io/AI-HR-Automation-Portfolio/)

---

## Workflows Included

| Workflow File | Trigger | Description |
| :--- | :--- | :--- |
| `recruitment-1-intake.json` | Form Trigger | Validates applicant fields & PDF file headers, prevents duplicates, uploads CVs to Google Drive, and logs records to Google Sheets. |
| `recruitment-2-ai-scan.json` | Schedule (Hourly) / Manual | Reads new CVs (with Gemini OCR fallback for scanned PDFs), scores fit via Groq LLMs, emails invites/rejections, briefs HR, and updates Sheets. |
| `recruitment-3-error-handler.json` | Error Trigger & Schedules | Classifies errors, throttles duplicate critical alerts, logs incidents, sends daily digests at 8 AM, and runs a 2-hour stuck-queue heartbeat check. |
| `hr-policy-rag-assistant.json` | Chat Trigger | RAG-powered HR assistant that answers employee policy questions strictly from company documents with source citations. |
| `policy-loader.json` | Instance Start / Daily 7 AM | Automatically downloads company policy Google Docs from Drive and refreshes the in-memory vector store. |
| `job-application-coach.json` | Form Trigger | Multi-agent orchestrator (CV Matcher, Cover Letter Writer, Interview Coach) that emails a tailored career coaching report. |

---

## Services & Stack

* **Automation Engine:** n8n (Self-hosted via Docker + ngrok static domain)
* **LLMs & Embeddings:** Groq (`openai/gpt-oss-120b`, `openai/gpt-oss-20b`) & Google Gemini (`gemini-2.5-flash-lite`, `gemini-3.5-flash-lite`, Gemini Embeddings)
* **Storage & Comms:** Google Drive, Google Sheets, Gmail
* **Frontend:** Static single-page HTML/CSS/JS hosted on GitHub Pages

---

## How to Import & Run

1. **Import Workflows:** In n8n, create a project folder and import each `.json` file from this repository via **Workflow → … → Import from File**.
2. **Connect Credentials:** Configure your own **Google Sheets OAuth2**, **Google Drive OAuth2**, **Gmail OAuth2**, **Google Gemini (PaLM) API**, and **Groq API** credentials in n8n and assign them to the nodes.
3. **Configure Storage IDs:** Replace placeholder Sheet IDs (`Applications` and `Error Log`) and Google Drive Folder IDs (`CVs` and `HR Policies`) with your own.
4. **Set Error Workflow & Activate:** Link `recruitment-3-error-handler` as the Error Workflow in the settings of the other workflows, then activate all workflows.
