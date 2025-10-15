# 🏥 7-Day AI Context & Prompt Engineering Course  
### *From Raw Data to Streamlit Dashboards in One Week*  

**Author:** Erwin Maurice McDonald  
**Audience:** Advanced healthcare software developers working with sanitizing, cleansing, and reusable materials & data environments.  

---

## 🛠️ Shields.io Badges  

![Build Status](https://img.shields.io/badge/Build-Passing-00C853?style=for-the-badge&logo=github)
![AI Ready](https://img.shields.io/badge/AI--Ready-Powered_by_ChatGPT_5-0078D4?style=for-the-badge&logo=openai)
![Dashboard Framework](https://img.shields.io/badge/Dashboard-Streamlit_|_Plotly_Studio-FF6F00?style=for-the-badge&logo=plotly)
[![License: DACR](https://img.shields.io/badge/License-DACR_(Defensive_AI_Commercial_Rights)-4C4EFF?style=for-the-badge&logo=shield&logoColor=white)](./LICENSE.md)


---

## 🎯 Course Summary  

This 7-day immersive program walks you through the **AI lifecycle from concept to dashboard**, bridging **context engineering**, **prompt strategy**, and **data visualization**.  
By the end, you’ll build a **fully functional healthcare compliance dashboard** using **RAG, Streamlit, Plotly Studio, and Mito Labs**.  

---

## 🧠 Learning Outcomes  

- Understand **Generative vs Agentic AI** and **Retrieval-Augmented Generation (RAG)**.  
- Build context-aware dashboards using **Mito Labs** and **Python triggers**.  
- Apply **AI prompting strategies** to automate healthcare data workflows.  
- Deploy an end-to-end **Streamlit dashboard** integrated with real data.  

---

## ⚙️ Tech & Tool Stack  

| Category | Tools / Frameworks | Purpose |
|-----------|-------------------|----------|
| **AI & Context** | ChatGPT-5, Claude, LangChain | Generative vs Agentic AI & RAG |
| **Versioning** | GitHub, Markdown, Shields.io | Documentation & repo setup |
| **Data Layer** | Python, Pandas, Mito Labs | Cleansing, transformation |
| **Visualization** | Plotly Studio, Base44 | Data visualization & embedding |
| **Dashboarding** | Streamlit, Mito App Mode | Interactive front-end |
| **Hosting** | GitHub Pages / Base44 | Deployment & collaboration |

---

## 📂 Folder Structure  

```plaintext
7Day-AI-Dashboard-Course/
│
├── README.md
│
├── Day1_Generative_vs_Agentic_AI.md
├── Day2_GitHub_README_Badges.md
├── Day3_Mito_Jupyter_Setup.md
├── Day4_Python_Triggers.md
├── Day5_Advanced_Prompts.md
├── Day6_Plotly_Streamlit.md
├── Day7_Final_Dashboard.md
│
└── data/
    └── TG_Product_Activity_2025.xlsx
````

---

## 🧩 Mermaid Workflow Diagram

```mermaid
flowchart TD
  %% Advanced pipeline with subgraphs, decisions, labels, styling

  %% -------- Data Sources --------
  subgraph S0["Data Sources"]
    A1["Raw Sanitization Logs (XLSX / CSV)"];
    A2["Inventory and Supply Systems (API)"];
    A3["EHR Exports (PHI / PII)"];
    A4["Policy & SOP Docs (PDF / HTML)"];
  end

  %% -------- Ingestion & Cleaning --------
  subgraph S1["Ingestion & Cleaning — Mito + Jupyter"]
    B1["Mount Workspace (JupyterLab / VS Code)"];
    B2["Interactive Cleaning (Mito Labs)"];
    B3["Validation and De-dup"];
    B4["Save Clean Tables (Parquet / CSV)"];
  end

  %% -------- Trigger Logic --------
  subgraph S2["Trigger Logic — Python + Pandas"]
    C1["Anomaly Flags (Nulls, Duplicates, Outliers)"];
    C2["Business Rules (Units, Thresholds)"];
    C3["Feature Build (Dept, Shift, MaterialType)"];
  end

  %% -------- AI Context Layer --------
  subgraph S3["AI Context Layer — RAG + Prompt Engineering"]
    D1["Chunk and Embed SOPs (Vector Store)"];
    D2{"PII / PHI Present?"};
    D3["Mask / Redact + Access Control"];
    D4["Prompt Templates (Role, Tone, Constraints)"];
    D5["Semantic Retrieval (Top-k + Filters)"];
    D6["LLM Response (Reasoning Hidden)"];
  end

  %% -------- Visualization & App --------
  subgraph S4["Visualization & App — Streamlit + Plotly"]
    E1["Curated DataFrames"];
    E2["Plotly Charts (Trends, Compliance, Top-3 Issues)"];
    E3["Streamlit Pages (KPIs, Drilldowns)"];
    E4["App Config (Secrets, RLS Proxies)"];
  end

  %% -------- Deployment & Monitoring --------
  subgraph S5["Deployment & Monitoring"]
    F1["Deploy App (Base44 / GitHub Pages + Backend)"];
    F2["Scheduled Jobs (Cron / CI) every 5 min"];
    F3["Audit Logs and Metrics (Refresh, Errors, Usage)"];
    F4["User Feedback Loop (Annotate, Flag, Correct)"];
  end

  %% Main flow
  A1 -->|ingest| B1;
  A2 -->|ingest| B1;
  A3 -->|ingest| B1;
  A4 -->|ingest| B1;
  B1 --> B2 --> B3 --> B4;
  B4 -->|read| C1 --> C2 --> C3;

  %% AI governance gate
  C3 --> D1 --> D2;
  D2 -->|Yes| D3 --> D4;
  D2 -->|No| D4;
  D4 --> D5 --> D6;

  %% App build
  D6 -->|contextual insights| E1 --> E2 --> E3 --> E4;

  %% Deploy + monitor + feedback
  E4 --> F1 --> F2 --> F3 --> F4;
  F4 -.->|label issues, add prompts| D4;
  F4 -.->|tighten rules| C2;
  F3 -.->|data quality alerts| B3;

  %% Styling (periwinkle/dark)
  classDef data fill:#1F2A44,stroke:#6C7AE0,color:#E6E8FF,stroke-width:1.2px;
  classDef clean fill:#24273A,stroke:#8FA2FF,color:#E6E8FF,stroke-width:1.2px;
  classDef logic fill:#1E2230,stroke:#A7B4FF,color:#E6E8FF,stroke-width:1.2px;
  classDef ai fill:#1B1F2C,stroke:#B7C1FF,color:#E6E8FF,stroke-width:1.2px;
  classDef app fill:#181C2A,stroke:#C6CEFF,color:#E6E8FF,stroke-width:1.2px;
  classDef deploy fill:#141826,stroke:#D8D8F6,color:#E6E8FF,stroke-width:1.2px;
  classDef decision fill:#2A2F45,stroke:#FFD166,color:#FFF5CC,stroke-width:1.4px;

  class A1,A2,A3,A4 data;
  class B1,B2,B3,B4 clean;
  class C1,C2,C3 logic;
  class D1,D3,D4,D5,D6 ai;
  class E1,E2,E3,E4 app;
  class F1,F2,F3,F4 deploy;
  class D2 decision;


```

---

## 🚀 How to Use This Repo

1. Clone this repository:

   ```bash
   git clone https://github.com/yourusername/7Day-AI-Dashboard-Course.git
   cd 7Day-AI-Dashboard-Course
   ```
2. Open each Day’s `.md` file sequentially (Day 1–7).
3. Follow code examples, prompts, and exercises.
4. Use `data/TG_Product_Activity_2025.xlsx` as your working dataset.
5. Deploy your final dashboard with:

   ```bash
   streamlit run app.py
   ```

---

## 💡 Next Steps

Each lesson file (Day 1–7) will include:

* Learning objectives
* AI prompts
* Python code examples
* Visual diagrams (Plotly/Mermaid)
* Deliverable checklists

---

### ✍️ Author

**Erwin Maurice McDonald**
*AI Strategist | Data Visualization Engineer | Healthcare Software Developer*


```
