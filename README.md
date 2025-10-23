# Ai_Customer_Support_Bot

# DEMO VIDEO: [https://drive.google.com/file/d/1HrHmekZD_nWIV-pDi8w1BrmucEiHCrpu/view?usp=sharing]

# 🤖 AI Customer Support Bot

![Python](https://img.shields.io/badge/Python-3.10%2B-blue)
![FastAPI](https://img.shields.io/badge/FastAPI-Framework-success)
![LLM](https://img.shields.io/badge/Powered%20By-Gemini%202.5%20Flash-orange)
![License](https://img.shields.io/badge/License-MIT-green)



## 🧠 Project Overview
The **AI Customer Support Bot** is a high-performance conversational assistant built using **FastAPI**.  
It leverages **Retrieval-Augmented Generation (RAG)** and **contextual memory** to deliver accurate, context-aware, and human-like responses to customer inquiries.

This project is designed for **e-commerce platforms** to handle FAQs, manage sessions, and simulate human escalation when necessary.

---

## ⚙️ Key Technical Features

- **🧩 RESTful API:**  
  Built using **FastAPI**, with two key endpoints:
  - `/chat` — handles real-time user queries  
  - `/history` — retrieves chat history for a session  

- **🤖 LLM Integration:**  
  Uses the **Gemini 2.5 Flash** model for:
  - Intelligent and accurate responses  
  - Summarization of context  
  - Maintaining conversational awareness  

- **📚 Retrieval-Augmented Generation (RAG):**  
  Utilizes a **pre-indexed FAQ dataset** to ensure fact-grounded, relevant answers.

- **🧠 Contextual Memory:**  
  Maintains ongoing session context for understanding follow-up queries.

- **🚨 Escalation Logic:**  
  Simulates a **human hand-off** when the bot lacks confidence or relevant data.

- **💾 Session Management:**  
  Tracks user interactions using session IDs — supports both in-memory and persistent databases.

---

## 🚀 Getting Started

### 🧰 Prerequisites
- Python **3.10+**
- **Gemini API Key**
- **Ngrok Auth Token** (for testing public access)

---

### ⚡ Setup and Run Locally

1. **Clone the Repository**
   ```bash
   git clone https://github.com/khushikumari0202/Ai_Customer_Support_Bot
   cd ai-customer-support-bot
   ```


2. **Install Dependencies**

   ```bash
   pip install -r requirements.txt
   ```

3. **Set Environment Variables**
   Create a `.env` file in the project root or export them directly:

   ```bash
   export GEMINI_API_KEY="YOUR_GEMINI_API_KEY"
   export NGROK_AUTH_TOKEN="YOUR_NGROK_AUTH_TOKEN"
   ```

   > 💡 If running in Google Colab, set these variables inside your notebook.

4. **Run the FastAPI Server**

   ```bash
   python app.py
   ```

   Once running, you’ll get a public URL from ngrok like:

   ```
   https://unmonitored-perorational-nathaniel.ngrok-free.dev
   ```

---

## 🧠 LLM Implementation & Prompt Design

The system is engineered to manage **three conversational states**:

1. **RAG-Based Response**
2. **Contextual Follow-up**
3. **Escalation (Human Hand-off)**

---

### 🧾 System Prompt (Core Instruction)

> You are an AI Customer Support Agent for a modern e-commerce platform.
> Your persona is **friendly, professional, and efficient**.
> Your primary goal is to answer customer questions accurately using the provided FAQs.

**RULES:**

1. **Tone:** Be polite, professional, and concise.
2. **Fact-Grounded Response:** Always use `RAG_CONTEXT` for answers.
3. **Contextual Memory:** Refer to `CHAT_HISTORY` for follow-ups.
4. **Escalation:** If unsure or lacking data, escalate — never guess.
5. **Output:** Return only the final response (no reasoning or logs).

**CONTEXTUAL INPUTS:**

```
RAG_CONTEXT: what are the ways to pay for my order
CHAT_HISTORY: We accept major credit cards, debit cards, and PayPal as payment methods for online orders. | Can I use that for my refund?
CURRENT_USER_QUERY: I apologize, I was unable to find a definitive answer in our FAQs. I will now simulate an escalation to a human agent.
```

---

### ⚠️ Escalation Logic (Fail-Safe)

If the RAG search returns fewer than one relevant document
(i.e., similarity score < 0.5 or no results), the bot triggers escalation.

**Escalation Message:**

> "I apologize, I was unable to find a definitive answer in our FAQs.
> I will now simulate an escalation to a human agent.
> Please provide your order number and we will connect you to a specialist shortly."

---

## 🔌 API Endpoints

| Endpoint                | Method | Description                                          | Request Body                                           |
| ----------------------- | ------ | ---------------------------------------------------- | ------------------------------------------------------ |
| `/`                     | GET    | API Health Check                                     | None                                                   |
| `/chat`                 | POST   | Handles chat input, runs RAG, and returns LLM output | `{ "session_id": "string", "user_message": "string" }` |
| `/history/{session_id}` | GET    | Fetches full chat history for a session              | None                                                   |

**📘 API Documentation:**
Available automatically at 👉 **`/docs`**

---

## 🎥 Demo Features

* ✅ **RAG Response:** Answers common FAQ questions
* 🧠 **Contextual Memory:** Understands pronouns and follow-ups
* 🚨 **Escalation:** Gracefully transfers unresolved cases
* 💬 **Session History:** Tracks complete conversation transcripts

---

## 🧩 Example Workflow

**User:** "What is your return policy?"
→ Bot answers based on RAG data

**User:** "How long does it take?"
→ Bot uses **contextual memory** to recall “return policy” context

**User:** "Do you sell flight tickets?"
→ Bot **escalates** since it’s beyond its knowledge scope

---

## 🛠️ Tech Stack

| Component                      | Technology                      |
| ------------------------------ | ------------------------------- |
| **Backend Framework**          | FastAPI                         |
| **Language**                   | Python                          |
| **Model**                      | Gemini 2.5 Flash                |
| **Vector Search / RAG Engine** | FAISS / Custom Embedding Search |
| **Session Storage**            | In-memory or Persistent DB      |
| **Tunneling**                  | Ngrok                           |

---

## 🏁 Future Enhancements

* 🔐 Add authentication and role-based access
* 📈 Integrate live chat dashboard for human agents
* 🧩 Expand FAQ dataset dynamically
* ☁️ Add persistent cloud storage for conversations

---

## 🧭 Project Architecture

```text
         ┌────────────────────────┐
         │        User            │
         └────────────┬───────────┘
                      │
                      ▼
          ┌────────────────────┐
          │     FastAPI API     │
          │   (app.py server)   │
          └─────────┬───────────┘
                    │
     ┌──────────────┼────────────────┐
     │                              │
     ▼                              ▼
┌──────────────┐          ┌───────────────────┐
│  RAG System  │          │  Context Manager  │
│(FAQ Retriever│◄────────►│(Chat Memory/State)│
└──────┬───────┘          └───────────────────┘
       │
       ▼
┌──────────────────────┐
│  Gemini 2.5 Flash LLM│
│ (Response Generation)│
└──────────────────────┘
```

---

## 👨‍💻 Author

**Khushi Kumari**
📧 *kk2682117@gmail.com*
🌐 *[https://www.linkedin.com/in/khushi-kumari-582a02241/]*

---

## 📜 License

This project is licensed under the **MIT License** — free to use, modify, and distribute.

---

⭐ **If you like this project, don't forget to give it a star on GitHub!**

