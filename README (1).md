# 🤖 HireIQ — AI-Powered Resume Screening System

<div align="center">
  <p><strong>A modern, intelligent Applicant Tracking System (ATS) powered by Semantic AI and Natural Language Processing.</strong></p>
</div>

---

## 📖 1. Project Overview

**HireIQ** is a scalable, AI-driven Applicant Tracking System (ATS) designed to eliminate the manual fatigue of resume screening. Rather than relying on rigid, primitive keyword matching that often screens out highly qualified candidates, HireIQ leverages **Transformer-based Large Language Models (LLMs)** and **Natural Language Processing (NLP)**. 

The system extracts deep semantic context from candidate resumes and explicitly calculates their "Fit Score" based on role-specific requirements, semantic overlapping, and experience density.

### ✨ Key Capabilities
* **Intelligent File Parsing:** Lossless text extraction from complex `.pdf`, `.docx`, and `.txt` files using structural parsing limits.
* **Semantic Context Matching:** Computes resume-to-job similarity using `sentence-transformers` and Cosine Similarity matrices.
* **Dynamic Ranking:** Weights candidates holistically based on semantic overlap (35-40%), identified technical skills (30-35%), experience matching (15%), and education.
* **Actionable Rationale:** AI generates human-readable reasoning to explain exactly *why* a candidate received their placement score.
* **Modern Premium UI:** A dynamic, Vue/React-inspired vanilla DOM interface featuring interactive Radar charts, Data metrics, and a fully persistent Light/Dark aesthetic.

---

## 🚀 2. Quick Start Guide (One-Click Setup)

To make testing and running this project as easy as possible across different operating systems, we have included automated setup scripts. You only need **Python 3.8+** installed.

### 🪟 For Windows Users:
1. Navigate into the `resume_screener/` folder.
2. Double click the **`start-windows.bat`** file.
3. *That's it!* The script will automatically create a Python virtual environment, safely install all machine learning dependencies, and launch the WSGI server. 
4. Click the link provided in the terminal: `http://localhost:5000`

