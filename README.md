Multimodal RAG

A multimodal Retrieval-Augmented Generation (RAG) system that extracts text, tables, images, charts, and diagrams from PDF documents, converts the extracted information into searchable embeddings, stores them in Pinecone, and uses Groq LLMs to answer user questions.

🚀 Features
Extract text from PDF documents using PyMuPDF
Extract tables and convert them into Markdown
Extract images, charts, and diagrams
Convert images to Base64 Data URIs
Summarize visual content using a Groq vision model
Create separate LangChain Documents for text, tables, and visuals
Generate embeddings using HuggingFace all-MiniLM-L6-v2
Store embeddings in Pinecone
Perform semantic search using the top 5 relevant documents
Automatically detect whether retrieved content contains visual information
Use a text LLM for normal RAG queries
Use a vision LLM when visual context is retrieved
Display retrieved sources and images


🏗️ Architecture

                         PDF
                          │
                          ▼
                       PyMuPDF
                          │
             ┌────────────┼────────────┐
             ▼            ▼            ▼
           Text         Tables      Images
             │            │            │
             │        Markdown     Base64 URI
             │            │            │
             │            │       Groq Vision
             │            │            │
             └────────────┼────────────┘
                          ▼
                 LangChain Documents
                          │
                          ▼
              HuggingFace Embeddings
                          │
                          ▼
                       Pinecone
                          │
                          ▼
                    Top 5 Retrieval
                          │
                 ┌────────┴────────┐
                 ▼                 ▼
             Text Context      Visual Context
                 │                 │
                 ▼                 ▼
             Text LLM          Vision LLM
                 │                 │
                 └────────┬────────┘
                          ▼
                     Final Answer
🛠️ Tech Stack

Python main development
PyMuPDF	PDF text/table/image extraction
Pandas	Table processing
Pillow	Image processing
Groq	Text and vision LLMs
LangChain	RAG and document processing
HuggingFace	Embeddings
Pinecone	for Vector database
Jupyter Notebook	Development
uv	Environment/package management

🤖 Models

Text LLM-->	openai/gpt-oss-20b
Vision LLM-->	qwen/qwen3.8-27b
Embedding model-->	sentence-transformers/all-MiniLM-L6-v2 (384-dimensional vectors)

  
⚙️ Setup

Create the environment:

bash
uv venv

Activate on Windows:

powershell
.venv\Scripts\Activate.ps1

Install dependencies:

bash
uv sync

Install the Jupyter kernel:

bash
uv add ipykernel
python -m ipykernel install --user --name multimodelrag

Open main.ipynb and select the multimodelrag kernel.

📁 Project Structure

multimodelrag/
│
├── main.ipynb
├── README.md
├── requirements.txt
├── pyproject.toml
├── uv.lock
├── .gitignore
└── src/

Note :
  Add data.pdf to home folder because it was used as fallback when primary one fails this file is used as data.




👨‍💻 Author

Naveen Manuka
NIT Srinagar
Mail : naveenmanuka710@gmail.com

