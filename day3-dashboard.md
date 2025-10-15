# 📘 LESSON — R vs Python for Dashboards + Why Starter Code Matters + Shiny/RStudio Intro

*With ready-to-run example apps in **R (Shiny)** and **Python (Streamlit)**, plus an advanced AI prompt that generates simulated data you can open in **Mito Labs**.*

---

## 1) R vs Python: the practical differences (for BI teams)

| Area             | R                                                      | Python                                              |
| ---------------- | ------------------------------------------------------ | --------------------------------------------------- |
| Core strength    | Statistical analysis, reporting, reproducible research | General-purpose, ML/AI, data engineering, apps      |
| Learning curve   | Gentle for stats/plots; rich IDE (RStudio)             | Gentle for scripting; huge ecosystem                |
| Data wrangling   | `dplyr`, `data.table`, `tidyr`                         | `pandas`, `polars`, `numpy`                         |
| Viz              | `ggplot2`, `plotly`, `highcharter`                     | `plotly`, `matplotlib`, `altair`                    |
| Dashboards       | **Shiny**, `flexdashboard`, `shiny.semantic`           | **Streamlit**, **Dash**, Panel                      |
| Packaging/Deploy | RStudio Connect, shinyapps.io, plumber                 | pip/venv, FastAPI, Streamlit Cloud, Docker          |
| Where it shines  | Analysts & biostats; fast reporting in regulated orgs  | End-to-end pipelines, MLOps, API agents, full-stack |

**Rule of thumb**

* If your team already lives in **RStudio** and wants point-and-click reproducible analytics → **R + Shiny**.
* If you’ll expand into **LLMs/agents**, data APIs, and productized services → **Python + Streamlit/Dash**.

---

## 2) “Starter code” inside advanced prompts = fewer errors, faster wins

When you ask an AI to “build a dashboard,” you’ll get better results if you **seed the prompt with**:

* **Target schema** (columns, types, example rows)
* **Minimal scaffold** (imports, app skeleton, color/theme)
* **Constraints** (must handle missing data, must plot X/Y)
* **Outputs** (e.g., one `.R` and one `.py`)

**Why it works:** you reduce ambiguity → the model fills the *gaps* instead of guessing the *entire* structure. This is the difference between **prompt engineering** (“what to do”) and **context engineering** (“give the model the ingredients”).

---

## 3) Quick intro: Shinyapps.io & RStudio

* **RStudio (Posit)**: IDE for R/Python; integrated console, plots, package management, and Git.
* **Shiny**: R package to build interactive web apps (pure R).
* **shinyapps.io**: Managed hosting for Shiny—no servers to manage. `rsconnect` deploys from RStudio in a few clicks.

  * Typical flow: Test locally → `rsconnect::deployApp()` → share URL.

---

## 4) Advanced AI Prompt (drop into ChatGPT/Claude/Grok)

> **Role:** You are a senior analytics engineer and instructor.
> **Goal:** Generate **simulated healthcare sanitization data** and produce starter **dashboard code** in **R (Shiny)** and **Python (Streamlit)** that:
>
> 1. auto-loads `sanitization.csv` if present else generate a realistic demo dataset;
> 2. provides filters (date, department, material);
> 3. shows a time-series and a bar/heatmap;
> 4. handles missing values and odd date formats;
> 5. uses accessible, enterprise-neutral styling (no brand colors).
>
> **Data schema (CSV):**
> `date (YYYY-MM-DD)`, `department`, `material`, `usage_count (int)`, `compliance_rate (0-100 float)`
>
> **Starter code hints (Python):**
>
> ```python
> import os, pandas as pd, numpy as np
> from datetime import datetime
> ```
>
> **Starter code hints (R):**
>
> ```r
> library(dplyr); library(readr); library(lubridate)
> ```
>
> **Deliverables:**
>
> 1. A python cell that **creates & saves** `sanitization.csv` with ~1,000 rows (randomized but realistic).
> 2. A complete `app.R` (Shiny) and `app.py` (Streamlit) that read the CSV or simulate if missing, and render:
>
>    * filters,
>    * a time-series line of `usage_count`,
>    * a bar chart of `usage_count` by `department`,
>    * an optional heatmap `department × material` of `compliance_rate`.
> 3. Code must be robust to messy CSVs (trim, coerce, parse dates).
> 4. Include comments that show students **where** to add new metrics.
>
> **Output exactly these files as fenced code blocks:**
>
> * `mito_sim_demo.py` (CSV generator students can run in Mito)
> * `app.R` (Shiny app)
> * `app.py` (Streamlit app)