### 🍎/🐧 For Mac & Linux Users:
1. Open your terminal and navigate to the `resume_screener/` directory.
2. Make the script executable (if it isn't already): `chmod +x start-mac-linux.sh`
3. Run the script: `./start-mac-linux.sh`
4. The server will boot dynamically. Open `http://localhost:5000` in your browser.

### 🔐 Admin Credentials
When you access the application, you will be prompted to log in. Please use the following default credentials:
* **Username:** `admin`
* **Password:** `admin`

---

## 🏗️ 3. Technology & Architecture Stack

The project operates on a tightly-coupled monolithic architecture leveraging a robust Python backend and a lightweight, extremely responsive client interface.

### ⚙️ Backend Module
* **Framework:** Python / Flask (RESTful API architecture)
* **Datastore:** SQLite with `Flask-SQLAlchemy` ORM (Easily extendable to PostgreSQL)
* **Machine Learning & NLP Operations:**
  * `sentence-transformers`: Drives the `all-mpnet-base-v2` dense embedding mathematical mapping.
  * `PyTorch`: Handles high-dimensional tensor matrices for calculating Cosine Similarity.
  * `spacy`: Specialized NLP processing for Named Entity Recognition (NER).
* **Document Parsing Engine:** `pdfplumber` & `python-docx`
* **Data Pipelines:** `pandas` and `numpy` for statistical processing.

### 🎨 Frontend Interface
* **Structure:** HTML5, Jinja2 Templates, Semantic DOM elements.
* **Aesthetics:** Vanilla CSS (CSS3 scoped CSS Variables, Flexbox/Grid), Glassmorphism, dynamically injected variables.
* **Theming Engine:** Integrated, persistent **Light Mode / Dark Mode** toggling via `localStorage`.
* **Interactivity:** Vanilla modular JavaScript (`app.js`) handling asynchronous REST `fetch()` calls for zero-reload operations.
* **Data Visualization:** `Chart.js` with responsive theming hooks to auto-update Colors and Grid Lines instantly on theme change.

---

## 🧠 4. Core System Workflow (How It Works)

**1. Data Ingestion & Parsing (`text_utils.py` & `file_handler.py`)**
As HR uploads bulk resumes, the system cryptographically hashes files to prevent collisions and routes them to isolated local storage. `pdfplumber` extracts unstructured binary tokens into raw readable strings. System Regex and NER extract Candidate Names, Emails, Phone Numbers, and statistically compute explicitly cited experience dates.

**2. Semantic Embedding (`similarity_engine.py`)**
HireIQ employs `all-mpnet-base-v2`, mapping sentences to a 768-dimensional dense vector space. The candidate's extracted text is fed into the LLM alongside the target Job Description, allowing the system to understand that a "React Developer" fits a "UI Engineer" role without requiring explicit matching keywords.

**3. Weighted AI Ranking (`ranker.py`)**
The raw tensor Similarity Score is merged onto a weighted metric matrix consisting of:
* Semantic Alignment
* Skill Intersections 
* Experience Bounds
* Education Status 

The backend injects the calculated rationale and score into the SQLite Database as a `ScreeningResult`.

---

## 📊 5. Detailed End-to-End Structure

```text
resume_screener/
│
├── app/
│   ├── routes/              # Flask Blueprint API Endpoints
│   │   ├── candidates.py    # Document streaming, parsing triggers, API fetch
│   │   ├── dashboard.py     # Aggregated analytics and reporting
│   │   ├── jobs.py          # CRUD endpoints for Job posting tracking 
│   │   └── screening.py     # AI Batch-evaluation pipeline trigger
│   │
│   ├── services/            # Core Intelligence Layer
│   │   ├── ranker.py            # Weight distributions & Fit Scores math
│   │   ├── similarity_engine.py # S-BERT Machine Learning PyTorch backend 
│   │   └── summarizer.py        # Explainable AI rationale generator
│   │
│   ├── utils/               # Secure Handlers
│   │   ├── file_handler.py  # Validation & binary extraction limits
│   │   └── text_utils.py    # NER, complex Regex cleaning, Metadata mapping
│   │
│   ├── static/              # Compiled logic for the Browser
│   │   ├── css/style.css    # Premium application dashboard stylesheets
│   │   └── js/app.js        # Asynchronous fetch engine locking UI state
│   │
│   ├── templates/
│   │   └── index.html       # The single-page application structure
│   │
│   ├── __init__.py          # Flask Application Factory & CORS definitions
│   ├── config.py            # Environment configurations & threshold triggers
│   ├── database.py          # SQLAlchemy binding
│   └── models.py            # ORM Schema (Candidates, Jobs, ScreeningResults)
│
├── tests/                   # Native Pytest Integration test suite
│   ├── conftest.py          # Fixtures for app and in-memory test database
│   └── test_api.py          # Endpoint regression tests
│
├── uploads/                 # Web-isolated physical binary storage 
├── instance/                # Isolated SQLite database cluster location
├── run.py                   # The WSGI local development startup script
├── requirements.txt         # Verified Python dependencies list
└── README.md                # Project documentation
```

---

## 🧪 6. Running Application Tests

HireIQ includes an integration test suite built with `pytest` that uses an in-memory database (`sqlite:///:memory:`). This means you can run comprehensive integration tests on the APIs **without overwriting or modifying your local production database**.

**To run the test suite:**
1. Ensure your virtual environment is active.
2. Run the command:
   ```bash
   pytest tests/ -v
   ```
This will automatically execute the fixture setup, run health checks against the candidate and job pipelines, and tear down the memory database upon success.

---

## ✉️ 7. Email Notification Service

* **Provider:** SMTP-based notification system (`smtplib`).
* **Trigger:** Automated "Shortlist" emails are dynamically dispatched to candidates the moment they are moved to the `shortlisted` status on the dashboard.

To utilize this, update your `resume_screener/.env` file:
```bash
MAIL_SERVER=smtp.gmail.com
MAIL_PORT=587
MAIL_USE_TLS=true
MAIL_USERNAME=your-email@gmail.com
MAIL_PASSWORD=your-app-password
MAIL_DEFAULT_SENDER=hireiq-no-reply@yourdomain.com
```
*(If using Gmail, utilize an "App Password" rather than your core Google account password).*

---

## 🗺️ 8. Future Expansion Roadmap

HireIQ's blueprint has been mapped for scalable enterprise deployment:
1. **Vertical Vector scaling (`pgvector`)**: Transitioning SQLite to PostgreSQL with `pgvector` will enable instantaneous native database-level neural matching across millions of parsed candidates.
2. **Background Inference Workers**: Migrating the heavy `sentence-transformers` inference math from synchronous request handlers directly into a `Celery/Redis` messaging queue to handle massive multi-tenant load.
3. **Generative Summarization**: Pointing `summarizer.py` endpoints to closed/open Generative LLMs (e.g., Azure OpenAI GPT-4o, Llama 3) to create deeply nuanced and narrative-driven evaluations rather than deterministic string formatting.
