# 📊 DAY 3 — Mito Labs + Jupyter Setup (Windows, one-click BAT)

> Goal: get you from zero → a working **JupyterLab + Mito** environment with a single double-click, plus fixes for the exact warnings/errors you saw.

---

## 🧠 What you’ll accomplish

* Create an isolated Python env (`mitoenv`)
* Install **JupyterLab**, **mitosheet (Mito UI)**, and **mito-ai** (optional)
* Register a Jupyter kernel: **Python (mitoenv)**
* Launch JupyterLab in your **Documents** (default) or **Downloads**
* Open a notebook and run **Mito** (`mitosheet.sheet()`)

---

## ⚡ One-click installer (BAT)

👇 **Tell me which you prefer** and I’ll tailor next steps:

* **A.** `%USERPROFILE%\Documents\mitoenv` (default & recommended)
* **B.** `%USERPROFILE%\Downloads\mitoenv`

> Reply with **A or B** (or a custom full path) and I’ll update the script path for you.
> For now, the script below defaults to **Documents** and supports **Downloads** via a toggle.

### 1) Save this as `Setup_MitoEnv.bat` (right-click → New → Text Document → paste → Save as “All Files”)

```bat
@echo off
setlocal ENABLEDELAYEDEXPANSION

:: =========================================================
:: Mito Labs + Jupyter One-Click Setup (Windows)
:: Creates venv, installs JupyterLab, mitosheet, mito-ai, registers kernel, launches JupyterLab.
:: Default location: %USERPROFILE%\Documents\mitoenv
:: To install under Downloads instead, run with: Setup_MitoEnv.bat downloads
:: =========================================================

:: ---- Choose install base (Documents by default) ----
set TARGET_BASE=%USERPROFILE%\Documents
if /I "%1"=="downloads" set TARGET_BASE=%USERPROFILE%\Downloads

set VENV_DIR=%TARGET_BASE%\mitoenv
set LOG_FILE=%TARGET_BASE%\mito_setup.log

echo [INFO] Installing to: %VENV_DIR%
echo [INFO] Logging to: %LOG_FILE%
echo.

:: ---- Ensure Python is available ----
where python >nul 2>&1
if errorlevel 1 (
  echo [ERROR] Python not found on PATH. Install Python 3.10+ from https://www.python.org/downloads/ and re-run. | tee
  goto :end
)

:: ---- Create virtual environment ----
if not exist "%VENV_DIR%" (
  echo [INFO] Creating virtual environment: mitoenv | tee
  python -m venv "%VENV_DIR%" >> "%LOG_FILE%" 2>&1
) else (
  echo [INFO] Virtual environment already exists: %VENV_DIR% | tee
)

:: ---- Activate and upgrade pip/wheel ----
call "%VENV_DIR%\Scripts\activate"
echo [INFO] Upgrading pip and wheel... | tee
python -m pip install --upgrade pip wheel >> "%LOG_FILE%" 2>&1

:: ---- Core installs ----
echo [INFO] Installing JupyterLab, mitosheet, and mito-ai... | tee
pip install ^
  "jupyterlab>=4" "notebook-shim" "jupyter-lsp" ^
  "mitosheet>=0.1.582" "openpyxl" ^
  "mito-ai>=0.1.0" "streamlit" "plotly" "pandas>=2" "numpy" ^
  >> "%LOG_FILE%" 2>&1

:: ---- Optional: pin setuptools <81 to suppress pkg_resources removal warnings (safe) ----
pip install "setuptools<81" >> "%LOG_FILE%" 2>&1

:: ---- Register Jupyter kernel ----
echo [INFO] Registering Jupyter kernel 'Python (mitoenv)'... | tee
python - << "PYCODE"
import sys, json, subprocess
name = "mitoenv"
display = "Python (mitoenv)"
subprocess.check_call([sys.executable, "-m", "ipykernel", "install", "--user", "--name", name, "--display-name", display])
print("[SUCCESS] Kernel registered:", display)
PYCODE
>> "%LOG_FILE%" 2>&1

:: ---- Friendly summary ----
echo. | tee
echo [SUCCESS] Mito environment ready. Launching JupyterLab... | tee
echo     - Kernel: Python (mitoenv) | tee
echo     - Working dir: %TARGET_BASE% | tee
echo     - If browser opens, create a new notebook and run: | tee
echo           import mitosheet ^&^& mitosheet.sheet() | tee
echo. | tee

:: ---- Launch JupyterLab in the chosen folder ----
pushd "%TARGET_BASE%"
jupyter lab
popd

goto :end

:tee
:: Echo both to console and append to log
for /f "tokens=1,* delims=:" %%a in ("%*") do (
  echo %%a:%%b
  echo %%a:%%b>>"%LOG_FILE%"
)
exit /b 0

:end
endlocal
```

**How to run**

