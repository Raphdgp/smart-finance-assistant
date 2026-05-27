# 📓 Developer's Diary – AI Collaboration Guide

## Entry 1 – Environment Setup and Notebook Organisation

**Artifact:**  
See `AI-EVIDENCE/session_01_setup_environment.md`.

**Context:**  
I needed to set up the Smart Finance Assistant repository, configure Python, and organise the starter notebook before building the application.

**My Prompt:**  
I asked AI for a step-by-step setup process covering the GitHub repository, VS Code, Python environment, package installation, notebook kernel selection, and initial notebook organisation.

**AI Response Summary:**  
AI guided me through opening the forked repository in VS Code, checking Python versions, using Python 3.12, creating a virtual environment, installing packages, selecting the notebook kernel, and running a small import test. AI also helped reorganise the notebook so that the implementation followed the starter notebook’s existing structure.

**My Critique/Improvement:**  
I questioned the setup process when it became more complicated than expected. When PowerShell blocked virtual environment activation, I avoided changing Windows security settings and used the virtual environment’s Python executable directly instead. I also noticed that adding code cells randomly would make the notebook messy, so I asked for the work to be integrated into the existing notebook sections.

**Result:**  
The repository opened correctly in VS Code, the required packages were installed, the notebook kernel worked, and the project structure was cleaned up before the main implementation began.

**Reflection:**  
This step helped me understand that environment setup and notebook organisation are part of the programming process. A working notebook depends on the correct Python version, installed packages, kernel selection, and execution order. I also learned that AI suggestions need to be checked and adapted to the actual development environment rather than followed mechanically.

## Entry 2 – CSV Loading, Cleaning, and Spending Analysis

**Artifact:**  
See `AI-EVIDENCE/session_02_data_analysis.md`.

**Context:**  
I needed to build the transaction data foundation for Budget Buddy by loading the CSV file, cleaning it, calculating spending totals, handling refunds, and identifying the largest spending categories.

**My Prompt:**  
I asked AI to help inspect the transaction CSV, decide which dataset to use, write a data loading and cleaning function, and create a spending analysis function.

**AI Response Summary:**  
AI helped confirm that `sample_transactions.csv` had the required columns: `Date`, `Amount`, `Category`, and `Description`. It then helped create the `load_and_clean_transaction_data()` function and the `analyze_spending_patterns()` function. These functions load the CSV, clean dates and amounts, validate required columns, separate refunds, calculate net expenses, group spending by category, and identify the largest spending category.

**My Critique/Improvement:**  
I checked the notebook output rather than assuming the generated code was correct. The refund logic needed adjustment because refunds were initially at risk of being treated as normal expenses or being subtracted twice. I reviewed the figures and kept the final logic consistent with the dataset, where all amounts are stored as positive values.

**Result:**  
The notebook now loads 25 transaction rows, calculates net expenses of `$1208.65`, identifies refunds of `$37.50`, and shows Groceries as the largest spending category at `$540.30`.

**Reflection:**  
This section helped me understand that successful code is not always correct code. The functions ran, but the financial interpretation still needed checking. I learned to compare outputs against the actual transaction data and to think carefully about how refunds should affect spending analysis.

## Entry 3 – CSV-Based RAG and hands-on-ai Chatbot Integration

**Artifact:**  
See `AI-EVIDENCE/session_03_rag_chatbot.md`.

**Context:**  
I needed to connect the transaction data to the chatbot so that Budget Buddy could answer finance questions using relevant CSV information instead of giving generic advice.

**My Prompt:**  
I asked AI to help place the RAG code in the correct notebook section, create a CSV-based retrieval function, improve the retrieval output, and integrate the retrieved transaction context with a hands-on-ai chatbot.

**AI Response Summary:**  
AI helped create the CSV-based RAG functions `retrieve_relevant_transactions()` and `create_transaction_context()`. The first retrieval version returned some unrelated rows for a grocery question, so AI helped improve the matching logic by handling singular and plural forms such as `grocery` and `groceries`. AI also helped replace the earlier local chatbot with a hands-on-ai chatbot that uses the transaction summary and retrieved CSV rows as prompt context.

