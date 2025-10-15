# ⚙️ DAY 4 — Writing Trigger Python Code

---

## 🧭 Overview

Welcome to **Day 4**, where we shift from *clicking through data in Mito* to *automating those steps using Python trigger cells*.  
You’ll learn how to write small, intelligent scripts that clean, summarize, and monitor your data automatically — the foundation of AI-ready analytics.

---

## 🧠 Learning Objectives

By the end of this lesson, you will be able to:

- Write **Python scripts** that detect and clean common data issues.  
- Build **trigger cells** that automatically run cleaning or anomaly detection steps.  
- Understand how to **chain trigger logic** to build workflows that update dashboards in real time.  
- Document and reuse your code for multiple datasets in a reproducible way.

---

## 💡 Why This Matters

Think of trigger cells as the **“heartbeats”** of your data pipeline.

In a healthcare environment, you can’t afford manual delays — sanitation logs, supply usage, or infection tracking all need to refresh automatically.  
Trigger logic makes this happen: once written, it quietly runs every time your dataset updates.

---

## 🧩 Key Concepts

| Concept | Description | Example |
|----------|-------------|----------|
| **Trigger Cell** | A Jupyter cell that runs automatically to clean or transform data. | Auto-remove duplicates or outliers on open. |
| **Reusable Script** | Code that adapts to any dataset without rewriting. | Load any `.xlsx` file and apply the same cleaning rules. |
| **Prompt-Driven Logic** | When an AI-generated script executes based on a user prompt. | “Clean nulls, summarize by department, and show anomalies.” |

---

## 🧪 Example: Auto-Clean and Detect Anomalies

Below is your first “trigger cell.”  
It cleans a dataset and prints a short diagnostic summary.

```python
# Trigger: auto-clean data and detect anomalies

import pandas as pd

# Step 1: Load dataset (replace with your file path)
df = pd.read_excel('TG Product Activity 2025-10-02 09_38_41.xlsx')

# Step 2: Clean nulls and duplicates
cleaned = df.dropna().drop_duplicates()

# Step 3: Generate a basic summary
summary = cleaned.describe()

# Step 4: Report anomalies
print("Anomalies detected:", len(df) - len(cleaned))
print("\nSummary:\n", summary)
````

### 🩺 Interpreting Output

* **Anomalies detected:** number of rows dropped due to missing or duplicate data.
* **Summary table:** quick stats (mean, min, max) for each numeric column.

Each time you run this cell, the logic automatically “self-checks” your data integrity.

---

## 🔁 Creating Reusable Triggers

A trigger script should be flexible enough to handle multiple files without rewriting.
Here’s a reusable version using variables and reusable logic.

```python
import pandas as pd, os

def trigger_autoclean(filepath):
    if not os.path.exists(filepath):
        raise FileNotFoundError(f"{filepath} not found.")
    df = pd.read_excel(filepath)
    original_rows = len(df)
    cleaned = df.dropna().drop_duplicates()
    anomalies = original_rows - len(cleaned)
    summary = cleaned.describe()
    print(f"\n✅ Clean complete: {anomalies} anomalies removed.")
    return cleaned, summary

# Example use
data, stats = trigger_autoclean("TG Product Activity 2025-10-02 09_38_41.xlsx")
```

### 📦 Why it’s powerful

* Works on any file.
* Can plug into your **Mito** or **Streamlit** app.
* Schedulable (cron, Airflow, Windows Task Scheduler).

---

## 🧬 Advanced: Add AI-Driven Triggers

```python
# Trigger: AI-driven cleaning logic

from openai import OpenAI
import pandas as pd

client = OpenAI(api_key="YOUR_API_KEY")

df = pd.read_excel("sanitization_data.xlsx")
prompt = f"Analyze the following dataframe columns: {', '.join(df.columns)}. Identify any anomalies or patterns worth noting."

response = client.chat.completions.create(
    model="gpt-4o-mini",
    messages=[{"role": "user", "content": prompt}]
)

