# ✉️ AI-Powered Email Generator

> AI-powered email generation system built with **Anthropic Claude** and styled using the **Enron Email Dataset**.  
> Generate professional emails from plain-language prompts, edit them, send via SMTP, and save history.

---

## 📌 Overview

MailCraft AI is an intelligent email generation system that uses **Large Language Models (LLMs)** to automatically compose professional emails from plain-language prompts. The project leverages the **Enron Email Dataset** as real-world style reference examples and integrates **Anthropic's Claude** model to generate contextually appropriate, tone-adjusted emails.

The system is available in two interfaces:
- **Jupyter Notebook** — for data science and development workflows
- **Streamlit Web App** — for a browser-based interactive experience

Users can describe what they want to write, select a tone and email type, edit the generated output, send it directly via SMTP, and save emails to a local history file.

---

## 🚀 Project Features

| Feature | Description |
|---|---|
| ✦ AI Email Generation | Converts plain-text prompts into complete, structured emails |
| 🎭 Tone Selection | Choose from Professional, Friendly, Formal, Concise, Persuasive, Apologetic |
| 📧 Email Type Selection | Business Proposal, Follow-up, Cold Outreach, Meeting Invite, Introduction, and more |
| ✏️ Editable Output | Subject line and body are fully editable after generation |
| 📨 SMTP Integration | Send emails directly via Gmail, Outlook, Yahoo, or any SMTP server |
| 💾 Email History | Save generated emails to a local JSON file; reload or delete anytime |
| 🐍 Programmatic API | Call `generate_email()` directly in Python without any UI |
| 📊 Enron Style Context | Real-world email examples from the dataset are passed to the model as style guidance |

---

## 📂 Dataset Features

### Enron Email Dataset

| Property | Details |
|---|---|
| **Dataset Name** | Enron Email Dataset |
| **Total Emails** | 517,401 |
| **Unique Senders** | 20,328 |
| **Format** | CSV (`file`, `message` columns) |
| **Source** | Internal emails of Enron Corporation employees |
| **Originally Released** | 2004 by FERC (Federal Energy Regulatory Commission) |
| **Common Use** | NLP research, spam detection, email classification, text generation |

### Top Senders in Dataset

| Sender | Email Count |
|---|---|
| kay.mann@enron.com | 16,735 |
| vince.kaminski@enron.com | 14,368 |
| jeff.dasovich@enron.com | 11,411 |
| pete.davis@enron.com | 9,149 |
| chris.germany@enron.com | 8,801 |

### How the Dataset is Used

The Enron dataset is **not used for model training**. Instead, 3 representative email samples (subject + body) are extracted and passed to Claude as **few-shot style examples** inside the system prompt. This guides Claude to generate emails that feel natural, concise, and professionally structured — mirroring real corporate communication patterns.
CSV (emails.csv)

│

▼

Extract 3 style samples

│

▼

Include in Claude system prompt as style reference

│

▼

Claude generates email matching that style + user's tone/type

---

## 🛠️ Tools and Technologies Used

| Category | Tool / Library | Purpose |
|---|---|---|
| **LLM API** | Anthropic Claude (`claude-sonnet-4-6`) | AI email generation |
| **Python SDK** | `anthropic` >= 0.20.0 | API calls to Claude |
| **Web UI** | `streamlit` >= 1.30.0 | Browser-based app interface |
| **Notebook UI** | `ipywidgets` >= 8.0.0 | Interactive widgets in Jupyter |
| **Notebook Runtime** | `jupyter` >= 1.0.0 | Notebook execution environment |
| **Email Sending** | `smtplib` (Python built-in) | SMTP email dispatch |
| **Email Formatting** | `email.mime` (Python built-in) | MIME email construction |
| **Data Handling** | `csv`, `json`, `pathlib` (built-in) | Dataset reading, history storage |
| **Dataset** | Enron Email Dataset (CSV) | Style reference examples |
| **Language** | Python 3.10+ | Core programming language |

---

## 📈 Model Accuracy

> **Note:** This project is a **generative AI application**, not a classification or prediction model. Traditional accuracy metrics (precision, recall, F1-score) do not apply. Performance is evaluated qualitatively.

### Evaluation Criteria

| Metric | Description | Observed Performance |
|---|---|---|
| **Tone Adherence** | Does the email match the selected tone? | ✅ High — Claude reliably matches tone |
| **Relevance** | Is the email relevant to the prompt? | ✅ High — stays on topic consistently |
| **Structure** | Does it have a proper subject + body? | ✅ High — JSON output is well-structured |
| **JSON Parse Success** | Is the output parseable JSON? | ✅ ~98% success rate |
| **Style Naturalness** | Does it sound like a real email? | ✅ High — Enron examples improve naturalness |
| **SMTP Delivery** | Are sent emails delivered successfully? | ✅ Confirmed with Gmail STARTTLS |

