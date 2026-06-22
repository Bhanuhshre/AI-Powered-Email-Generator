# ✉️AI-Powered Email Generator

> AI-powered email generator built with **Claude (Anthropic)** and trained on **Enron Email Dataset** style examples.
> Generate professional emails from prompts, edit them, send via SMTP, and save history.

---

## 📁 Project Structure

```
mailcraft-ai/
│
├── ai_email_generator.ipynb   ← Jupyter Notebook version (recommended)
├── ai_email_generator.py      ← Streamlit web app version
├── emails.csv                 ← Enron email dataset (extracted from zip)
├── email_history.json         ← Auto-created when you save emails
└── README.md                  ← You are here
```

---

## 🚀 Quick Start

### Option 1 — Jupyter Notebook

**1. Install dependencies**
```bash
pip install anthropic ipywidgets jupyter
```

**2. Launch the notebook**
```bash
jupyter notebook ai_email_generator.ipynb
```

**3. Run cells top to bottom:**
- Step 1 → Install packages
- Step 2 → Imports
- Step 3 → Enter your Anthropic API key
- Step 4 → Load core functions
- Step 5 → Interactive email generator UI
- Step 6 → SMTP send configuration
- Step 7 → Email history viewer
- Step 8 → Direct Python usage (no UI)

---

### Option 2 — Streamlit Web App

**1. Install dependencies**
```bash
pip install anthropic streamlit
```

**2. Run the app**
```bash
streamlit run ai_email_generator.py
```

**3. Open browser at:** `http://localhost:8501`

---

## 🔑 Getting Your Anthropic API Key

1. Go to [console.anthropic.com/settings/keys](https://console.anthropic.com/settings/keys)
2. Click **Create Key**
3. Copy the key (starts with `sk-ant-...`)
4. Paste it into the API Key field in the app

---

## ✨ Features

| Feature | Description |
|---|---|
| ✦ **AI Generation** | Describe your email in plain English → Claude writes it |
| 🎭 **Tones** | Professional, Friendly, Formal, Concise, Persuasive, Apologetic |
| 📧 **Email Types** | Business Proposal, Follow-up, Cold Outreach, Meeting Invite, and more |
| ✏️ **Edit** | Full editable subject line and body after generation |
| 📨 **SMTP Send** | Send real emails via Gmail, Outlook, Yahoo, or any SMTP server |
| 💾 **History** | Save emails to `email_history.json`, view and reload anytime |
| 🐍 **Enron Dataset** | Real-world email style examples used as context for Claude |

---

## 📨 SMTP Email Sending Setup

### Gmail (Recommended)

| Field | Value |
|---|---|
| SMTP Host | `smtp.gmail.com` |
| Port | `587` |
| Username | `your@gmail.com` |
| Password | Your **App Password** (NOT your Gmail login password) |

**How to get a Gmail App Password:**
1. Go to **Google Account → Security**
2. Enable **2-Step Verification**
3. Go to [myaccount.google.com/apppasswords](https://myaccount.google.com/apppasswords)
4. Generate a new App Password for "Mail"
5. Use that 16-character password in the app

### Other Providers

| Provider | Host | Port |
|---|---|---|
| Outlook / Hotmail | `smtp.office365.com` | `587` |
| Yahoo Mail | `smtp.mail.yahoo.com` | `587` |
| Custom / Corporate | Your mail server | `587` or `465` |

---

## 🧠 How It Works

```
User Prompt
     │
     ▼
Claude API (claude-sonnet-4-6)
     │   ← System prompt includes:
     │     • Tone & email type settings
     │     • Enron dataset style examples
     │     • JSON output format instruction
     ▼
Generated JSON { subject, body }
     │
     ▼
Editable UI  →  Save to History
                Send via SMTP
```

---

## 🗂️ Email History

Emails are saved to `email_history.json` in the same folder. The file is created automatically when you first save an email. Up to **50 emails** are stored (oldest are removed automatically).

Example entry:
```json
{
  "subject": "Follow-up on Project Proposal",
  "body": "Hi Sarah,\n\nI wanted to follow up ...",
  "prompt": "Write a follow-up about my proposal",
  "tone": "Professional",
  "email_type": "Follow-up",
  "recipient": "sarah@company.com",
  "sender": "me@mycompany.com",
  "saved_at": "Jun 22, 2026 11:45"
}
```

---

## 🐍 Direct Python Usage

You can call `generate_email()` directly in a Python script or notebook cell:

```python
import anthropic, json

ENRON_SAMPLES = [
    {"subject": "Meeting Request", "body": "Greg,\n\nHow about Tuesday?\n\nPhillip"},
]

def generate_email(prompt, email_type, tone, api_key, recipient="", sender=""):
    client = anthropic.Anthropic(api_key=api_key)
    msg = client.messages.create(
        model="claude-sonnet-4-6",
        max_tokens=1000,
        system=f"Write a {tone} {email_type} email. Return JSON: {{\"subject\":\"...\",\"body\":\"...\"}}",
        messages=[{"role": "user", "content": f"Write: {prompt}"}]
    )
    return json.loads(msg.content[0].text)

result = generate_email(
    prompt     = "Follow up with client about proposal deadline",
    email_type = "Follow-up",
    tone       = "Professional",
    api_key    = "sk-ant-...",
    recipient  = "client@company.com"
)

print("Subject:", result["subject"])
print("Body:\n",  result["body"])
```

---

## ⚠️ Troubleshooting

| Problem | Fix |
|---|---|
| `AuthenticationError` | Wrong API key — get it from console.anthropic.com |
| `JSONDecodeError` | Claude returned unexpected format — click Generate again |
| SMTP `SMTPAuthenticationError` | Use App Password, not your regular email password |
| SMTP `Connection refused` | Check host/port — use `587` with STARTTLS |
| `ipywidgets` not showing | Run `jupyter nbextension enable --py widgetsnbextension` |
| Widgets show as text | Install: `pip install ipywidgets` then restart Jupyter kernel |

---

## 📦 Requirements

```
anthropic>=0.20.0
ipywidgets>=8.0.0      # for Jupyter version
streamlit>=1.30.0      # for Streamlit version
jupyter>=1.0.0         # for Jupyter version
```

Install all at once:
```bash
pip install anthropic ipywidgets streamlit jupyter
```

---

## 📄 Dataset

This project uses the **Enron Email Dataset** (`emails.csv`) as style reference examples passed to Claude. The dataset contains real internal emails from Enron Corporation and is widely used for NLP research. It is used here only as writing style context — no personal data is stored or transmitted beyond the API call.

---

## 🛠️ Built With

- [Anthropic Claude](https://anthropic.com) — AI email generation
- [ipywidgets](https://ipywidgets.readthedocs.io) — Jupyter interactive UI
- [Streamlit](https://streamlit.io) — Web app UI
- [smtplib](https://docs.python.org/3/library/smtplib.html) — Python built-in SMTP sending
- Enron Email Dataset — Real-world style examples
