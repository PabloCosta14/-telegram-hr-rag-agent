# 🤖 AI HR Assistant with RAG (Telegram + MySQL)

An intelligent AI agent integrated into Telegram that answers employee queries using **Retrieval-Augmented Generation (RAG)** techniques. It connects to a **MySQL** database for real-time personal data (vacation balances, hour banks) and a Vector Store for general company policies. 

A key feature of this workflow is the implementation of **Relevance Guardrails**, ensuring the bot only responds to HR-related topics and gracefully handles out-of-scope questions.

## 🛠️ Tech Stack

- **Orchestration:** [n8n](https://n8n.io/)
- **LLM:** Cohere Chat Model (Command R+)
- **Embeddings:** Cohere Embed Multilingual v3.0
- **Database:** MySQL (Structured employee data: vacations, hours, departments)
- **Vector Store:** In-Memory Vector Store (General HR policies & documents)
- **Interface:** Telegram Bot API
- **Language:** Spanish (Native support for multilingual embeddings)

## ✨ Key Features

1. **🛡️ Relevance Guardrails (Switch Logic):** 
   - Before processing complex queries, the system evaluates user intent.
   - If the question is unrelated to HR (e.g., weather, jokes, general knowledge), the bot returns a predefined polite message, saving tokens and preventing hallucinations.
   - If relevant, it proceeds to the RAG pipeline.

2. **🔍 Hybrid Data Retrieval:**
   - **Structured Data:** Uses SQL tools to query specific employee records (e.g., "How many vacation days do I have left?") by matching the user's name.
   - **Unstructured Data:** Uses Vector Search to retrieve context from company handbooks and general HR policies.

3. **👤 Personalized Context:**
   - The agent identifies the user by name and fetches their specific `saldo_vacaciones` (vacation balance) and `banco_horas` (hour bank) from the MySQL database.
   - If the user is not found in the database, it falls back to general policy answers without inventing personal data.

4. **💬 Conversational Memory:**
   - Maintains short-term memory to handle follow-up questions naturally within the same session.

## 🏗️ Architecture Diagram

```mermaid
graph TD
    A[📱 Telegram Trigger] --> B{🧠 Intent Classifier}
    B -- ❌ Irrelevant --> C[💬 Predefined 'Out of Scope' Message]
    B -- ✅ Relevant --> D[🤖 AI Agent with Tools]
    D --> E[🔍 Tool 1: MySQL Query]
    D --> F[📚 Tool 2: Vector Store Search]
    E & F --> G[📝 LLM Generates Response]
    C & G --> H[📤 Send Reply to Telegram]
