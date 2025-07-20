# Influencer Monitoring AI Agent

An AI-powered system that tracks a curated list of influencers on YouTube, Instagram, and LinkedIn, summarizes their latest content, and generates a bi‑daily Trend Brief delivered via email. This MVP can be up and running in 48 hours using free, efficient tools and minimal code.

---

## 📂 Repository Structure

```
influencer-monitor/  
├── README.md          # Project overview & setup guide  
├── LICENSE           # License file (MIT)  
├── .gitignore        # Ignore patterns for Python & sensitive files  
├── .env.example      # Template for environment variables  
│  
├── data/             # Storage for fetched data and reports  
│   ├── raw/          # JSON files of raw influencer posts/videos  
│   └── reports/      # Generated Trend Brief markdown files  
│  
├── scripts/          # Python scripts for fetch, summarize, and report  
│   ├── fetch_youtube.py       
│   ├── fetch_instagram.py     
│   ├── fetch_linkedin.py      
│   ├── summarize.py           
│   └── generate_report.py     
│  
└── n8n_workflow.json # Export of the automation flow configuration  
```

---

## 📝 Quick Overview

* **Goal:** Automate the fetching, summarization, and reporting of influencer content every 48 hours.
* **Tech Stack:** Python, n8n (workflow automation), Google APIs, Instaloader, LinkedIn Scraper, OpenAI, Hugging Face Transformers, SendGrid (email delivery).
* **Primary Features:**

  1. Fetch latest posts/videos from influencers.
  2. Summarize content using AI (GPT-3.5 Turbo or local BART summarizer).
  3. Generate a Trend Brief (Markdown) with top themes, notable posts, and insights.
  4. Automate the pipeline via n8n and email the report every 48 hours.

---

## ⚙️ Setup Instructions

### 1. Clone the Repository

```bash
git clone https://github.com/<your-username>/influencer-monitor.git
cd influencer-monitor
```

### 2. Create and Activate Python Environment

```bash
python3 -m venv venv
# macOS/Linux
source venv/bin/activate
# Windows (PowerShell)
venv\Scripts\Activate.ps1
```

### 3. Install Dependencies

```bash
pip install --upgrade pip
pip install -r requirements.txt
```

> If `requirements.txt` is not present, generate it:
>
> ```bash
> pip freeze > requirements.txt
> ```

### 4. Configure Environment Variables

1. Copy the example file:

   ```bash
   cp .env.example .env
   ```
2. Edit `.env` and fill in your API keys and credentials:

   ```dotenv
   YOUTUBE_API_KEY=your_youtube_api_key
   INSTAGRAM_USERNAME=your_instagram_username
   INSTAGRAM_PASSWORD=your_instagram_password
   LINKEDIN_EMAIL=your_linkedin_email
   LINKEDIN_PASSWORD=your_linkedin_password
   OPENAI_API_KEY=your_openai_api_key
   AIRTABLE_API_KEY=your_airtable_api_key
   SENDGRID_API_KEY=your_sendgrid_api_key
   ```

### 5. Define Influencer List

Edit `influencers.csv` (or connect a Google Sheet) with your target influencers:

```Example:
name,platform,handle_or_url
FitJane,instagram,fitjane
GymGuy,youtube,UCgymguy123
WellnessWanda,linkedin,https://www.linkedin.com/in/wellnesswanda
```

---

## 🗓️ Running the Pipeline

### Manual Execution

1. **Fetch Data:**

   ```
   python scripts/fetch_youtube.py
   python scripts/fetch_instagram.py
   python scripts/fetch_linkedin.py
   ```
2. **Generate Report:**

   ```
   python scripts/generate_report.py
   ```
3. **Check Output:**

   * Raw JSON files in `data/raw/`
   * Trend Brief in `data/reports/trend_brief.md`

### Automated Execution via n8n

1. **Start n8n:**

   ```bash
   docker run -it --rm -p 5678:5678 -v ~/.n8n:/home/node/.n8n n8nio/n8n
   ```
2. **Import Workflow:**

   * In the n8n UI, import `n8n_workflow.json`.
   * Configure the Cron trigger to run every 48 hours.
   * Verify paths to Python scripts and the SendGrid node settings.
3. **Activate Workflow:** Once configured, toggle the workflow to Active.

---

## 🛠️ Scripts Explained

* **fetch\_youtube.py:** Uses YouTube Data API to pull recent videos (title, description, timestamp).
* **fetch\_instagram.py:** Uses Instaloader to export Instagram post metadata as JSON.
* **fetch\_linkedin.py:** Uses linkedin-scraper to pull recent posts from a LinkedIn profile.
* **summarize.py:** Defines `summarize_text()` that calls OpenAI API or falls back to Hugging Face.
* **generate\_report.py:** Aggregates raw data, summarizes content, extracts keywords, and writes a Markdown report.

---

## 🌱 Extending & Customizing

* **Dashboard:** Add a Streamlit app (`app.py`) to visualize trends and raw data.
* **Storage:** Swap Airtable or Google Sheets for a database like PostgreSQL or MongoDB.
* **Delivery:** Integrate Slack, Microsoft Teams, or SMS by adding new n8n nodes.

---

## 📋 Contributing & Branching

* **Main branch:** stable code only.
* **Feature branches:** `feature/xxx` (e.g., `feature/fetch-instagram`).
* **Commit style:**

  ```
  feat(fetch): add YouTube scraping
  fix(report): handle empty summaries
  docs: update README with new env vars
  ```

---

## ⚖️ License

This project is licensed under the [MIT License](LICENSE).

---

## 🎖️ Badges (Optional)

Add shields at the top of your README for:

```markdown
![Python](https://img.shields.io/badge/Python-3.9%2B-blue)
![License: MIT](https://img.shields.io/badge/License-MIT-green)
![Build Status](https://img.shields.io/github/workflow/status/your-username/influencer-monitor/CI)
```

Replace URLs and workflow names as appropriate.

---

*Happy hacking!*
