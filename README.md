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


Features in detail
1. Google Drive folder monitoring

The workflow watches a chosen Google Drive folder and starts ingestion whenever a new PDF is added.

2. PDF text extraction

The PDF is downloaded and parsed into plain text so it can be processed by the vector pipeline.

3. Chunking

The extracted text is split into smaller chunks using a Recursive Character Text Splitter.

4. Embeddings

Each chunk is converted into vector embeddings using OpenAI embeddings.

5. Vector storage

The embeddings are stored in Pinecone for similarity search and retrieval.

6. RAG question answering

When a chat message is received, the AI Agent queries Pinecone for relevant chunks and uses them to generate an answer.

Setup instructions
1. Create your Google Drive folder

Create a dedicated folder in Google Drive for PDFs that should be indexed.

The workflow will watch this folder for new files.

2. Set up Pinecone

Create a Pinecone project and an index for your document embeddings.

Make sure the vector dimension matches your embedding model.

3. Add OpenAI credentials

Add your OpenAI API key in n8n for embeddings.

4. Add OpenRouter credentials

Add your OpenRouter API key in n8n for the chat model used by the AI Agent.

5. Configure the n8n workflow

Import the workflow JSON into n8n and connect the required credentials:

Google Drive
OpenAI
Pinecone
OpenRouter
Important notes
The Google Drive Trigger should point to the exact folder you want to monitor.
The ingestion flow runs automatically when a new PDF is added.
The same embedding model should be used consistently for ingestion and retrieval.
Pinecone acts as the vector database for retrieving relevant PDF text chunks.
The AI Agent should use the Pinecone tool for answering document-related questions.
Example usage
Ingestion

Add a PDF to the monitored Google Drive folder.

The workflow automatically:

downloads the PDF
extracts the text
splits the text
embeds the chunks
stores them in Pinecone
Question answering

Ask a question in chat, such as:

Summarize the uploaded PDF
What are the main skills mentioned in the document?
Extract the key project details from the resume

The AI Agent searches Pinecone and responds using the most relevant chunks.

Suggested project structure