* Double-click `Setup_MitoEnv.bat` to install under **Documents**
* Or **right-click → Run** with argument `downloads` to install under **Downloads**:

  ```
  Setup_MitoEnv.bat downloads
  ```

> After it finishes, your default web browser should open **JupyterLab** (e.g. `http://127.0.0.1:8888/lab`).
> In JupyterLab, click **Python (mitoenv)** kernel → **New Notebook**, then run:

```python
import mitosheet
mitosheet.sheet()
```

---

## ✅ What “good” looks like (matches your logs)

You should see messages like:

* `Installed kernelspec mitoenv ...`
* `jupyter_lsp | extension was successfully loaded.`
* `mito_ai | extension was successfully loaded.`
* `Serving notebooks from ... Documents`
* JupyterLab URLs with tokens (e.g., `http://127.0.0.1:8888/lab?...`)

---

## 🧪 Quick verification cell (copy into a new notebook)

```python
import sys, pandas as pd, mitosheet
print("Python:", sys.version)
print("Pandas:", pd.__version__)
mitosheet.sheet()
```

You should see the **Mito spreadsheet** UI open in your notebook.

---

## 🧰 Common issues & fixes (tailored to your output)

### 1) `UserWarning: pkg_resources is deprecated`

* Cause: Newer Setuptools deprecates `pkg_resources`.
* Fix: The BAT script adds `setuptools<81` to suppress this. You can ignore the warning; functionality is unaffected.

### 2) `mito_ai ... Importing 'mito_ai' outside a proper installation.`

* Usually harmless; it occurs when the server extension is linked. If it persists:

  * Re-install: `pip install --force-reinstall mito-ai`
  * Or **disable AI extension** if you won’t use it:

    ```bash
    jupyter server extension disable mito_ai
    ```

### 3) Anthropic timeouts / `ProviderCompletionException`

If you see retries or “Request timed out”:

* Check your **internet** or firewall/VPN.
* Provide a valid **Anthropic/OpenAI key** to mito-ai (if you intend to use AI completions):

  * In Windows, set environment variables before launching Jupyter:

    ```bat
    setx ANTHROPIC_API_KEY "sk-ant-..."
    setx OPENAI_API_KEY "sk-..."
    ```
  * Then re-launch `jupyter lab` from the **same** `mitoenv` (`Setup_MitoEnv.bat` already does this).
* Or **disable AI completions** entirely (see #2).

### 4) `websocket_ping_timeout` vs `websocket_ping_interval` warning

* Tornado warns if timeout > interval. It auto-adjusts. Safe to ignore.
* To silence it, create (optional) `%USERPROFILE%\.jupyter\jupyter_server_config.py`:

  ```python
  c = get_config()  # type: ignore
  c.ServerApp.websocket_ping_interval = 30000  # ms
  c.ServerApp.websocket_ping_timeout  = 30000  # ms
  ```

### 5) CPU spikes / slow builds

* The script installs **Plotly/Streamlit** too. If you want a slimmer Mito-only env, comment those lines in the BAT.

---

## 🧱 (Optional) Desktop shortcut to launch later

Create `Launch_MitoLab.bat`:

```bat
@echo off
set VENV_DIR=%USERPROFILE%\Documents\mitoenv
if /I "%1"=="downloads" set VENV_DIR=%USERPROFILE%\Downloads\mitoenv
call "%VENV_DIR%\Scripts\activate"
pushd %USERPROFILE%\Documents
jupyter lab
popd
```

Right-click → **Send to → Desktop (create shortcut)**.

---

## 🧩 Starter notebook cell: load Excel & start Mito

```python
import pandas as pd, mitosheet
df = pd.read_excel("TG_Product_Activity_2025.xlsx")  # put the file in your working folder
mitosheet.sheet(df)
```

Then use Mito’s point-and-click tools to:

* Drop nulls/dupes
* Standardize column names
* Derive columns
* Export the cleaned DataFrame back to CSV/Excel

---

## 🧼 Export Mito steps as Python

From the **Mito UI**, click **Export → Python** to capture your cleaning steps as reproducible code.
Drop that code into your **Day 4 Trigger Cells**.

---

## 🧽 Uninstall / Update

* Remove env: delete the folder (`Documents\mitoenv` or `Downloads\mitoenv`)
* Re-run `Setup_MitoEnv.bat` for a fresh install
* Update packages later:

  ```bat
  call %USERPROFILE%\Documents\mitoenv\Scripts\activate
  pip install --upgrade mitosheet mito-ai jupyterlab
  ```

---

## 🎯 Your turn

1. **Tell me where you want the BAT to install** (A: Documents, B: Downloads, or custom path).
2. I’ll return a path-locked BAT tailored to your choice.
3. Double-click it, then create a new notebook → run `import mitosheet; mitosheet.sheet()` and you’re done.
