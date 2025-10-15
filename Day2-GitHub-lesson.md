# 💻 DAY 2 — Understanding GitHub Repositories & GitHub Desktop

---

## 🧭 Learning Objectives

By the end of this lesson, you will be able to:
- Explain what a **repository** is and why it’s essential for AI, data, and dashboard projects.
- Create a new **GitHub repository** from scratch (public or private).
- **Clone** any public repository to your computer.
- Understand how **GitHub Desktop** simplifies version control — no terminal required.

---

## 🧩 1. What is a Repository?

A **repository** (often called a *repo*) is like a digital project folder stored in the cloud.  
It contains everything your project needs — code, documentation, images, and notebooks — and it tracks every change over time.

Think of it like:
- 📁 *A versioned filing cabinet* for your project.
- 🧠 *A time machine* that lets you go back to any version.
- 🤝 *A collaboration hub* where multiple people can safely work on the same files.

**Example:**  
Your `7Day-AI-Dashboard-Course` repo might include:
```

7Day-AI-Dashboard-Course/
├── README.md
├── Day1_Generative_vs_Agentic_AI.md
├── Day2_GitHub_Repositories_and_Desktop.md
└── assets/
└── screenshots/

````

---

## 🛠️ 2. Creating a New Repository (Step-by-Step)

### Option A — From the GitHub Website

1. Go to **[https://github.com/new](https://github.com/new)**.  
2. Enter a **Repository Name** (e.g., `ai-dashboard-project`).
3. Add a short **description**.
4. Choose:
   - **Public** → anyone can see your code.
   - **Private** → only invited collaborators can view.
5. ✅ Check **“Add a README file”** (this creates your first file).
6. Click **Create repository**.

You now have an online home for your project.

---

### Option B — From GitHub Desktop

GitHub Desktop allows you to create a repo locally and publish it later.

1. Open **GitHub Desktop**.  
2. Select **File → New Repository**.  
3. Give it a **name**, e.g., `ai-dashboard-course`.  
4. Choose where to save it on your computer.  
5. Check “Initialize this repository with a README.”  
6. Click **Create Repository**.

You can now edit your files locally, and when ready, click **Publish Repository** to sync with GitHub.com.

---

## 🔄 3. Cloning an Existing Repository

**Cloning** means creating a *local copy* of a repo on your computer.

### A. Using GitHub.com
1. Go to any **public repository**, for example:
   - [https://github.com/plotly/dash-sample-apps](https://github.com/plotly/dash-sample-apps)
2. Click the **<> Code** button.
3. Copy the **HTTPS link** (e.g., `https://github.com/plotly/dash-sample-apps.git`).

---

### B. Using GitHub Desktop
1. Open GitHub Desktop.
2. Select **File → Clone Repository**.
3. Paste the link you copied.
4. Choose a local folder to store the project.
5. Click **Clone**.

Once cloned, you can open it in **VS Code**, **RStudio**, or **Jupyter** — it’s now yours to explore.

---

## 🧭 4. Why GitHub Desktop Matters

If you prefer *point-and-click over terminal commands*, **GitHub Desktop** is your best friend.

It simplifies:
- **Commiting** = saving your changes with a short message.
- **Pushing** = sending your changes to GitHub.com.
- **Pulling** = downloading the latest changes from teammates.
- **Branching** = experimenting safely without breaking your main code.

**Visual overview:**
```mermaid
flowchart LR
  A[Local Project Folder] --> B[GitHub Desktop: Commit Changes]
  B --> C[Push to GitHub.com]
  C --> D[Teammates Pull Updates]
  D --> A
````

---

## 🎯 5. Pro Tips for AI + Dashboard Projects

* **Always include a README.md** — it’s your project’s first impression.
* **Use `.gitignore`** to skip logs, cache, or sensitive files.
* **Branch for experiments** — e.g., `dashboard-ui-upgrade` or `data-cleaning-test`.
* **Commit often** with clear messages like “Added Streamlit chart filter” or “Cleaned Mito data step.”

---

## 🧩 6. Mini Practice Challenge

1. Create a new GitHub repo named `my-dashboard-demo`.
2. Clone it using **GitHub Desktop**.
3. Add a `README.md` and write:

   ```
   # My Dashboard Demo
   This repo contains my first AI-driven dashboard experiment.
   ```
4. Commit and push your changes to GitHub.
5. Verify it appears online.

---

## 🧱 7. Folder Structure Example (for your course repo)

```
7Day-AI-Dashboard-Course/
├── README.md
├── Day1_Generative_vs_Agentic_AI.md
├── Day2_GitHub_Repositories_and_Desktop.md
├── Day3_Mito_Jupyter_Setup.md
├── data/
│   └── TG_Product_Activity_2025.xlsx
└── assets/
    └── diagrams/
```

---

## 🏷️ 8. Recommended Shields.io Badges

Add these to your README to make your repo look professional:

```markdown
![Build](https://img.shields.io/badge/build-passing-brightgreen)
![AI Ready](https://img.shields.io/badge/AI-Ready-blue)
![License](https://img.shields.io/badge/license-MIT-yellow)
![Dashboard](https://img.shields.io/badge/dashboard-live-success)
```

---

## ✅ Summary

| Concept            | Description                                           |
| ------------------ | ----------------------------------------------------- |
| **Repository**     | The main project folder that tracks all versions      |
| **Clone**          | Copy a remote repo to your local computer             |
| **Commit**         | Save local changes with a message                     |
| **Push**           | Upload local changes to GitHub                        |
| **GitHub Desktop** | GUI tool that makes version control beginner-friendly |

---

## 🧠 Next Lesson

Now that you’ve mastered repositories and version control,
you’re ready for **Day 3 — Setting Up Mito Labs & Jupyter** to start cleaning and visualizing data interactively.

---

### ✍️ Authored by

**Erwin Maurice McDonald**
*AI Strategist | Data Visualization Engineer | Healthcare Software Developer*

```
