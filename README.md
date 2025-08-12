# llama-text-summarizer

A Text Summarizer AI project using LLaMA via Ollama, integrated with FastAPI for the backend, Streamlit for the frontend, and GitHub for version control and publishing.

---

## Features

- Summarize any text using LLaMA (via Ollama)
- FastAPI backend API
- Streamlit web frontend
- Easy local setup

![Frontend Screenshot](screenshot.png)

---

## Project Structure

```
llama-text-summarizer/
├── backend/
│   ├── main.py
│   └── requirements.txt
├── frontend/
│   ├── app.py
├── requirements.txt
├── README.md
```

---

## Prerequisites

- Python 3.8+
- [Ollama](https://ollama.com/) installed and running
- LLaMA model pulled via Ollama
- Git

---

## Setup Instructions

### 1. Clone the Repository

```bash
git clone https://github.com/Ichrafsassi/llama-text-summarizer.git
cd llama-text-summarizer
```

### 2. Set Up Python Virtual Environment

```bash
python3 -m venv venv
source venv/bin/activate
```

---

### 3. Start Ollama and Pull LLaMA Model

```bash
ollama serve
ollama pull llama2
```

---

### 4. Backend Setup (FastAPI)

```bash
cd backend
pip install -r requirements.txt
uvicorn main:app --reload
```

#### `backend/main.py` Example

```python
from fastapi import FastAPI, Form
import requests

app = FastAPI()

@app.post("/summarize/")
def summarize(text: str = Form(...)):
    response = requests.post(
        "http://localhost:11434/api/generate",
        json={
            "model": "llama2",
            "prompt": f"Summarize this:\n\n{text}",
            "stream": False
        }
    )
    result = response.json()
    return {"summary": result.get("response", "No summary generated.")}
```

---

### 5. Frontend Setup (Streamlit)

Open a new terminal, activate your virtual environment, then:

```bash
cd frontend
pip install -r requirements.txt
streamlit run app.py
```

#### `frontend/app.py` Example

```python
import streamlit as st
import requests

st.title("LLaMA Text Summarizer")
user_input = st.text_area("Enter your text here:")

if st.button("Summarize"):
    response = requests.post(
        "http://localhost:8000/summarize/",
        data={"text": user_input}
    )
    summary = response.json().get("summary", "Error generating summary.")
    st.subheader("Summary:")
    st.write(summary)
```

---

## Usage

1. Open [http://localhost:8501](http://localhost:8501) in your browser.
2. Enter the text you want to summarize.
3. Click **Summarize**.
4. The summary will appear below.

---


