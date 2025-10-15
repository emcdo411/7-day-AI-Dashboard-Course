# 📊 DAY 6 — Plotly Studio + Base44 Integration

---

## 🧭 Overview

Welcome to **Day 6**, where you connect everything you’ve learned so far — cleaning, triggers, and AI-generated code — into a **live dashboard**.  
This lesson bridges **data engineering** and **data storytelling**, teaching you how to deploy your work interactively using **Plotly Studio**, **Streamlit**, and **Base44**.

By the end of this session, you’ll turn your static scripts into a functional prototype that updates in real time, ready to present or host online.

---

## 🧠 Learning Objectives

By the end of Day 6, you’ll be able to:

- Transform your **Mito Labs logic** into a fully interactive Streamlit app.  
- Use **Plotly Studio** to design dynamic, PowerBI-style visualizations.  
- Deploy dashboards using **Base44** or similar lightweight hosting options.  
- Understand how data flows from **Python logic → visualization → web app → deployment**.  

---

## 🧩 Key Concepts

| Concept | Description | Example |
|----------|-------------|----------|
| **Mito to Streamlit Export** | Converts your Jupyter work into an app-ready `.py` file. | `mito export streamlit app.py` |
| **Plotly Studio** | No-code tool to enhance, colorize, and animate charts. | Drag & drop data, then export to Python or Dash. |
| **Base44 Hosting** | Simplified platform for hosting Streamlit/Plotly dashboards. | Share live apps without manual servers. |
| **Interactive Dashboarding** | Blends AI-generated logic with manual design refinements. | Toggle filters, hover for tooltips, dynamic scaling. |

---

## 🧱 Mito → Streamlit Workflow

Your goal today is to take your cleaned and AI-visualized dataset from Day 5 and make it **clickable, filterable, and shareable**.

### Step 1 — Export from Mito
After cleaning your data in Mito Labs:

```bash
mito export streamlit app.py
````

This generates a **Streamlit-ready Python file** (`app.py`) containing your logic, transformations, and visuals.

---

### Step 2 — Integrate Plotly

In your exported file, you’ll see placeholders for visualizations.
Replace them with interactive Plotly components:

```python
import streamlit as st
import plotly.express as px
import pandas as pd

# Load the cleaned data
cleaned = pd.read_excel("cleaned_output.xlsx")

# Build an interactive chart
fig = px.bar(
    cleaned,
    x="Department",
    y="Usage_Count",
    color="MaterialType",
    title="Sanitizer Usage by Department",
    text_auto=True,
    color_discrete_sequence=px.colors.sequential.Purp
)

# Streamlit UI
st.title("🧴 Infection Control Dashboard")
st.plotly_chart(fig, use_container_width=True)
```

---

### Step 3 — Add Interactivity

Streamlit supports live controls using widgets.

```python
department_filter = st.selectbox(
    "Select Department:",
    options=cleaned["Department"].unique()
)

filtered = cleaned[cleaned["Department"] == department_filter]
st.dataframe(filtered)

st.write(f"Displaying sanitizer usage for {department_filter}")
```

This lets users dynamically explore departments in your dataset — no code editing required.

---

### Step 4 — Save & Run Locally

Run your Streamlit app:

```bash
streamlit run app.py
```

You should see:

```
Local URL: http://localhost:8501
Network URL: http://192.168.1.x:8501
```

Your interactive dashboard is now live on your machine. 🎉

---

## 💡 Enhancing with Plotly Studio

### Why Use Plotly Studio?

Plotly Studio allows you to:

* Visually edit chart designs.
* Adjust color palettes, labels, and hover info.
* Export your visual logic to `.py` for production.

### Example Workflow

1. Go to **[https://chart-studio.plotly.com](https://chart-studio.plotly.com)**
2. Upload your CSV or connect via API.
3. Design your chart visually.
4. Export → *“Download as Python code.”*
5. Paste that code into your `app.py`.

This makes it easy to apply professional-grade design to AI-generated data.

---

## 🚀 Step 5 — Deploy to Base44 (or Alternative)

When your dashboard is polished, deploy it using **Base44**, a streamlined host for Streamlit apps.

### Base44 Deployment Steps

1. Sign in at [Base44.io](https://base44.io).
2. Connect your GitHub account or upload the project ZIP.
3. Set your main file to `app.py`.
4. Choose the environment: `Python 3.10 + Streamlit`.
5. Click **Deploy**.

In a few minutes, you’ll get a live URL you can share.

---

## ⚙️ Example Folder Structure

```
7Day-AI-Dashboard-Course/
├── data/
│   └── cleaned_output.xlsx
├── notebooks/
│   └── Day6_Dashboard_Prototype.ipynb
├── app/
│   ├── app.py
│   └── requirements.txt
├── static/
│   └── styles.css
└── README.md
```

---

## 🧩 Mini-Project: Build & Host Your Dashboard

### 🎯 Objective

Turn your infection-control dataset into a live hosted dashboard.

### 🧰 Requirements

1. At least one **interactive chart** (bar, line, or scatter).
2. A **filter or dropdown** for departments or materials.
3. A **summary metric** (e.g., total sanitizer usage).
4. A **Base44 link** or **screenshot** of your hosted app.

---

### 🧪 Bonus Challenge: Thematic Dashboard

Apply a **PowerBI-inspired theme**:

* Dark background
* White grid lines
* Periwinkle color scale
* Custom title fonts (Roboto/Inter)

Example theme snippet:

```python
fig.update_layout(
    plot_bgcolor="#0e1117",
    paper_bgcolor="#0e1117",
    font=dict(family="Roboto", color="#E5E7EB"),
    title_font=dict(size=22, color="#A78BFA")
)
```

---

## 💬 Reflection

Ask yourself:

* How does interactivity change the story your data tells?
* What features would make this dashboard client-ready?
* How might AI improve visual design or automate updates in the future?

---

## ✅ Summary

| Skill               | Description                                       |
| ------------------- | ------------------------------------------------- |
| **Plotly Studio**   | Create, style, and export visualizations visually |
| **Streamlit**       | Build live interactive dashboards from Python     |
| **Mito Export**     | Transform notebooks into apps automatically       |
| **Base44**          | Host dashboards online with minimal setup         |
| **Design Thinking** | Apply PowerBI-quality polish to AI-generated data |

---

## 🧠 Next Lesson

Tomorrow, on **Day 7**, you’ll combine everything you’ve built into a **complete end-to-end dashboard**.
You’ll simulate real healthcare data workflows — from raw files to a hosted, AI-augmented dashboard.

---

### ✍️ Authored by

**Erwin Maurice McDonald**
*AI Strategist | Data Visualization Engineer | Healthcare Software Developer*

```

---
