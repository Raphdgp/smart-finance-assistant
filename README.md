# 📊 Budget Buddy – Smart Finance Assistant

<!-- BADGES:START -->
[![curtin](https://img.shields.io/badge/-curtin-f57c00?style=flat-square)](https://github.com/topics/curtin) [![ai-assistant](https://img.shields.io/badge/-ai--assistant-blue?style=flat-square)](https://github.com/topics/ai-assistant) [![chatbot](https://img.shields.io/badge/-chatbot-blue?style=flat-square)](https://github.com/topics/chatbot) [![edtech](https://img.shields.io/badge/-edtech-4caf50?style=flat-square)](https://github.com/topics/edtech) [![finance](https://img.shields.io/badge/-finance-blue?style=flat-square)](https://github.com/topics/finance) [![financial-tools](https://img.shields.io/badge/-financial--tools-blue?style=flat-square)](https://github.com/topics/financial-tools) [![gradio](https://img.shields.io/badge/-gradio-blue?style=flat-square)](https://github.com/topics/gradio) [![jupyter-notebook](https://img.shields.io/badge/-jupyter--notebook-blue?style=flat-square)](https://github.com/topics/jupyter-notebook) [![python](https://img.shields.io/badge/-python-3776ab?style=flat-square)](https://github.com/topics/python) [![rag](https://img.shields.io/badge/-rag-blue?style=flat-square)](https://github.com/topics/rag)
<!-- BADGES:END -->

Welcome to my project repository for the **ISYS2001 Final Programming Project**. This repository contains my **Budget Buddy Smart Finance Assistant**, a personal finance application that analyses transaction data and gives practical budgeting guidance.

---

## 📖 Project Overview

**Budget Buddy** is a Smart Finance Assistant designed for students and young adults who want to understand their spending habits from transaction data.

The assistant uses a CSV file containing transaction records and turns it into useful finance insights. It can identify the largest spending categories, calculate net expenses, handle refunds, estimate savings based on monthly income, answer finance questions through a chatbot-style response, and calculate how long it will take to reach a savings goal.

The project includes the required Smart Finance Assistant components:

- **Chat**: a finance-oriented Budget Buddy chatbot that answers user questions using transaction data.
- **RAG**: CSV-based retrieval that finds transactions relevant to the user's question.
- **Agent Tool**: a custom savings goal calculator.
- **Gradio UI**: an interactive interface tying the analysis, chatbot, and tool together.
- **Tests**: a testing section covering normal data, edge cases, and error handling.

---

## 💼 Business Problem

Many people can access their bank transactions, but the raw data is not always easy to interpret. Users may know that they are spending money, but not which category matters most or what action would make the biggest difference.

Budget Buddy solves this by converting transaction records into clear and practical insights. For example, instead of only showing a table of purchases, it can explain that groceries are the largest spending category, show how much they represent, and suggest realistic ways to reduce spending without making extreme changes.

---

## 🧠 Main Features

### 📊 Transaction Analysis

The assistant loads a transaction CSV file with these columns:

```text
Date, Amount, Category, Description
```

It then calculates:

- Total number of transactions
- Expense transactions
- Refund transactions
- Net expenses
- Average expense transaction
- Spending by category
- Largest spending category

### 🔍 CSV-Based RAG Context

The assistant retrieves relevant rows from the CSV based on the user's question.

Example question:

```text
How can I reduce my grocery spending?
```

The system retrieves grocery-related transactions such as Woolworths, Coles, IGA, and Costco transactions, then uses them as context for the chatbot response.

### 💬 Budget Buddy Chatbot

The chatbot gives warm, practical, finance-focused advice using the user's transaction data.

Example response summary:

```text
Your grocery spending is $540.30, which is about 44.7% of your net expenses.
A 10% reduction would save about $54.03.
A 15% reduction would save about $81.05.
```

### 🛠️ Savings Goal Calculator

The custom tool calculates how long it will take to reach a savings goal.

Example input:

```text
Goal amount: $5000
Current savings: $1200
Monthly contribution: $400
```

Example output:

```text
Remaining amount: $3800
Months needed: 10
Approximate time: 10 months
```

### 🌐 Gradio Interface

The Gradio interface lets the user:

- Upload a transaction CSV or use the default sample file
- Enter monthly income
- Ask a finance question
- Enter savings goal details
- View spending insights, chatbot advice, and savings goal results

---

## 📂 Repository Layout

```text
/README.md                  ← Project overview and run instructions
/starter_notebook.ipynb     ← Main Smart Finance Assistant notebook
/sample_transactions.csv    ← Sample transaction dataset
/diary.md                   ← Developer diary / AI collaboration documentation
/AI-EVIDENCE/          ← AI evidence snippets or screenshots, if used
```

---

## 📌 Sample Data

The default sample file is:

```text
sample_transactions.csv
```

It contains 25 transaction rows with these columns:

```text
Date, Amount, Category, Description
```

Example categories include:

- Groceries
- Entertainment
- Utilities
- Dining
- Transport
- Coffee
- Refund

---

## 🧪 Testing

The notebook includes two testing sections:

1. **Function and edge-case tests**, covering:
   - valid CSV loading;
   - correct row count;
   - spending summary creation;
   - largest category detection;
   - net expense calculation;
   - business insight output;
   - RAG context output;
   - chatbot response;
   - savings goal calculator;
   - missing file handling;
   - missing column handling;
   - invalid amount handling.

2. **Advanced integration tests**, checking that the full Budget Buddy pipeline works through the same backend function used by the Gradio interface.

Tests passed: 17
Tests failed: 0
Advanced tests failed: 0
---

## 🤖 AI Collaboration

AI assistance was used during development for planning, code drafting, debugging, testing, and improving the finance logic. The notebook and diary document how AI suggestions were tested and refined.

Examples of AI-supported development work include:

- setting up the project environment;
- organising the starter notebook;
- creating the CSV cleaning function;
- improving refund handling;
- refining the chatbot tone and use of figures;
- building test cases for normal and invalid data.

AI output was not accepted blindly. The generated code was run in the notebook, checked against actual outputs, and adjusted when results were unclear or financially misleading.

---

## 📊 Assessment Criteria Addressed

This project addresses the main assessment areas:

- **Functionality**: chatbot, CSV/RAG context, custom savings tool, and Gradio UI are integrated.
- **Testing & Debugging**: the notebook includes meaningful tests and edge cases.
- **AI Collaboration**: AI use is documented in the diary/evidence materials.
- **Business Relevance**: the assistant solves a practical budgeting and spending awareness problem.
- **Clarity & Reflection**: the repository includes a README, notebook explanations, and project documentation.

---

## 💡 Notes and Limitations

- The default dataset is small and intended for demonstration.
- The sample transaction file does not include income rows, so monthly income is entered manually.
- The chatbot uses the `hands-on-ai` external chat server with CSV-based transaction context.
- The chatbot response may vary slightly because it is generated by an AI model, but it is grounded using the transaction summary and retrieved CSV rows.
- The assistant gives general budgeting guidance, not professional financial advice.

---

## 📜 License

This repository is based on the provided Smart Finance Assistant student project template. The template includes an MIT License file.

The project was developed for ISYS2001 assessment purposes.