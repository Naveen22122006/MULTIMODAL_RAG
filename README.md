# Multimodal RAG

A multimodal Retrieval-Augmented Generation (RAG) system that extracts **text, tables, images, charts, and diagrams from PDF documents**, converts the extracted information into searchable embeddings, stores them in **Pinecone**, and uses **Groq LLMs** to answer user questions.

## 🚀 Features

- Extract text from PDF documents using PyMuPDF
- Extract tables and convert them into Markdown
- Extract images, charts, and diagrams
- Convert images to Base64 Data URIs
- Summarize visual content using a Groq vision model
- Create separate LangChain Documents for text, tables, and visuals
- Generate embeddings using HuggingFace `all-MiniLM-L6-v2`
- Store embeddings in Pinecone
- Perform semantic search using the top 5 relevant documents
- Automatically detect whether retrieved content contains visual information
- Use a text LLM for normal RAG queries
- Use a vision LLM when visual context is retrieved
- Display retrieved sources and images

## 🏗️ Architecture

```text
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
```

## 🛠️ Tech Stack

| Technology | Purpose |
|---|---|
| Python | Main development |
| PyMuPDF | PDF text/table/image extraction |
| Pandas | Table processing |
| Pillow | Image processing |
| Groq | Text and vision LLMs |
| LangChain | RAG and document processing |
| HuggingFace | Embeddings |
| Pinecone | Vector database |
| Jupyter Notebook | Development |
| uv | Environment/package management |

## 🤖 Models

| Role | Model |
|---|---|
| Text LLM | `openai/gpt-oss-20b` |
| Vision LLM | `qwen/qwen3.8-27b` |
| Embedding model | `sentence-transformers/all-MiniLM-L6-v2` (384-dimensional vectors) |

## 📄 Multimodal PDF Processing

Each PDF page is processed for three modalities: **Text**, **Table**, and **Visual**.

**Text**

```text
PDF → Text → LangChain Document
```

**Tables**

```text
PDF Table → Pandas DataFrame → Markdown → LangChain Document
```

**Images / Charts / Diagrams**

```text
PDF Image
   ↓
Extract Image
   ↓
Base64 Data URI
   ↓
Groq Vision Model
   ↓
Visual Summary
   ↓
LangChain Document
```

Each document contains metadata such as:

- `page`
- `modality`
- `source`
- `table_number`
- `image_path`

## 🔢 Embeddings & Vector Database

Extracted documents are converted into embeddings using `all-MiniLM-L6-v2` with a dimension of 384 and normalization enabled.

Pinecone configuration:

| Setting | Value |
|---|---|
| Index | `multimodal-rag` |
| Namespace | `demo_fy2026` |
| Metric | cosine |
| Vector type | dense |
| Dimension | 384 |
| Cloud | AWS |
| Region | us-east-1 |

## ♻️ Duplicate Prevention

The project creates stable IDs for extracted documents using their source, page, modality, and document position.

```text
Document
   ↓
Generate ID
   ↓
Check Pinecone
   ├── Already exists → Skip
   └── New document   → Upload
```

This prevents the same documents from being uploaded repeatedly.

## 🔎 Retrieval

The system creates a Pinecone retriever with `k = 5`.

```text
Question
   ↓
Embedding
   ↓
Pinecone Semantic Search
   ↓
Top 5 Relevant Documents
```

The retrieved documents provide the context used by the LLM.

## 🧠 RAG Pipeline

**Normal text questions**

```text
User Question
     ↓
Pinecone Retrieval
     ↓
Relevant Context
     ↓
Prompt
     ↓
Groq Text LLM
     ↓
Answer
```

**When retrieved documents contain visual information**

```text
User Question
     ↓
Pinecone Retrieval
     ↓
Visual Document Found
     ↓
Load Original Image
     ↓
Convert Image to Base64
     ↓
Groq Vision LLM
     ↓
Answer
```

Up to three retrieved visual images can be provided to the vision model.

## 💬 Example

The notebook provides an `ask()` function:

```python
ask("give me brief about NovaCore")
```

The function:

1. Retrieves relevant documents from Pinecone
2. Builds the retrieved context
3. Checks for visual documents
4. Selects the appropriate LLM
5. Generates the answer
6. Displays retrieved sources
7. Displays retrieved images when available

## ⚙️ Setup

Create the environment:

```bash
uv venv
```

Activate on Windows:

```powershell
.venv\Scripts\Activate.ps1
```

Install dependencies:

```bash
uv sync
```

Install the Jupyter kernel:

```bash
uv add ipykernel
python -m ipykernel install --user --name multimodelrag
```

Open `main.ipynb` and select the `multimodelrag` kernel.

## 📁 Project Structure

```text
multimodelrag/
│
├── main.ipynb
├── README.md
├── requirements.txt
├── pyproject.toml
├── uv.lock
├── .gitignore
└── src/
```

> **Note:** Add `data.pdf` to the project's root folder. It is used as the fallback data source when the primary PDF fails to load.

## 👨‍💻 Author

**Naveen Manuka**
NIT Srinagar
Email: [naveenmanuka710@gmail.com](mailto:naveenmanuka710@gmail.com)
