# Ask Me Anything: Rulebook-Assistant
<ins> *by Jayvind Baghel.* </ins>

Ever wanted to build your own document-based AI assistant? In this project, I built a powerful, interactive Q&A system for the **VARCHASVA '25 Rulebook** using open-source tools like LLaMA-2, ChromaDB, and Sentence Transformers.

# 🚀 What This Project Does

📄 Loads a PDF Rulebook

✂️ Cleans and splits the content into semantic chunks

🔎 Embeds and stores those chunks in a vector database (ChromaDB)

🤖 Uses LLaMA-2 to answer user questions based on the rulebook content

💬 Provides follow-up questions to guide the conversation naturally

# 🔧 Tools Used
PyMuPDF + pdfplumber — Extract plain text and tables from PDF

Sentence Transformers — Convert text to embeddings

ChromaDB — Store and query embeddings

LangChain — Text chunking

LLaMA-2 (Hugging Face) — Generate context-aware answers

Google Colab Notebooks — No setup, run from your browser

# 📂 Step-by-Step Breakdown

1. **PDF Upload & Parsing**
   
First, we upload the Rulebook.pdf directly to the notebook (via Kaggle's sidebar) so anyone who forks it can use it instantly. We extract text using PyMuPDF, and extract structured tables using pdfplumber.

    pdf_path = "/kaggle/input/rulebook/Rulebook.pdf"

2. **Text Cleaning and Chunking**
   
The full text is cleaned and broken into manageable chunks using LangChain’s RecursiveCharacterTextSplitter.

    splitter = RecursiveCharacterTextSplitter(chunk_size=500, chunk_overlap=50)
    text_chunks = splitter.split_text(cleaned_text)

3. **Table Cleaning and Integration**
   
Tables are cleaned and converted into human-readable text chunks (like: Event: Kabaddi, Points: P1: 10, P2: 8, …, Venue: Main Ground) and merged into the main chunk list.

4. **Storing Embeddings in ChromaDB**
   
Each chunk is embedded using all-MiniLM-L6-v2 from Sentence Transformers and stored in a local ChromaDB collection.

    collection.add(ids=[str(i)], embeddings=[embedding], metadatas=[{"text": chunk}])

5. **Answering Queries Using LLaMA-2**
   
When the user asks a question, the system:

Finds the most relevant text chunks using vector similarity
Feeds the context + question to LLaMA-2 via a prompt
Returns an answer + three follow-up questions

**🎯 Sample Query**

Question:

    “How long will all the matches of Kho Kho for girls be?”

Generated Answer + Follow-ups:
```
Answer:
Each Kho Kho match for girls is scheduled to last 30 minutes, with two innings of 15 minutes each.
Follow-up Questions:
1. What are the rules for deciding the winner in Kho Kho?
2. Where will the Kho Kho matches take place?
3. How many teams are participating in Kho Kho for girls?
```
# To run it:

1. Upload the Rule book pdf in your Drive.

2. Open your Google colab and paste the code in a new notebook.

3. Provide path for the pdf.
```
# --- Set File Path ---
pdf_path = "/content/drive/Rulebook.pdf"  #Upload path if needed
```

4. Paste your Hugging Face token here
```
#--- HuggingFace Login ---
login(token="Your_Hugging_face_token")
```
5. Click Run All

Done 🎉
