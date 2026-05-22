# 📄 Xperience AI Resume Analyzer (n8n Webhook Edition)

This repository contains an advanced n8n workflow that automates the resume screening process using Local LLMs. It acts as the backend engine for the Xperience AI Resume Analysis web application.

## 🌟 Overview
This workflow receives a candidate's CV and a target Job Description (JD) via a Webhook. It automatically handles file conversions, extracts text, and leverages **Llama3 (via Ollama)** to generate a structured AI analysis report. 

It supports two distinct analysis modes:
1. **Employer Mode:** Acts as a Technical Recruiter to evaluate candidate match scores, strengths, and missing qualifications for hiring managers.
2. **Job Seeker Mode:** Acts as a Career Coach to provide actionable feedback and readiness levels for candidates.

## 🚀 Features
* **Multi-format Support:** Accepts ZIP, DOCX, and PDF files.
* **Auto File Conversion:** Integrates with **ConvertAPI** to seamlessly convert DOCX/ZIP files into PDFs for text extraction.
* **Local AI Processing:** Uses a local instance of Ollama (`llama3:latest`) to ensure data privacy and zero API token costs for LLM inference.
* **Structured JSON Output:** Enforces strict JSON schemas in the LLM prompt to ensure the output is perfectly formatted for frontend consumption or Excel export.

## 🛠️ Prerequisites
* An active [n8n](https://n8n.io/) instance.
* [Ollama](https://ollama.com/) installed and running locally with the `llama3` model (`http://ollama:11434`).
* A [ConvertAPI](https://www.convertapi.com/) account and API Secret.

## ⚙️ Setup Instructions
1. Download the `Xperience WebHook Edition.json` file.
2. In your n8n workspace, click **Import from File** and select the JSON file.
3. Configure the **ConvertAPI** credentials in the HTTP Request nodes.
4. Ensure your Ollama server is accessible from your n8n instance (update the URL in the OpenAI nodes if necessary).
5. Activate the workflow and copy the **Test/Production Webhook URL**.

## 📡 Webhook Payload Structure
Send a `POST` request (multipart/form-data) to the Webhook URL with:
* `cv_post_file`: The candidate's CV file.
* `jd_post_file`: The Job Description file.
* `Mode`: String (`Employer` or `Job Seeker`).