# 🤖 Pinecone RAG Chatbot (n8n Workflow)

An intelligent, context-aware AI Agent workflow built in **n8n** using **Retrieval-Augmented Generation (RAG)**, powered by **Pinecone** vector database and LLMs (OpenAI / Gemini).

This workflow allows users to query custom domain knowledge (such as recipes, ingredients, cooking methods, or documentation) and receive accurate responses grounded directly in your Pinecone database context.

---

## 🚀 Features

* **Low-Code Automation:** Built entirely within **n8n** using native AI Agent and Vector Store nodes.
* **Semantic Vector Search:** Integrates Pinecone to perform dense vector similarity searches on incoming chat queries.
* **Context-Grounded Answers:** Uses retrieved database chunks as prompt context to prevent AI hallucinations.
* **Dynamic Tool Calling:** The n8n AI Agent calls the Pinecone Retriever node as an external tool automatically.

---

## 🏗️ Architecture & Data Flow

```
[ Chat Trigger ] ➔ [ AI Agent Node ] ──(Tool Call)──► [ Pinecone Vector Store ]
                          │                                     │
                          │◄─── (Top K Context Chunks) ─────────┘
                          ▼
              [ Grounded AI Response ]

```

1. **User Query:** The user sends a question via the Chat Trigger (`chatInput`).
2. **Tool Invocation:** The AI Agent inspects the tool description and queries Pinecone for matching vectors.
3. **Retrieval:** Pinecone returns the top $k$ relevant context chunks.
4. **Generation:** The AI Agent synthesizes a structured answer based strictly on the retrieved vector data.

---

## ⚙️ Prerequisites

* An active **[n8n Instance](https://n8n.io/)** (Cloud or Self-Hosted via Docker)
* **[Pinecone Account & API Key](https://www.pinecone.io/)** with an existing index
* **OpenAI API Key** (or Google Gemini / Ollama) for the LLM & Embedding models

---

## 🛠️ n8n Node Configuration

### 1. Pinecone Vector Store Node Settings

* **Operation Mode:** `Retrieve Documents (As Tool for AI Agent)`
* **Tool Name:** `pinecone_recipe_retriever`
* **Description:**
> `Use this tool to search and retrieve meal recipes, ingredients, and cooking methods from the Pinecone vector database.`



### 2. AI Agent Node Settings

* **Prompt / Input:** `{{ $('When chat message received').item.json.chatInput }}`
* **System Message:**
```text
You are a knowledgeable and helpful AI assistant.

Core Guidelines:
1. Always query the attached Pinecone vector database tool when answering user questions.
2. Ground your answers strictly on the context returned from Pinecone.
3. Structure your responses using clear markdown formatting (bullet points for ingredients, numbered lists for steps).
4. If the information is not present in the vector store, clearly inform the user that it was not found in the knowledge base.

```



---

## 📂 Project Structure

```text
.
├── workflows/
│   └── pinecone-rag-agent.json    # Exported n8n workflow template
├── data/
│   └── sample_recipes.json        # Sample knowledge base entries
├── .env.example                   # Environment variable setup
└── README.md                      # Documentation

```

---

## 🤝 Contributing & License

Contributions, issues, and feature requests are welcome! This project is licensed under the [MIT License](https://www.google.com/search?q=LICENSE).
