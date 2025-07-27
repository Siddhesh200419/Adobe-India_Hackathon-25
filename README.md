# 🚀 Adobe Hackathon Round 1B: Persona-Driven Document Intelligence

## 🧠 Overview

This project implements a **persona-driven document intelligence system** that intelligently extracts and ranks relevant content from a set of 3–10 related PDF documents. By understanding a **persona** and their **job-to-be-done**, the system surfaces the most semantically aligned sections and outputs a structured, easy-to-consume JSON format.

> 📄 Input: 3–10 PDF files
> 🧍 Persona: Domain-specific role (e.g., Research Analyst)
> ✅ Output: Structured, ranked JSON of relevant sections

---

## ✨ Key Features

* 🔍 **Semantic Relevance Scoring**: Uses sentence embeddings to rank content based on persona relevance.
* 🧾 **Section & Subsection Detection**: Extracts document structure using font heuristics and layout analysis.
* ⚙️ **Fast & Lightweight**: Completes processing under 60 seconds (3–5 PDFs) using a <100MB model.
* 🔐 **Fully Offline**: Requires no internet access, ensuring compliance with air-gapped environments.
* 📦 **Dockerized for Portability**: One-command setup and execution via Docker.

---

## 🧪 Technical Architecture

### 📂 Core Modules

| Component             | Description                                                       |
| --------------------- | ----------------------------------------------------------------- |
| `DocumentProcessor`   | Orchestrates the pipeline from PDF input to ranked output         |
| PyMuPDF (`fitz`)      | Extracts rich text + formatting metadata from PDFs                |
| Sentence Transformers | Computes semantic similarity using `all-MiniLM-L6-v2` (~90MB)    |
| Heuristic Detection   | Identifies sections using font size, boldness, and regex patterns |

---

### 🔁 Processing Pipeline

1. **PDF Parsing**: Extracts text, layout, and styling info
2. **Section Identification**: Detects headings based on visual cues and text patterns
3. **Content Grouping**: Bundles text under logical sections and subsections
4. **Semantic Scoring**: Measures similarity to persona and job-to-be-done using embeddings
5. **Structured Output**: Returns ranked sections in JSON with metadata

---

## 🧰 Libraries & Tools

* [`PyMuPDF`](https://pymupdf.readthedocs.io/): High-performance PDF extraction
* [`SentenceTransformers`](https://www.sbert.net/): For semantic similarity
* `NumPy`, `re`, `json`: Utilities for analysis, scoring, and output

---

## 🚀 How to Run

### 🧾 Prerequisites

* Docker installed (`https://www.docker.com`)
* Linux/AMD64 architecture support

### 🏗️ Build the Docker Image

```bash
docker build --platform linux/amd64 -t adobe-doc-intel:latest .
```

### 📂 Prepare Input & Output Folders

```bash
mkdir -p input output
# Add 3–10 related PDFs to the 'input' folder
```

### ▶️ Run the Pipeline

```bash
docker run --rm \
  -v $(pwd)/input:/app/input \
  -v $(pwd)/output:/app/output \
  --network none \
  adobe-doc-intel:latest
```

### 📤 Output Format

`output/output.json` includes:

* ✅ `metadata`: Files processed, persona, job description, timestamp
* 🔝 `sections`: Top 20 most relevant sections
* 🔍 `subsections`: Top 30 most relevant subsections

---

## 📋 Sample Configuration (`config.json`)

```json
{
  "persona": "Research Analyst",
  "job_to_be_done": "Analyze key findings and methodologies from research documents"
}
```

Update this file to match different personas or tasks.

---

## ✅ Hackathon Constraints Checklist

| Constraint               | Status         |
| ------------------------ | -------------- |
| CPU-only                 | ✅ Compliant    |
| Model size ≤ 1GB         | ✅ 90MB         |
| No internet access       | ✅ Offline mode |
| Process ≤ 60s (3–5 PDFs) | ✅ Optimized    |
| Platform: AMD64 + Docker | ✅ Compatible   |

---

## 🧪 Testing

### 📌 Run Tests

```bash
python run_tests.py
```

### 🔍 Validations

* ✅ Model size, offline compliance
* ✅ Document parsing and section detection
* ✅ Semantic ranking logic
* ✅ Sample JSON generation

---

## 🗂️ File Structure

```
.
├── main.py                 # Entry point for processing
├── config.json             # Persona & task configuration
├── requirements.txt        # Python dependencies
├── Dockerfile              # Docker setup
├── test_solution.py        # Unit & functional tests
├── run_tests.py            # Test runner
├── approach_explanation.md # Deep-dive on methodology
└── README.md               # Project documentation (this file)
```

---

## 🧾 Supported Document Types

* Research papers
* Technical reports
* Academic textbooks
* Financial or policy documents
* Instruction manuals

---

## 🚨 Troubleshooting

| Issue                     | Fix                                                        |
| ------------------------- | ---------------------------------------------------------- |
| 🗂️ Empty `input/` folder | Add 3–10 valid PDF files                                   |
| ❌ PDFs not processed      | Ensure they're text-based (not image-only)                 |
| 🐳 Docker issues          | Confirm resources are allocated and AMD64 platform is used |
| ⚠️ No output.json         | Check logs and validate `config.json` syntax               |

---

## 🏁 Conclusion

This solution demonstrates how persona-aligned insights can be efficiently extracted from complex documents. Built with performance and portability in mind, it's tailored for Adobe's evaluation framework — fast, offline, and intelligent.

---

