# C-RAG-chatbot-remote-using-llm
💬 A C++ RAG Chatbot built with Streamlit, LangChain, FAISS, HuggingFace Embeddings, and Ollama. It retrieves relevant information from a C++ knowledge base and generates answers using the Gemma 2B local LLM.
# 💬 C++ RAG Chatbot using Ollama

A simple **Retrieval-Augmented Generation (RAG) chatbot for C++** built using **Python, Streamlit, LangChain, FAISS, HuggingFace Embeddings, and Ollama**.

The application uses a local `C++_Introduction.txt` file as its knowledge base. When a user asks a question, the application searches the document for relevant information and sends the retrieved context to the **Gemma 2B** model running locally through Ollama.

---

## 🚀 Features

* 💬 Simple Streamlit web interface
* 📚 Uses a local C++ knowledge document
* 🔎 Semantic similarity search with FAISS
* 🧠 HuggingFace sentence-transformer embeddings
* 🤖 Local AI responses using Ollama
* ⚡ Uses the `gemma2:2b` model
* 🔐 No external API key required
* 📖 Context-based answers using RAG
* 💻 Runs locally on your computer

---

## 🛠️ Technologies Used

| Technology            | Purpose                   |
| --------------------- | ------------------------- |
| Python                | Main programming language |
| Streamlit             | Web interface             |
| LangChain             | RAG and LLM integration   |
| FAISS                 | Vector database           |
| HuggingFace           | Text embeddings           |
| Sentence Transformers | Embedding model           |
| Ollama                | Local LLM execution       |
| Gemma 2B              | AI language model         |

---

## 🧠 What is RAG?

**RAG stands for Retrieval-Augmented Generation.**

Instead of asking the AI model to answer a question directly, the application first retrieves relevant information from a knowledge base.

That information is then given to the language model as context.

### RAG Workflow

```text
                C++_Introduction.txt
                        │
                        ▼
                  TextLoader
                        │
                        ▼
                Text Splitting
                        │
                        ▼
              HuggingFace Embeddings
                        │
                        ▼
                  FAISS Database
                        │
                        ▼
                  User Question
                        │
                        ▼
                Similarity Search
                        │
                        ▼
               Relevant C++ Context
                        │
                        ▼
                 Prompt Creation
                        │
                        ▼
                 Ollama Gemma 2B
                        │
                        ▼
                   Final Answer
```

---

# 📁 Project Structure

```text
C++-RAG-Chatbot/
│
├── app.py
├── C++_Introduction.txt
├── requirements.txt
└── README.md
```

### File Description

**`app.py`**

Contains the complete Streamlit application, document processing, embeddings, FAISS vector database, and Ollama LLM code.

**`C++_Introduction.txt`**

The C++ document used as the chatbot's knowledge base.

**`requirements.txt`**

Contains the Python dependencies required to run the project.

**`README.md`**

Project documentation.

---

# ⚙️ Installation

## 1. Clone the Repository

```bash
git clone https://github.com/YOUR-USERNAME/C++-RAG-Chatbot.git
```

Go to the project folder:

```bash
cd C++-RAG-Chatbot
```

---

## 2. Create a Virtual Environment

For Windows:

```powershell
python -m venv .venv
```

Activate the environment:

```powershell
.venv\Scripts\activate
```

You should see something similar to:

```text
(.venv) PS C:\...\C++-RAG-Chatbot>
```

---

## 3. Install Python Dependencies

Run:

```powershell
pip install -r requirements.txt
```

Or install them manually:

```powershell
pip install streamlit langchain langchain-community langchain-text-splitters langchain-huggingface faiss-cpu sentence-transformers
```

---

# 🤖 Install and Configure Ollama

This project uses **Ollama** to run the language model locally.

Check whether Ollama is installed:

```powershell
ollama --version
```

Download the required Gemma model:

```powershell
ollama pull gemma2:2b
```

Check the installed models:

```powershell
ollama list
```

You should see:

```text
gemma2:2b
```

Test the model:

```powershell
ollama run gemma2:2b
```

If the model responds correctly, Ollama is ready.

---

# ▶️ Run the Application

Make sure your virtual environment is activated.

Run:

```powershell
python -m streamlit run app.py
```

Streamlit will start the application.

Open the URL shown in the terminal, normally:

```text
http://localhost:8501
```

---

# 💬 How to Use

After opening the application, you will see:

```text
💬 C++ RAG Chatbot using Ollama

Ask a question about C++:
```

Enter a C++ question.

For example:

```text
What is C++?
```

Other examples:

```text
What are the features of C++?
```

```text
What is object-oriented programming?
```

```text
What is a class in C++?
```

The application retrieves relevant information from the C++ document and generates an answer using Gemma 2B.

