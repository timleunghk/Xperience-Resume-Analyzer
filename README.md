# 📄 Xperience AI Resume Analyzer (Enterprise & HIPAA-Ready Edition)

This repository contains an advanced, enterprise-grade **n8n automation workflow** designed to revolutionize the resume screening process. Acting as the backend engine for the Xperience AI Resume Analysis platform, it evaluates candidate Curriculum Vitae (CVs) against target Job Descriptions (JDs) using Large Language Models (LLMs).

**🌟 Upwork Portfolio Highlight:** 
Unlike standard AI wrappers, this architecture is built with a **Privacy-First** approach. It implements **HIPAA-ready data minimization** and **Cryptographic anonymization** to enable unbiased "Blind Hiring" while securely leveraging Enterprise LLMs.

## 🚀 Key Features

* **🔒 Privacy-First & HIPAA-Ready:** Automatically redacts Personally Identifiable Information (PII) such as emails and phone numbers using Regex *before* sending any data to external LLMs.
* **🛡️ Blind Hiring Enablement:** Utilizes cryptographic hashing to anonymize candidate names, providing Hiring Managers with objective, bias-free evaluation reports.
* **🏢 Enterprise-Grade AI Integration:** Powered by **Azure OpenAI** (GPT models) with strict System Prompts and LangChain Structured Output Parsers to guarantee 100% reliable JSON responses.
* **📂 Advanced File Handling:** Natively processes `multipart/form-data` webhooks, automatically extracts CVs from `.zip` archives, and parses `.pdf` files without relying on expensive third-party APIs.
* **🧩 Modular Sub-Workflow Architecture:** Separates Job Description (JD) parsing into a dedicated sub-workflow (`Support JD SubWorkflow`) for better maintainability and scalability.
* **📊 Automated Reporting:** Aggregates AI evaluation results (Match Score, Key Strengths, Missing Qualifications) and automatically generates an Excel spreadsheet for HR teams.

## ⚙️ How It Works (Architecture Flow)

1. **Trigger:** A Webhook receives the candidate's CV (PDF/ZIP) and the target JD file.
2. **Data Preparation:** 
   - Unzips archives and extracts text from PDFs.
   - **[Security Step]** Executes a custom JavaScript node to mask PII (Email/Phone).
3. **AI Extraction:** Azure OpenAI extracts structured data from the CV based on a strict JSON schema.
4. **JD Processing:** Calls a Sub-Workflow to parse and standardize the Job Description requirements.
5. **Semantic Evaluation:** 
   - **[Security Step]** Cryptographically hashes the candidate's name.
   - A secondary AI Agent performs a semantic comparison between the CV and JD, calculating a definitive Match Score (0-100) and generating actionable feedback.
6. **Output:** Returns a structured JSON response to the frontend and generates a downloadable Excel report.

## 🛠️ Prerequisites

* An active [n8n](https://n8n.io/) instance (Self-hosted or Cloud).
* An **Azure OpenAI** account with a deployed Chat Model (e.g., GPT-4o or GPT-4-turbo).
* *Note: To maintain strict HIPAA compliance in a production environment, ensure your organization has a signed Business Associate Agreement (BAA) with Microsoft.*

## 🚀 Setup Instructions

1. Download both JSON files:
   - `Xperience WebHook Lead Management -- JD SubWorkflow.json`
   - `Xperience WebHook Lead Management (Support JD SubWorkflow).json`
2. In your n8n workspace, import the **JD SubWorkflow** first and activate it.
3. Import the **Main Workflow**.
4. Inside the Main Workflow, update the `Execute Workflow` node to point to your newly imported JD SubWorkflow.
5. Configure your **Azure OpenAI credentials** in all LLM nodes.
6. Activate the workflow and copy your **Webhook URL**.

## 📡 Webhook Payload Structure

Send a `POST` request (`multipart/form-data`) to the Webhook URL with the following fields:

* `cv_post_file`: The candidate's CV file (PDF or ZIP containing PDFs).
* `jd_post_file`: The Job Description file (PDF).

### Example JSON Response:
```json
{
  "Candidate ID": "candidate_8f4e2a1b...",
  "Match Percentage": 85,
  "Recommendation": "Strong Hire",
  "Key Strengths": ["React.js", "Node.js", "Cloud Architecture"],
  "Missing Qualifications": ["GraphQL"],
  "Executive Summary": "The candidate strongly matches the core requirements with extensive experience in full-stack development. Highly recommended for an interview."
}