> **After generating, print a short “How to run” section.**

---

## 5) Ready-to-run examples

Below are **working** versions you can use immediately.

### A) `mito_sim_demo.py` — simulated data generator (use in Mito Labs or plain Python)

```python
# mito_sim_demo.py
# Generates sanitization.csv in the current folder with realistic demo data for dashboards / Mito Labs.

import os
import numpy as np
import pandas as pd
from datetime import datetime, timedelta

np.random.seed(42)

start = datetime(2025, 1, 1)
days = 120  # ~4 months
dates = [start + timedelta(days=i) for i in range(days)]

departments = ["ER", "ICU", "Surgery", "Pediatrics", "Radiology", "Pharmacy"]
materials = ["Wipes", "Gel", "Spray"]

rows = []
for d in dates:
    for dept in departments:
        for mat in materials:
            base = {
                "ER": 120, "ICU": 90, "Surgery": 80,
                "Pediatrics": 70, "Radiology": 60, "Pharmacy": 50
            }[dept]
            # Weekly seasonality + noise
            season = 1.0 + 0.12*np.sin(2*np.pi*(d.timetuple().tm_yday)/7.0)
            usage = max(0, np.random.normal(base*season, 10))
            compliance = np.clip(np.random.normal(88, 6), 60, 100)

            rows.append({
                "date": d.strftime("%Y-%m-%d"),
                "department": dept,
                "material": mat,
                "usage_count": int(round(usage)),
                "compliance_rate": round(float(compliance), 2)
            })

df = pd.DataFrame(rows)
df.to_csv("sanitization.csv", index=False)
print("Wrote sanitization.csv with", len(df), "rows")
print(df.head())
```

**Run:**

```bash
python mito_sim_demo.py
```

Then open **Mito** and load `sanitization.csv`.

---

### B) `app.R` — minimal Shiny dashboard (reads CSV or simulates)

