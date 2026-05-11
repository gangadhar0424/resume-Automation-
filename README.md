# resume-Automation-
# 🤖 AI-Powered ATS Resume Automation Agent

![n8n](https://img.shields.io/badge/n8n-Workflow%20Automation-orange)
![Gemini](https://img.shields.io/badge/Google%20Gemini-AI-blue)
![Supabase](https://img.shields.io/badge/Supabase-Database-green)
![LaTeX](https://img.shields.io/badge/LaTeX-PDF%20Generation-lightgrey)
![Apify](https://img.shields.io/badge/Apify-Web%20Scraping-red)

## 📌 Overview

An end-to-end automated pipeline that scrapes LinkedIn for relevant job postings every morning, rewrites your resume using Google Gemini AI to match each job description, generates professional ATS-optimized PDF resumes, and delivers them to your inbox — all without any manual effort.

Built as a personal productivity tool to solve the problem of spending hours tailoring resumes for every job application.

---

## 🎯 Problem Statement

Job seekers spend an average of 2-3 hours per day manually tailoring resumes for different job postings. This workflow automates the entire process:

- ❌ **Before:** Manually search jobs → Read JD → Rewrite resume → Format PDF → Apply
- ✅ **After:** Wake up → Open email → Review tailored PDFs → Apply in one click

---

## ⚙️ Tech Stack

| Tool | Purpose |
|------|---------|
| **n8n** | Workflow orchestration and automation |
| **Google Gemini AI** | ATS resume optimization and tailoring |
| **Apify** | LinkedIn job scraping |
| **Google Drive API** | Resume storage and PDF upload |
| **Google Docs API** | Reading master resume |
| **Supabase** | Job tracking and duplicate filtering |
| **LaTeX (YtoTech)** | Professional PDF generation |
| **Gmail API** | Daily email digest delivery |

---

## 🏗️ Architecture
Daily 7AM Trigger
↓
Read Resume (Google Docs)
↓
Scrape LinkedIn Jobs (Apify)
↓
Filter Duplicates (Supabase)
↓
For Each New Job:
├── Gemini AI rewrites resume for job
├── Build LaTeX document
├── Compile to PDF
├── Upload to Google Drive
└── Store job record in Supabase
↓
Send Email Digest (Gmail)
with job links + PDF resume links

---

## ✨ Features

- 🕖 **Daily automation** — runs every morning at 7 AM automatically
- 🔍 **Smart job scraping** — fetches latest LinkedIn jobs posted in last 24 hours
- 🧠 **AI resume tailoring** — Gemini AI optimizes resume keywords for each specific job
- 📄 **Professional PDF generation** — LaTeX-compiled clean resume PDFs
- 🔄 **Duplicate filtering** — Supabase tracks processed jobs to avoid repeats
- 📧 **Email digest** — single email with all jobs and tailored resume links
- ☁️ **Cloud storage** — all PDFs saved and shared via Google Drive

---

## 🔄 How It Works

### Step 1 — Trigger
Workflow fires automatically at 7:00 AM every day via n8n Schedule Trigger.

### Step 2 — Read Resume
Fetches your master resume from Google Docs using the Google Drive & Docs API and extracts full plain text.

### Step 3 — Scrape Jobs
Sends a request to Apify's LinkedIn Jobs Scraper with your target job query and location (India). Polls every 10 seconds until scraping completes.

### Step 4 — Filter & Process
- Parses raw job data and filters jobs posted in last 24 hours
- Checks Supabase database to remove already-processed jobs
- Limits to top 5 new jobs per run

### Step 5 — AI Resume Optimization
For each job, sends both the master resume and job description to Google Gemini AI with a detailed ATS optimization prompt. Gemini rewrites the resume with:
- Relevant keywords from the job description
- Tailored professional summary
- Reordered experience bullets matching the role
- Role-specific skills emphasis

### Step 6 — PDF Generation
Converts Gemini's plain text output into a structured LaTeX document and compiles it to a professional PDF via YtoTech LaTeX API.

### Step 7 — Storage & Delivery
- Uploads each PDF to Google Drive
- Makes PDFs publicly accessible via shareable link
- Stores job record in Supabase to prevent future duplicates
- Sends a single HTML email digest with a table of all jobs and resume links

---

## 📧 Sample Email Output

| Company | Role | Posted | Job | Resume |
|---------|------|--------|-----|--------|
| TechCorp India | ML Engineer | 2026-05-11 | 🔗 View Job | 📄 PDF Resume |
| AI Startup | Python Developer | 2026-05-11 | 🔗 View Job | 📄 PDF Resume |

---

## 🚀 Setup Guide

### Prerequisites
- [n8n Cloud account](https://n8n.io) or self-hosted n8n
- [Apify account](https://apify.com) (free tier)
- [Supabase account](https://supabase.com) (free tier)
- [Google Cloud project](https://console.cloud.google.com) with Docs API enabled
- [Google Gemini API key](https://aistudio.google.com)
- Gmail account

### Installation

1. **Clone this repository**
```bash
git clone https://github.com/gangadhar0424/ai-resume-automation.git
```

2. **Import workflow to n8n**
   - Open n8n → Click "Add Workflow"
   - Click import → Upload `workflow.json`

3. **Create Supabase table**
```sql
create table jobs (
  id uuid primary key default gen_random_uuid(),
  job_url text unique,
  job_title text,
  company text,
  processed_at timestamptz
);
```

4. **Configure credentials in n8n**
   - Google Drive OAuth2
   - Google OAuth2 (for Docs API)
   - Google Gemini API
   - Gmail OAuth2
   - Supabase API

5. **Update Workflow Configuration node**
   - `resumeFileId` — your Google Doc resume ID
   - `jobSearchQuery` — e.g. `Machine Learning Engineer`
   - `userEmail` — your email address
   - `apifyToken` — your Apify API token
   - `supabaseUrl` — your Supabase project URL
   - `supabaseKey` — your Supabase secret key

6. **Activate the workflow**
   - Toggle the workflow to **Active**
   - It will now run automatically every day at 7 AM

---

## 📁 Repository Structure
ai-resume-automation/
│
├── workflow.json          # n8n workflow export
├── README.md              # Project documentation
├── screenshots/
│   ├── workflow.png       # n8n workflow canvas
│   ├── email-output.png   # Sample email digest
│   └── resume-output.png  # Sample generated PDF

---

## 🔧 Configuration Options

| Parameter | Description | Default |
|-----------|-------------|---------|
| `jobSearchQuery` | LinkedIn search keyword | `Machine Learning Engineer` |
| `location` | Job location filter | `India` |
| `f_TPR=r86400` | Time filter (last 24 hours) | 24 hours |
| `maxItems` | Max jobs per run | 5 |
| `triggerAtHour` | Daily trigger time | 7 AM |

---

## 🌱 Future Improvements

- [ ] Auto-apply to jobs using LinkedIn Easy Apply
- [ ] Cover letter generation per job
- [ ] Job matching score before processing
- [ ] WhatsApp/Telegram notification instead of email
- [ ] Support for multiple job portals (Naukri, Indeed)
- [ ] Dashboard to track application status

---

## 👨‍💻 Author

**Gangadhara Reddy Nallakaluva**
- 📧 gangadharreddy0424@gmail.com
- 💼 [LinkedIn](https://linkedin.com/in/gangadhar-reddy0424)
- 🐙 [GitHub](https://github.com/gangadhar0424)

---

## 📄 License

This project is open source and available under the [MIT License](LICENSE).

---

## ⭐ If this project helped you, please give it a star!
