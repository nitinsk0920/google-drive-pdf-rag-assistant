# Google Drive PDF RAG Assistant

A no-code AI assistant built in **n8n** that automatically watches a specific **Google Drive folder** for new PDF files, extracts the text, stores embeddings in **Pinecone**, and answers user questions using **Retrieval-Augmented Generation (RAG)**.

The workflow also includes a chat-based AI Agent that retrieves relevant chunks from the uploaded PDF documents and generates concise answers.

---

## What this project does

- Monitors a specific Google Drive folder for new PDF files
- Downloads newly added PDFs automatically
- Extracts text from each PDF
- Splits the text into smaller chunks
- Generates embeddings using OpenAI
- Stores embeddings in Pinecone as a vector database
- Uses a chat-based AI Agent to answer questions from the uploaded PDFs
- Retrieves relevant text chunks from Pinecone during question answering
- Uses OpenRouter as the chat model provider

---

## Workflow overview

### Ingestion flow
When a new PDF is added to the watched Google Drive folder:

`Google Drive Trigger → Download file → Extract from File → Default Data Loader → Recursive Character Text Splitter → Embeddings OpenAI → Pinecone Vector Store`

### Question answering flow
When a user sends a chat message:

`When chat message received → AI Agent → Pinecone Vector Store Tool → OpenRouter Chat Model`

---

## Architecture

```text
Google Drive Trigger
    ↓
Download file
    ↓
Extract from File
    ↓
Pinecone Vector Store
    ├── Default Data Loader
    │   └── Recursive Character Text Splitter
    └── Embeddings OpenAI

When chat message received
    ↓
AI Agent
    ├── OpenRouter Chat Model
    └── Pinecone Vector Store (tool)
         └── Embeddings OpenAI


## Features in Detail

### 1. Google Drive Folder Monitoring

The workflow continuously monitors a specified Google Drive folder. Whenever a new PDF is uploaded, the ingestion pipeline is triggered automatically.

---

### 2. PDF Text Extraction

The newly uploaded PDF is downloaded from Google Drive and converted into plain text, making it suitable for further processing.

---

### 3. Text Chunking

The extracted text is divided into smaller chunks using a **Recursive Character Text Splitter**. This improves retrieval accuracy and embedding quality.

---

### 4. Embedding Generation

Each text chunk is converted into vector embeddings using **OpenAI Embeddings**, enabling semantic search.

---

### 5. Vector Storage

The generated embeddings are stored in **Pinecone**, which serves as the vector database for fast similarity search and document retrieval.

---

### 6. RAG-based Question Answering

When a user sends a query, the AI Agent retrieves the most relevant document chunks from Pinecone and uses them as context to generate an accurate response.

---

# Setup Instructions

### 1. Create a Google Drive Folder

Create a dedicated Google Drive folder that will contain all PDFs you want the workflow to index.

The **Google Drive Trigger** node will continuously monitor this folder for newly added files.

---

### 2. Set Up Pinecone

Create a Pinecone project and create an index to store document embeddings.

> **Note:** Ensure that the index dimension matches the embedding model you are using.

---

### 3. Configure OpenAI Credentials

Add your **OpenAI API Key** in n8n and connect it to the **OpenAI Embeddings** node.

---

### 4. Configure OpenRouter Credentials

Add your **OpenRouter API Key** in n8n and connect it to the **OpenRouter Chat Model** node.

---

### 5. Import the Workflow

Import the exported workflow JSON into n8n.

Reconnect the following credentials:

- Google Drive
- OpenAI
- Pinecone
- OpenRouter

---

# Important Notes

- Configure the **Google Drive Trigger** to watch the correct folder.
- Every newly uploaded PDF is automatically indexed.
- Use the **same embedding model** for both document ingestion and document retrieval.
- Pinecone stores all document embeddings and performs semantic similarity search.
- The AI Agent should always use the **Pinecone Vector Store** tool when answering questions related to uploaded documents.

---

# Example Usage

## 📄 Document Ingestion

Upload a PDF into the monitored Google Drive folder.

The workflow automatically:

- Downloads the PDF
- Extracts the document text
- Splits the text into chunks
- Generates vector embeddings
- Stores the embeddings in Pinecone

---

## 💬 Question Answering

Ask questions such as:

- *Summarize the uploaded PDF.*
- *What are the main skills mentioned in the document?*
- *Extract the key project details from the resume.*

The AI Agent retrieves the most relevant document chunks from Pinecone and generates an accurate response using Retrieval-Augmented Generation (RAG).