```r
# app.R
# Shiny dashboard for sanitization data.
# If "sanitization.csv" exists in the working dir, loads it; otherwise simulates data.

library(shiny)
library(dplyr)
library(readr)
library(lubridate)
library(plotly)
library(tidyr)

load_or_sim <- function() {
  path <- "sanitization.csv"
  if (file.exists(path)) {
    df <- suppressWarnings(read_csv(path, show_col_types = FALSE))
  } else {
    set.seed(42)
    start <- ymd("2025-01-01")
    days <- 120
    dates <- seq.Date(start, by = "day", length.out = days)
    departments <- c("ER","ICU","Surgery","Pediatrics","Radiology","Pharmacy")
    materials <- c("Wipes","Gel","Spray")

    rows <- expand.grid(date = dates, department = departments, material = materials) |>
      mutate(
        dow = wday(date),
        base = case_when(
          department == "ER" ~ 120,
          department == "ICU" ~ 90,
          department == "Surgery" ~ 80,
          department == "Pediatrics" ~ 70,
          department == "Radiology" ~ 60,
          TRUE ~ 50
        ),
        season = 1 + 0.12*sin(2*pi*(yday(date))/7),
        usage_count = pmax(0, round(rnorm(n(), base*season, 10))),
        compliance_rate = pmin(100, pmax(60, round(rnorm(n(), 88, 6), 2)))
      ) |>
      select(date, department, material, usage_count, compliance_rate)
    df <- rows
  }

  # Normalize columns
  df <- df |>
    mutate(
      date = suppressWarnings(ymd(date)),
      department = as.character(trimws(department)),
      material = as.character(trimws(material)),
      usage_count = suppressWarnings(as.integer(usage_count)),
      compliance_rate = suppressWarnings(as.numeric(compliance_rate))
    ) |>
    filter(!is.na(date), !is.na(usage_count), !is.na(compliance_rate))
  df
}

ui <- fluidPage(
  tags$head(tags$style(HTML("
    .well { background: #f7f7f9; }
  "))),
  titlePanel("Sanitization Dashboard (Shiny)"),
  sidebarLayout(
    sidebarPanel(
      dateRangeInput("dates", "Date range",
                     start = NA, end = NA, format = "yyyy-mm-dd"),
      selectInput("dept", "Department(s)", choices = NULL, multiple = TRUE),
      selectInput("mat", "Material(s)", choices = NULL, multiple = TRUE),
      helpText("If sanitization.csv is in the working directory, it will be used automatically.")
    ),
    mainPanel(
      tabsetPanel(
        tabPanel("Time Series", plotlyOutput("ts_plot", height = "420px")),
        tabPanel("Bar by Department", plotlyOutput("bar_plot", height = "420px")),
        tabPanel("Heatmap (Dept × Material)", plotlyOutput("heat_plot", height = "420px")),
        tabPanel("Data", DT::dataTableOutput("table"))
      )
    )
  )
)

server <- function(input, output, session) {
  df <- load_or_sim()

  updateSelectInput(session, "dept", choices = sort(unique(df$department)),
                    selected = unique(df$department))
  updateSelectInput(session, "mat", choices = sort(unique(df$material)),
                    selected = unique(df$material))
  updateDateRangeInput(session, "dates",
                       start = min(df$date, na.rm = TRUE),
                       end = max(df$date, na.rm = TRUE))

  filtered <- reactive({
    out <- df
    if (!is.null(input$dates[1])) out <- out |> filter(date >= input$dates[1])
    if (!is.null(input$dates[2])) out <- out |> filter(date <= input$dates[2])
    if (length(input$dept)) out <- out |> filter(department %in% input$dept)
    if (length(input$mat)) out <- out |> filter(material %in% input$mat)
    out
  })

  output$ts_plot <- renderPlotly({
    dat <- filtered() |>
      group_by(date, department) |>
      summarise(usage = sum(usage_count), .groups = "drop")
    if (nrow(dat) == 0) return(NULL)
    plot_ly(dat, x = ~date, y = ~usage, color = ~department, type = "scatter", mode = "lines+markers") |>
      layout(yaxis = list(title = "Usage Count"), xaxis = list(title = "Date"))
  })

  output$bar_plot <- renderPlotly({
    dat <- filtered() |>
      group_by(department) |>
      summarise(total_usage = sum(usage_count), .groups = "drop") |>
      arrange(desc(total_usage))
    if (nrow(dat) == 0) return(NULL)
    plot_ly(dat, x = ~total_usage, y = ~department, type = "bar", orientation = "h") |>
      layout(xaxis = list(title = "Total Usage"), yaxis = list(title = "Department"))
  })

  output$heat_plot <- renderPlotly({
    dat <- filtered() |>
      group_by(department, material) |>
      summarise(avg_compliance = mean(compliance_rate), .groups = "drop")
    if (nrow(dat) == 0) return(NULL)
    mat <- tidyr::pivot_wider(dat, names_from = material, values_from = avg_compliance) |>
      as.data.frame()
    rownames(mat) <- mat$department
    mat$department <- NULL
    z <- as.matrix(mat)
    plot_ly(x = colnames(z), y = rownames(z), z = z, type = "heatmap",
            colorscale = "Blues", colorbar = list(title = "Compliance %"),
            zmin = 0, zmax = 100)
  })

  output$table <- DT::renderDataTable({
    DT::datatable(filtered(), options = list(pageLength = 10))
  })
}

shinyApp(ui, server)
```

**Run locally in RStudio:**

```r
# In RStudio Console:
# install.packages(c("shiny","dplyr","readr","lubridate","plotly","DT"))
shiny::runApp("app.R")
```

---

### C) `app.py` — minimal Streamlit dashboard (reads CSV or simulates)

