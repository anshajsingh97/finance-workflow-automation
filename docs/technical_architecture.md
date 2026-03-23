# Technical Architecture – AI-Powered Request Approval System

## 📌 Overview

This system is an event-driven, serverless workflow designed to automate the processing and approval of financial requests received via email. It leverages AI for data extraction, cloud storage for persistence, and APIs for human-in-the-loop decision making.

---

## 🧠 Architecture Summary

The system follows a layered architecture:

* **Ingestion Layer** – Captures incoming emails
* **Processing Layer** – Extracts and enriches data
* **Persistence Layer** – Stores state and history
* **Workflow Layer** – Handles approvals
* **Notification Layer** – Sends approval requests

---

## 🔄 End-to-End Workflow

1. Email is received and stored in Azure Blob Storage
2. Blob trigger activates Azure Function
3. Email content is cleaned and processed
4. Azure OpenAI extracts structured data
5. System performs lookup and validation
6. Record is stored in:

   * Current State (CSV)
   * Journal (event log)
7. Approval email is sent to approver
8. Approver clicks approve/reject link
9. HTTP-triggered function updates status
10. Final state and audit logs are stored

---

## ⚙️ Components and Services

### 1. Azure Blob Storage

**Purpose:**

* Store incoming emails
* Maintain lookup datasets
* Persist journal logs
* Maintain current state

**Why used:**

* Cost-effective
* Scalable
* Native integration with Azure Functions

---

### 2. Azure Functions

#### Blob Trigger Function

* Triggered on new email file
* Handles processing pipeline

#### HTTP Trigger Function

* Handles approval actions
* Updates system state

**Why used:**

* Serverless execution
* Event-driven architecture
* Automatic scaling

---

### 3. Azure OpenAI (GPT-4o-mini)

**Purpose:**

* Extract structured data from unstructured email content

**Why used:**

* High accuracy for text extraction
* Cost-efficient
* Fast response time

**Role in system:**

* Converts email text → JSON

---

### 4. Pandas (Data Processing)

**Purpose:**

* Perform lookup operations
* Handle CSV-based state management

---

### 5. SMTP (Email Notification)

**Purpose:**

* Send approval requests to users

**Limitations:**

* Not ideal for production-scale systems
* Will be replaced with enterprise email service

---

## 🧾 Data Architecture

### 1. Journal (Event Log)

* Stored as JSON files
* Append-only
* Maintains full history

Example:

```json
{
  "bg_number": "BG123",
  "status": "pending_approval",
  "event_type": "sent_for_approval"
}
```

---

### 2. Current State (CSV)

* One row per BG number
* Stores latest status

Example:

```
bg_number,status,last_updated
BG123,pending_approval,2026-03-20
```

---

### Design Pattern

This follows:

**Event Sourcing + State Projection**

* Journal = history
* CSV = latest state

---

## 🔍 Data Validation Logic

* BG number is treated as primary key
* Mapping ensures:

  * BG → Invoice
  * Invoice → Sales Data

This prevents reliance on AI-extracted values for critical joins.

---

## 📩 Approval Workflow

* Email contains:

  * Request details
  * Approve/Reject links

Example:

```
/api/approval?bg=BG123&action=approve
```

---

## 🔒 System Reliability Features

* Idempotent approval handling
* Duplicate action prevention
* Case normalization
* Error logging
* Retry-safe operations

---

## 💰 Cost Considerations

| Component       | Estimated Monthly Cost |
| --------------- | ---------------------- |
| Azure Functions | ~$0–2                  |
| Blob Storage    | ~$1                    |
| Azure OpenAI    | ~$1–5                  |
| Email (SMTP)    | Free                   |

---

## 🚀 Scalability

* Azure Functions auto-scale based on events
* Blob Storage supports large-scale ingestion
* Stateless processing ensures horizontal scalability

---

## 🔮 Future Enhancements

* Token-based secure approval links
* Dashboard for monitoring
* Rule-based validation engine
* Replace SMTP with Azure Email service
* Add confidence scoring for AI extraction

---

## 🧩 Conclusion

This system demonstrates a production-grade architecture combining:

* Serverless computing
* AI-based data extraction
* Event-driven processing
* Human-in-the-loop workflows

It significantly reduces manual effort, improves accuracy, and ensures auditability in financial approval processes.
