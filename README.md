# 🚀 AI-Powered Finance Workflow Automation System

## 📌 Overview

This project is an **end-to-end, event-driven approval workflow system** designed to automate the processing of financial requests received via email.

It uses **AI to extract structured data**, validates it against internal datasets, and enables **one-click approval/rejection** with full audit tracking.

---

## 🎯 Key Features

* 📩 Automated email ingestion
* 🤖 AI-based data extraction (unstructured → structured)
* 🔍 Data validation using internal mappings
* 📊 Centralized state tracking
* 📧 Email-based approval workflow
* 🔁 Idempotent and fault-tolerant processing
* 🧾 Full audit trail (event logging)

---

## 🏗️ Architecture

```plaintext
Email → Blob Storage → Azure Function (Processing)
      → AI Extraction → Data Validation
      → State Storage (CSV) + Audit Log (JSON)
      → Approval Email → User Action
      → HTTP API → Status Update
```

---

## ⚙️ Tech Stack

| Layer           | Technology                     |
| --------------- | ------------------------------ |
| Compute         | Azure Functions (Python)       |
| Storage         | Azure Blob Storage             |
| AI              | Azure OpenAI (gpt-4o-mini)     |
| Data Processing | Pandas                         |
| Notification    | SMTP (Outlook)                 |
| API             | HTTP Trigger (Azure Functions) |

---

## 🧠 How It Works

### 1. 📩 Email Ingestion

* Incoming financial emails are stored in Blob Storage.

---

### 2. 🤖 AI Extraction

* Email content is cleaned and passed to GPT.
* AI extracts:

  * BG Number
  * Credit Limit
  * Sale Amount

---

### 3. 🔍 Data Validation

* BG → Invoice mapping ensures correctness
* Invoice → Sales dataset enriches data

---

### 4. 📊 State Management

* **Current State (CSV)** → latest status
* **Journal (JSON)** → full event history

---

### 5. 📧 Approval Workflow

* Email sent to approver with links:

```plaintext
/api/approval?bg=BG123&action=approve
/api/approval?bg=BG123&action=reject
```

---

### 6. ✅ Decision Handling

* API updates status
* Prevents duplicate approvals
* Logs final action

---

## 📁 Project Structure

```plaintext
request-approval-system/
│
├── function_app.py
├── requirements.txt
├── host.json
├── local.settings.json (ignored)
│
├── src/
│   ├── email/
│   ├── processing/
│   ├── storage/
│   └── utils/
│
├── data/
├── docs/
│   └── technical_architecture.md
│
├── README.md
└── .gitignore
```

---

## 🧪 Running Locally

### 1. Install dependencies

```bash
pip install -r requirements.txt
```

---

### 2. Configure environment

Update `local.settings.json`:

```json
{
  "FUNCTION_APP_BASE_URL": "http://localhost:7072",
  "EMAIL_SENDER": "your_outlook_email",
  "EMAIL_PASSWORD": "app_password",
  "APPROVER_EMAIL": "approver_email",
  "ENABLE_EMAIL": "true"
}
```

---

### 3. Start the function app

```bash
func start --port 7072
```

---

### 4. Test approval API

```plaintext
http://localhost:7072/api/approval?bg=BG123&action=approve
```

---

## 📊 Data Design

### 🔹 Current State (CSV)

* One row per BG
* Stores latest status

### 🔹 Journal (JSON)

* Append-only
* Full audit history

---

## 🔒 Reliability Features

* Idempotent approval handling
* Duplicate action prevention
* Error logging
* Stateless processing
* Retry-safe design

---

## 💰 Cost Efficiency

| Component       | Estimated Cost |
| --------------- | -------------- |
| Azure Functions | ~$0–2/month    |
| Blob Storage    | ~$1/month      |
| OpenAI          | ~$1–5/month    |

👉 Designed to be **highly cost-efficient**

---

## 🚀 Future Enhancements

* 🔐 Secure (tokenized) approval links
* 📊 Dashboard for monitoring
* ⚖️ Rule-based validation engine
* 📧 Replace SMTP with enterprise email service
* 🧠 AI confidence scoring

---

## 💡 Business Impact

* Reduces manual effort by up to 80%
* Improves accuracy in financial processing
* Speeds up approval cycles
* Ensures full auditability

---

## 🧩 Summary

This system demonstrates a **production-grade architecture** combining:

* Serverless computing
* AI-powered automation
* Event-driven workflows
* Human-in-the-loop decision making

---

## 👤 Author

**Anshaj Singh**

---

## ⭐ If you found this useful

Consider giving the repo a star ⭐