---

# 🔍 How the Code Works

## 1. Load the C++ Document

The application loads the knowledge base using `TextLoader`:

```python
loader = TextLoader(
    "C++_Introduction.txt",
    encoding="utf-8"
)

documents = loader.load()
```

The contents of the text file become the source of information for the chatbot.

---

## 2. Split the Document

The document is divided into smaller chunks:

```python
text_splitter = RecursiveCharacterTextSplitter(
    chunk_size=200,
    chunk_overlap=20
)
```

This makes the document easier to search.

---

## 3. Create Embeddings

The project uses:

```python
HuggingFaceEmbeddings(
    model_name="sentence-transformers/all-MiniLM-L6-v2"
)
```

The text is converted into numerical vector representations.

These vectors allow the application to compare the meaning of the user's question with the document content.

---

## 4. Create FAISS Vector Database

The document chunks are stored in FAISS:

```python
db = FAISS.from_documents(
    final_docs,
    embeddings
)
```

FAISS is then used for fast similarity searching.

---

## 5. Search for Relevant Information

When the user enters a question:

```python
docs = db.similarity_search(
    user_question
)
```

FAISS finds the document sections that are most relevant to the question.

---

## 6. Create the Context

The retrieved document sections are combined:

```python
context = "\n".join(
    [doc.page_content for doc in docs]
)
```

This context is added to the prompt.

---

## 7. Generate the Answer

The application uses Ollama with Gemma 2B:

```python
llm = Ollama(
    model="gemma2:2b"
)
```

The prompt is sent to the model:

```python
response = llm.invoke(prompt)
```

The generated response is then displayed in Streamlit.

---

# 🧩 Main Components

### Streamlit

Creates the graphical web interface.

### TextLoader

Loads the C++ knowledge document.

### RecursiveCharacterTextSplitter

Splits the document into smaller chunks.

### HuggingFace Embeddings

Converts text into vector representations.

### FAISS

Stores the vectors and performs similarity search.

### Ollama

Runs the LLM locally.

### Gemma 2B

Generates the final answer using the retrieved context.

---

# 📦 requirements.txt

Create a file named `requirements.txt` and add:

```text
streamlit
langchain
langchain-community
langchain-text-splitters
langchain-huggingface
faiss-cpu
sentence-transformers
```

> **Note:** Ollama is installed separately on your system and is not installed using `pip`.

---

# ⚠️ Troubleshooting

## Streamlit Command Not Found

If you get:

```text
'streamlit' is not recognized
```

use:

```powershell
python -m streamlit run app.py
```

instead of:

```powershell
streamlit run app.py
```

---

## Ollama Model Not Found

Check the installed models:

```powershell
ollama list
```

If `gemma2:2b` is missing:

```powershell
ollama pull gemma2:2b
```

Then test:

```powershell
ollama run gemma2:2b
```

---

## C++ Document Not Found

Make sure `C++_Introduction.txt` is in the same folder as `app.py`:

```text
C++-RAG-Chatbot/
│
├── app.py
└── C++_Introduction.txt
```

---

## Ollama Server Error

First test Ollama separately:

```powershell
ollama run gemma2:2b
```

If the model works, exit the Ollama session and restart Streamlit:

```powershell
python -m streamlit run app.py
```

---

# 🔮 Future Improvements

The project can be improved by adding:

* 💬 Chat history
* 🧠 Conversation memory
* 📄 PDF document support
* 📚 Multiple knowledge documents
* 🔎 Similarity scores
* ⚡ Streaming responses
* 🎨 Improved Streamlit UI
* 💾 Persistent FAISS database
* 💻 C++ code generation
* 🐛 C++ code debugging
* 🌐 Online deployment
* 🔐 User authentication

---

# 🎯 Learning Objectives

This project helps demonstrate the fundamentals of:

* Retrieval-Augmented Generation
* Large Language Models
* Vector databases
* Semantic search
* Text embeddings
* LangChain
* FAISS
* HuggingFace Transformers
* Ollama
* Streamlit
* Local AI applications

---

# 🔒 Local AI

The chatbot uses Ollama to run the Gemma 2B model locally.

The basic flow is:

```text
User
 ↓
Streamlit
 ↓
FAISS Similarity Search
 ↓
Relevant C++ Context
 ↓
Prompt
 ↓
Ollama
 ↓
Gemma 2B
 ↓
Answer
```

This allows the project to work without an external LLM API key.

---

# 👨‍💻 Author

**Ayush**

A learning project focused on exploring **Generative AI, Retrieval-Augmented Generation, vector databases, embeddings, LangChain, and local LLMs**.

---

# ⭐ Support

If you found this project useful, consider giving the repository a ⭐ on GitHub.