**My Critique/Improvement:**  
I tested the RAG output and noticed that the first version was too broad because it included unrelated categories. I used this output to ask for a more accurate retrieval method. I also checked the hands-on-ai setup carefully because the first connection test could print a success message even when the returned response contained an error. After identifying the API key issue and restarting the notebook/app state, the hands-on-ai chatbot worked inside the Gradio interface.

**Result:**  
The project now includes CSV-based RAG and a hands-on-ai powered chatbot. For a grocery question, the RAG system retrieves grocery-related transactions from the CSV and the chatbot uses that context to produce a more relevant finance response.

**Reflection:**  
This section helped me understand why RAG matters. Without CSV context, the chatbot could only give general advice. With retrieved transaction rows and a spending summary, the chatbot can refer to actual categories, amounts, and examples from the user’s data. I also learned that external AI integration needs careful testing because connection issues, API key mistakes, and stale notebook state can all affect the result.

## Entry 4 – Custom Tool, Gradio Interface, and Testing

**Artifact:**  
See `AI-EVIDENCE/session_04_gradio_tool_testing.md`.

**Context:**  
I needed to turn the separate Budget Buddy functions into a complete application with a custom financial tool, an interactive interface, and meaningful tests.

**My Prompt:**  
I asked AI to help create a custom savings goal tool, connect the analysis and chatbot functions into a Gradio interface, and write tests for both normal use and error cases.

**AI Response Summary:**  
AI helped create the `calculate_savings_goal()` tool, the `run_budget_buddy_interface()` backend function, and the Gradio interface. The interface lets users upload a CSV, enter monthly income, ask a finance question, enter savings goal details, and view three outputs: spending insights, chatbot response, and savings goal result. AI also helped replace weak placeholder integration tests with tests that check the full application pipeline.

**My Critique/Improvement:**  
I tested the Gradio interface directly and confirmed that all three output boxes filled correctly. I also checked whether some test/demo cells were redundant and focused on keeping the meaningful tests. When the chatbot was changed to use hands-on-ai, I updated the tests so they checked for a valid response rather than relying on exact wording that could vary.

**Result:**  
The final notebook includes a working Gradio app, a custom savings goal calculator, CSV-based RAG, hands-on-ai chatbot integration, and passing tests. The notebook was also tested with “Run All,” and no issues appeared.

**Reflection:**  
This section helped me understand the difference between separate functions and an integrated application. The project only became a full assistant once the analysis, chatbot, RAG context, savings tool, and Gradio interface worked together. I also learned that testing should include both normal workflows and error cases, because users may upload bad files or enter invalid values.

## Entry 5 – Documentation, README, and Final Cleanup

**Artifact:**  
See `AI-EVIDENCE/session_05_documentation_final_cleanup.md`.

**Context:**  
I needed to clean up the notebook and README so that they accurately described the completed Budget Buddy project rather than the original starter template.

**My Prompt:**  
I asked AI to review the notebook and README for inconsistencies, redundant code, outdated wording, and anything that no longer matched the final implementation.

**AI Response Summary:**  
AI identified several documentation and cleanup issues. The README still looked like the starter template and needed to be rewritten around Budget Buddy. Some wording still described rule-based chatbot logic even though the chatbot had been updated to use `hands-on-ai`. The notebook also had some template-style or outdated wording, including references to document retrieval instead of CSV-based transaction retrieval. AI also helped clarify how to use the six-step methodology as one process for the whole project rather than repeating it for every function.

**My Critique/Improvement:**  
I focused on the changes that affected clarity and consistency, rather than trying to rewrite every small markdown issue. I checked whether the notebook still ran after the changes and asked AI to focus mainly on redundant code. I removed or corrected parts that could confuse the marker, such as old sample-data code and outdated descriptions.

**Result:**  
The README became specific to the completed Budget Buddy assistant. The notebook methodology section was rewritten around the actual project, the RAG wording was updated to reflect CSV-based retrieval, redundant starter code was removed, and the notebook ran successfully from top to bottom.

**Reflection:**  
This section showed me that a programming project is not finished just because the code works. The documentation also needs to explain what the project does, how it works, and how it should be run. I learned that cleanup is important because leftover template code or outdated descriptions can make a working project look inconsistent or unfinished.

