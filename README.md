# Ai_Customer_Support_Bot
Project Overview
This project implements a high-performance AI Customer Support Bot designed to handle common customer inquiries using Retrieval-Augmented Generation (RAG) and maintain contextual memory throughout a conversation. It is exposed as a robust FastAPI backend service, fulfilling the requirements for conversational accuracy and session management.

Key Technical Features
RESTful API: Implemented using FastAPI with two primary endpoints: /chat for conversation and /history for session retrieval.

LLM Integration: Leverages the gemini-2.5-flash model for intelligent response generation, summarization, and contextual awareness.

RAG System: Utilizes a pre-indexed FAQs dataset to ground responses, ensuring accuracy and relevance.

Contextual Memory: Maintains conversation history within a session to support complex follow-up questions.

Escalation Logic: Simulates human hand-off when the bot cannot confidently answer a query.

Session Management: Database integration (e.g., in-memory dictionary or persistent DB) to track conversation history by session_id.

🚀 Getting Started
Prerequisites
Python 3.10+

A Gemini API Key.

An ngrok Auth Token (for public access testing).

Setup and Running Locally
Clone the Repository:

Bash

git clone [YOUR REPO URL]
cd ai-customer-support-bot
Install Dependencies:

Bash

pip install -r requirements.txt
Set Environment Variables:
Create a .env file or export your keys in the terminal:

Bash

export GEMINI_API_KEY="YOUR_GEMINI_API_KEY"
export NGROK_AUTH_TOKEN="YOUR_NGROK_AUTH_TOKEN"
# Note: If running on Colab, set these within the notebook environment.
Run the FastAPI Server:
Execute the main application file, which starts both the Uvicorn server and the ngrok tunnel.

Bash

python app.py
The server will start, and the console will output the public Ngrok URL, e.g.: https://unmonitored-perorational-nathaniel.ngrok-free.dev.

🤖 LLM Implementation & Prompt Documentation
The system is engineered to handle three distinct conversational states: RAG, Contextual Follow-up, and Escalation.

1. System Prompt (The Core Instruction)
The following System Prompt governs the bot's tone, memory, and RAG usage:

Markdown

You are an AI Customer Support Agent for a modern e-commerce platform.
Your persona is friendly, professional, and efficient.
Your primary goal is to answer customer questions accurately using the provided FAQs.

**RULES:**
1.  **Tone:** Be polite, professional, and concise. Do not use overly complex language.
2.  **RAG/Fact-Grounded Response:** Always base your answer solely on the provided `RAG_CONTEXT`. If the context contains a definitive answer, provide it directly.
3.  **Contextual Memory:** Use the `CHAT_HISTORY` to understand the current user query, especially if it relies on pronouns (like 'it' or 'that') or refers to a previous topic. Summarize the history only when necessary for context.
4.  **Escalation Logic (Crucial):** If the `RAG_CONTEXT` is empty, or if your internal confidence score in the RAG result is too low, you MUST trigger the Escalation response. Do not invent an answer.
5.  **Output Format:** Provide only the final response text. Do not include internal thoughts, scores, or intermediate steps.

**CONTEXTUAL INPUTS:**
- RAG_CONTEXT: [Content retrieved from the FAQs database]
- CHAT_HISTORY: [Previous conversation turns in a USER: X | ASSISTANT: Y format]
- CURRENT_USER_QUERY: [The user's latest message]
2. Escalation Logic (The Fail-Safe)
The bot simulates a hand-off to a human agent when it cannot find a high-confidence answer. This logic is triggered programmatically before the final LLM call.

Escalation Condition:

The bot initiates escalation if the RAG search returns fewer than 1 highly relevant document (i.e., the similarity score for the top result is below 0.5 or the result list is empty).

Escalation Message:

"I apologize, I was unable to find a definitive answer in our FAQs. I will now simulate an escalation to a human agent. Please provide your order number and we will connect you to a specialist shortly."

🔌 API Endpoints
The API documentation is accessible via the public URL at the /docs route.

Endpoint	Method	Description	Request Body
/chat	POST	The main conversational endpoint. Processes the user message, updates history, runs RAG, and returns the LLM response.	{ "session_id": "string", "user_message": "string" }
/history/{session_id}	GET	Retrieves the full conversation transcript for a given session. Essential for session management and debugging.	None (requires session_id in path)
/	GET	Read Root / API Health Check.	None

Export to Sheets
🎥 Demo Video
Watch the short demonstration video to see all required features in action:

RAG Response: (Answering an FAQ question)

Contextual Memory: (Handling a pronoun follow-up)

Escalation: (Triggering the fail-safe message)

Session History: (Retrieving the full transcript via the /history endpoint)