print("AI Analysis:")
print(response.choices[0].message.content)
```

Creates a **“smart trigger”** — it doesn’t just clean data, it *thinks* about what it cleaned.

---

## 🧰 Best Practices

1. **Keep triggers modular** — one clear purpose per cell.
2. **Add docstrings** so you know what each trigger does later.
3. **Chain triggers** — cleaning → transformation → summary → export.
4. **Version control** your triggers in GitHub (`/scripts/triggers/`).
5. **Use consistent naming:**

   * `trigger_clean.py`
   * `trigger_summary.py`
   * `trigger_alert.py`

---

## 🧱 Example Folder Structure

```
7Day-AI-Dashboard-Course/
├── data/
│   └── TG_Product_Activity_2025.xlsx
├── scripts/
│   ├── trigger_clean.py
│   ├── trigger_summary.py
│   └── trigger_alert.py
└── notebooks/
    └── Data_Cleaning_Triggers.ipynb
```

---

## 🧩 Optional Challenge

**Task:**
Create a new trigger cell that:

1. Detects any `usage_count` above 250 (potential logging error).
2. Replaces missing department values with `"Unknown"`.
3. Exports the cleaned dataset as `sanitized_output.xlsx`.

Expected console output:

```
🚨 3 rows exceeded threshold
✅ Missing department values replaced with 'Unknown'
💾 File saved to sanitized_output.xlsx
```

---

## 🧪 Mini-Project: Trigger Simulation

Let’s simulate what happens when your trigger runs in production.

### 🎯 Objective

Build and execute a mini “trigger pipeline” that:

* Cleans mock hospital supply data.
* Detects anomalies.
* Generates a log summary.

### 🧰 Step 1 — Create a Sample Dataset

```python
import pandas as pd
import numpy as np

# Create sample data
np.random.seed(42)
df = pd.DataFrame({
    "Department": np.random.choice(["ICU", "ER", "Surgery", "Lab", None], 20),
    "Usage_Count": np.random.randint(10, 300, 20),
    "Sanitizer_Batch": np.random.choice(["BatchA", "BatchB", "BatchC"], 20)
})

# Save sample dataset
df.to_excel("simulated_sanitizer_log.xlsx", index=False)
print("💾 Sample data created: simulated_sanitizer_log.xlsx")
```

### 🧮 Step 2 — Run the Trigger Script

```python
def run_trigger(filepath):
    df = pd.read_excel(filepath)
    high_usage = df[df["Usage_Count"] > 250]
    df["Department"] = df["Department"].fillna("Unknown")
    cleaned = df.drop_duplicates()
    cleaned.to_excel("cleaned_output.xlsx", index=False)
    
    print("=== Trigger Simulation Report ===")
    print(f"Rows cleaned: {len(df) - len(cleaned)}")
    print(f"High-usage alerts: {len(high_usage)}")
    print("Cleaned data saved as 'cleaned_output.xlsx'")

run_trigger("simulated_sanitizer_log.xlsx")
```

### 📊 Step 3 — Inspect Output

* Open `cleaned_output.xlsx` in Excel or Mito.
* Confirm that:

  * No missing departments remain.
  * Duplicate rows were dropped.
  * Any count > 250 was logged.

### 🧠 Reflection

What patterns did your trigger catch?
How could you add visualization or alerts (e.g., email, dashboard updates) to this pipeline?

---

## ✅ Summary

| Skill                    | Description                                       |
| ------------------------ | ------------------------------------------------- |
| **Trigger cells**        | Automate repetitive notebook actions              |
| **Auto-cleaning**        | Drop duplicates/nulls instantly                   |
| **Reusable scripts**     | Handle any file                                   |
| **AI-assisted triggers** | Add intelligence and explanations                 |
| **Simulation**           | Validate and visualize workflow before deployment |

---

## 🧠 Next Lesson

Tomorrow (Day 5), you’ll take your triggers further — using **advanced AI prompts** to transform, summarize, and visualize data automatically.

You’ll learn to build *context-aware prompts* that adapt to your data in real time.

---

### ✍️ Authored by

**Erwin Maurice McDonald**
*AI Strategist | Data Visualization Engineer | Healthcare Software Developer*

```

---
