# 🤖 Pinecone RAG Chatbot

An intelligent, context-aware AI Agent built using **Retrieval-Augmented Generation (RAG)**, powered by **Pinecone** vector store and Large Language Models (LLM).

This project allows users to query custom domain knowledge (such as recipes, documentation, or internal company files) and receive accurate, hallucination-free responses grounded in real database context.

---

## 🚀 Features

- **Semantic Search Engine:** Leverages Pinecone vector database to perform dense vector similarity search.
- **Context-Grounded Answers:** Uses retrieved database chunks as prompt context to eliminate hallucinations.
- **Flexible Data Pipeline:** Supports document ingestion, text chunking, and embedding generation.
- **AI Agent Integration:** Built-in tool calling allowing the LLM agent to search Pinecone dynamically.

---

## 🏗️ Architecture Flow
[ User Query ] ➔ [ AI Agent / LLM ]
│
▼ (Tool Call Search)
[ Pinecone Vector DB ]
│
▼ (Returns Context Chunks)
[ Grounded Answer ] ◄─ [ AI Agent Response ]

1. **Ingestion:** Documents/Data are split into chunks and converted into high-dimensional vector embeddings.
2. **Storage:** Embeddings and metadata are indexed inside **Pinecone**.
3. **Retrieval:** When a user asks a question, the agent searches Pinecone for the top $k$ most relevant context vectors.
4. **Generation:** The LLM synthesizes a clean, structured answer based on the retrieved context.

---

## ⚙️ Prerequisites

Before running the project, ensure you have:

- [Pinecone Account & API Key](https://www.pinecone.io/)
- [OpenAI API Key](https://platform.openai.com/) (or Google Gemini API Key)
- An active Pinecone Index configured with matching embedding dimensions (e.g., `1536` for OpenAI `text-embedding-3-small`).

---

## 🛠️ Configuration & Setup

### 1. Vector Store Tool Description (For AI Agents / n8n)
When connecting Pinecone to an AI Agent tool node, configure the tool description as follows:

> **Tool Name:** `pinecone_knowledge_retriever`  
> **Description:** `Use this tool to search and retrieve relevant knowledge chunks, recipes, ingredients, and documentation from the Pinecone vector database.`

### 2. Recommended AI Agent System Prompt
```text
You are a knowledgeable and helpful AI assistant.

Core Guidelines:
1. Always query the attached Pinecone vector database tool when answering user questions.
2. Ground your answers strictly on the context returned from Pinecone.
3. Structure your responses using clear markdown formatting (bullet points, numbered lists).
4. If the information is not present in the vector store, clearly inform the user that it was not found in the knowledge base.

├── workflows/             # n8n export JSON templates
├── data/                  # Sample dataset or documents for ingestion
├── src/                   # Source code (if using LangChain / Node / Python)
├── .env.example           # API keys setup template
└── README.md              # Project documentation
Contributions, issues, and feature requests are welcome! Feel free to check the issues page.
This project is licensed under the MIT License.