```python
# app.py
# Streamlit dashboard for sanitization data.
# If "sanitization.csv" exists in the current folder, reads it; else simulates.

import os
import numpy as np
import pandas as pd
import plotly.express as px
import streamlit as st
from datetime import datetime, timedelta

st.set_page_config("Sanitization Dashboard (Streamlit)", layout="wide")

def load_or_sim():
    path = "sanitization.csv"
    if os.path.exists(path):
        df = pd.read_csv(path)
    else:
        np.random.seed(42)
        start = datetime(2025,1,1)
        days = 120
        dates = [start + timedelta(days=i) for i in range(days)]
        departments = ["ER","ICU","Surgery","Pediatrics","Radiology","Pharmacy"]
        materials = ["Wipes","Gel","Spray"]

        rows = []
        for d in dates:
            for dept in departments:
                for mat in materials:
                    base = {"ER":120,"ICU":90,"Surgery":80,"Pediatrics":70,"Radiology":60,"Pharmacy":50}[dept]
                    season = 1.0 + 0.12*np.sin(2*np.pi*(d.timetuple().tm_yday)/7.0)
                    usage = max(0, np.random.normal(base*season, 10))
                    compliance = np.clip(np.random.normal(88,6), 60, 100)
                    rows.append({
                        "date": d.strftime("%Y-%m-%d"),
                        "department": dept,
                        "material": mat,
                        "usage_count": int(round(usage)),
                        "compliance_rate": round(float(compliance), 2),
                    })
        df = pd.DataFrame(rows)

    # Normalize
    df["date"] = pd.to_datetime(df["date"], errors="coerce")
    df["department"] = df["department"].astype(str).str.strip()
    df["material"] = df["material"].astype(str).str.strip()
    df["usage_count"] = pd.to_numeric(df["usage_count"], errors="coerce").fillna(0).astype(int)
    df["compliance_rate"] = pd.to_numeric(df["compliance_rate"], errors="coerce")
    df = df.dropna(subset=["date","compliance_rate"])
    return df

df = load_or_sim()

st.title("Sanitization Dashboard (Streamlit)")
st.caption("Reads `sanitization.csv` if present; otherwise uses demo data. Adjust filters below.")

# Sidebar filters
min_d, max_d = df["date"].min(), df["date"].max()
date_range = st.sidebar.date_input("Date range", value=(min_d.date(), max_d.date()))
dept_sel = st.sidebar.multiselect("Department(s)", sorted(df["department"].unique()),
                                  default=list(sorted(df["department"].unique())))
mat_sel = st.sidebar.multiselect("Material(s)", sorted(df["material"].unique()),
                                 default=list(sorted(df["material"].unique())))

f = df.copy()
if isinstance(date_range, tuple) and len(date_range) == 2:
    f = f[(f["date"] >= pd.to_datetime(date_range[0])) & (f["date"] <= pd.to_datetime(date_range[1]))]
if dept_sel:
    f = f[f["department"].isin(dept_sel)]
if mat_sel:
    f = f[f["material"].isin(mat_sel)]

col1, col2, col3, col4 = st.columns(4)
col1.metric("Rows", f"{len(f):,}")
col2.metric("Avg Compliance %", f"{f['compliance_rate'].mean():.1f}" if len(f) else "—")
col3.metric("Total Usage", f"{f['usage_count'].sum():,}" if len(f) else "0")
latest = f.sort_values("date").groupby("department")["date"].max().max() if len(f) else None
col4.metric("Latest date in view", latest.date().isoformat() if latest is not None else "—")

# Charts
ts = (f.groupby(["date","department"], as_index=False)["usage_count"].sum()
        .sort_values("date"))
fig_ts = px.line(ts, x="date", y="usage_count", color="department", markers=True,
                 title="Usage Count over Time")
st.plotly_chart(fig_ts, use_container_width=True)

bar = (f.groupby("department", as_index=False)["usage_count"].sum()
         .sort_values("usage_count", ascending=False))
fig_bar = px.bar(bar, x="usage_count", y="department", orientation="h",
                 title="Total Usage by Department")
st.plotly_chart(fig_bar, use_container_width=True)

heat = (f.groupby(["department","material"], as_index=False)["compliance_rate"].mean())
if len(heat):
    pivot = heat.pivot(index="department", columns="material", values="compliance_rate").fillna(0)
    fig_hm = px.imshow(pivot, color_continuous_scale="Blues", aspect="auto",
                       title="Avg Compliance Rate (Dept × Material)",
                       labels=dict(color="Compliance %"))
    st.plotly_chart(fig_hm, use_container_width=True)
else:
    st.info("No data for heatmap with current filters.")

st.subheader("Filtered Data")
st.dataframe(f.sort_values("date", ascending=False), use_container_width=True)

st.caption("Tip: Replace the CSV with real data (same columns) to take this live.")
```

**Run:**

```bash
pip install streamlit plotly pandas numpy
streamlit run app.py
```

---

## 6) “How to use this lesson”

1. Run `mito_sim_demo.py` to create **sanitization.csv** and explore it in **Mito**.
2. Open **RStudio** and run `app.R`.
3. Or run `app.py` with Streamlit.
4. When you have real data, **keep the same columns** and swap the CSV.

---

## 7) Next steps

* Add **RAG** (retrieve departmental SOPs for context) and annotate insights.
* Introduce **periwinkle Blues** theme or corporate palette.
* Deploy: **shinyapps.io** (R) or **Streamlit Community Cloud/Docker** (Python).

---

### Files included above

* `mito_sim_demo.py` — data generator for Mito Labs
* `app.R` — Shiny dashboard
* `app.py` — Streamlit dashboard