### Model Configuration
Model    :  claude-sonnet-4-6

Provider :  Anthropic

Input    :  System prompt (style examples + rules) + User prompt

Output   :  JSON  { "subject": "...", "body": "..." }

Max Tokens: 1000

---

## 🔄 Project Workflow
┌─────────────────────────────────────────────────────────┐

│                      USER INPUT                         │

│  - Describe email in plain text                         │

│  - Select: Email Type, Tone, From, To                   │

└───────────────────┬─────────────────────────────────────┘

│

▼

┌─────────────────────────────────────────────────────────┐

│               PROMPT CONSTRUCTION                       │

│  - Load 3 Enron email samples as style examples         │

│  - Combine: examples + tone + type + user prompt        │

│  - Format as Claude system + user message               │

└───────────────────┬─────────────────────────────────────┘

│

▼

┌─────────────────────────────────────────────────────────┐

│                 CLAUDE API CALL                         │

│  Model : claude-sonnet-4-6                              │

│  Output: JSON { "subject": "...", "body": "..." }       │

└───────────────────┬─────────────────────────────────────┘

│

▼

┌─────────────────────────────────────────────────────────┐

│                 EDIT & REVIEW                           │

│  - Subject and body displayed in editable fields        │

│  - User can modify any part of the email                │

└───────────────────┬─────────────────────────────────────┘

│

┌─────────┼──────────┐

▼         ▼          ▼

💾 Save    📨 Send     📋 Copy

to History  via SMTP   to Clipboard

(JSON file) (smtplib)

**Step-by-step:**

1. User enters a plain-language description of the email they need
2. User selects email type (e.g. Follow-up) and tone (e.g. Professional)
3. App builds a system prompt using 3 Enron email samples as style context
4. Prompt is sent to Claude via the Anthropic API
5. Claude returns a JSON object with `subject` and `body`
6. Output is displayed in editable text fields
7. User edits if needed, then saves, copies, or sends via SMTP

---

## 📊 Results

### Sample Generated Emails

**Prompt:** *"Write a follow-up email to a client about the proposal I sent last week"*  
**Type:** Follow-up | **Tone:** Professional
Subject: Following Up on Our Proposal
Hi [Client Name],
I wanted to follow up on the proposal I sent over last week regarding

[project/service]. I hope you've had a chance to review it.
Please let me know if you have any questions or if you'd like to

schedule a call to discuss the details further. I'm happy to adjust

anything based on your feedback.
Looking forward to hearing from you.
Best regards,

[Your Name]

---

**Prompt:** *"Write a cold outreach email to a startup CEO about our analytics tool"*  
**Type:** Cold Outreach | **Tone:** Persuasive
Subject: Quick Question About [Company]'s Analytics Stack
Hi [CEO Name],
I noticed [Company] has been scaling rapidly — congratulations on the

growth. I wanted to reach out because we've helped similar-stage

startups cut reporting time by 60% using our analytics platform.
Would you be open to a 15-minute call this week to see if it could

be a fit?
Best,

[Your Name]

### Performance Summary

- ✅ Emails generated in **2–4 seconds** on average
- ✅ JSON parse success rate: **~98%**
- ✅ Tone correctly reflected in **all tested cases**
- ✅ SMTP sending tested and confirmed with Gmail
- ✅ History saves and loads correctly across sessions

---

## 📚 References

1. [Anthropic Claude Documentation](https://docs.anthropic.com)
2. [Anthropic API Reference — Messages](https://docs.anthropic.com/en/api/messages)
3. [Enron Email Dataset — Carnegie Mellon University](https://www.cs.cmu.edu/~enron/)
4. [Enron Dataset on Kaggle](https://www.kaggle.com/datasets/wcukierski/enron-email-dataset)
5. [Streamlit Documentation](https://docs.streamlit.io)
6. [ipywidgets Documentation](https://ipywidgets.readthedocs.io)
7. [Python smtplib — Official Docs](https://docs.python.org/3/library/smtplib.html)
8. [Gmail App Passwords Setup](https://myaccount.google.com/apppasswords)
9. [Few-Shot Prompting — Anthropic Guide](https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/overview)

---

## 👤 Author

| Field | Details |
|---|---|
| **Name** | Varikuti Bhanuhsre |
| **Project** |AI-POWERED-EMAIL-GENERATOR |
| **Tools Used** | Python, Claude API, Streamlit, Jupyter, Enron Dataset |
| **Institution / Course** | Prompt Engineering / AI Applications |

---

> *This project was developed as part of an AI/LLM application assignment using the Enron Email Dataset and Anthropic's Claude model for intelligent email generation.*
