# Session 01 Evidence – Environment Setup and Notebook Organisation

## Artifact 1: Setup Request

**Prompt summary:**  
I asked AI to give me a clear step-by-step setup process for the Smart Finance Assistant project. I needed help opening the forked GitHub repository in VS Code, setting up Python, selecting the correct notebook kernel, and preparing the starter notebook.

**AI response summary:**  
AI guided me through the setup process in small steps:
- open the forked GitHub repository in VS Code;
- check whether Python was available from the terminal;
- use Python 3.12 instead of Python 3.14;
- create a virtual environment;
- install the required packages;
- select the correct notebook kernel;
- run a small test cell to confirm the notebook worked.

**Result:**  
The repository opened successfully in VS Code and the notebook could run Python code.

---

## Artifact 2: Python and Package Setup Issue

**Problem encountered:**  
The command `python --version` did not work in the terminal, but `py --version` did work. PowerShell also blocked the normal virtual environment activation script.

**AI support:**  
AI explained that the VS Code Python extension is not the same as Python being correctly available in the terminal. AI also suggested avoiding the PowerShell activation issue by using the virtual environment Python executable directly.

**Command used:**

```powershell
.\venv\Scripts\python.exe -m pip install jupyter notebook pandas requests gradio matplotlib hands-on-ai
```

**Result:**  
The required packages installed successfully without changing Windows execution policy settings.

---

## Artifact 3: Notebook Kernel Test

**Notebook test code:**

```python
import pandas as pd
import gradio as gr
import matplotlib.pyplot as plt
import requests

print("Notebook setup works")
```

**Output:**

```text
Notebook setup works
```

**Result:**  
The notebook kernel and main packages were working correctly.

---

## Artifact 4: Notebook Organisation

**Prompt summary:**  
I asked AI how to keep the notebook organised because adding code cells one after another was making the file messy.

**AI response summary:**  
AI advised using the starter notebook’s existing sections instead of placing all new code at the top. The project was then organised into sections for setup, transaction analysis, RAG, chatbot, custom tool, Gradio, and testing.

**Result:**  
The notebook structure became clearer and better aligned with the assignment scaffold.

---

## Reflection

This first session focused on getting the project into a usable development state. The main issue was not the finance logic yet, but the environment: Python version, virtual environment, package installation, and notebook kernel selection. I also learned that notebook organisation matters because a working project still needs to be readable and easy to assess.

AI was useful as a setup guide, but I still had to run the commands, check the outputs, report errors, and decide which suggestions were practical for my computer.
