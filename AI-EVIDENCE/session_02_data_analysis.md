# Session 02 Evidence – CSV Loading, Cleaning, and Spending Analysis

## Artifact 1: Choosing and Inspecting the CSV Dataset

**Development issue:**  
At the start of the data analysis section, I was unsure whether to use the `sample_transactions.csv` file already inside the repository or the `transactions.csv` file from one of the weekly module files.

**Prompt summary:**  
I asked AI whether the repository CSV was suitable for the assignment or whether I should use the module CSV instead.

**AI response summary:**  
AI explained that the assignment required CSV-based transaction analysis, but did not require one exact CSV file. Since the repository file had the required transaction structure, it could be used as the main dataset.

**Code used to inspect the CSV:**

```python
import pandas as pd
import os

csv_files = ["sample_transactions.csv"]

for file_name in csv_files:
    if os.path.exists(file_name):
        print("=" * 60)
        print(f"File: {file_name}")
        data = pd.read_csv(file_name)
        print("Rows:", len(data))
        print("Columns:", data.columns.tolist())
        display(data.head())
    else:
        print(f"{file_name} is not in this folder.")
```

**Notebook output:**

```text
File: sample_transactions.csv
Rows: 25
Columns: ['Date', 'Amount', 'Category', 'Description']
```

**What this showed:**  
The CSV had the four required columns needed for the assistant:
- `Date`
- `Amount`
- `Category`
- `Description`

**Result:**  
I used `sample_transactions.csv` as the project’s working dataset because it matched the required structure and was already included in the repository.

---

## Artifact 2: Data Loading and Cleaning Function

**Development issue:**  
The raw CSV needed to be loaded safely before analysis. Even though the sample file was clean enough to open, the project still needed a function that could handle common CSV problems such as missing files, missing columns, invalid dates, and invalid amounts.

**Prompt summary:**  
I asked AI to help create a function that would load the transaction CSV and prepare it for analysis.

**AI response summary:**  
AI helped create a `load_and_clean_transaction_data()` function. The function:
- checks whether the file can be loaded;
- verifies that all required columns are present;
- converts the `Date` column into datetime format;
- removes dollar signs, commas, and spaces from `Amount`;
- converts `Amount` into numeric values;
- removes invalid rows;
- standardises the `Category` and `Description` text columns.

**Important code snippet:**

```python
required_columns = ["Date", "Amount", "Category", "Description"]

missing_columns = []

for column in required_columns:
    if column not in df.columns:
        missing_columns.append(column)

if len(missing_columns) > 0:
    print(f"Error: Missing required columns: {missing_columns}")
    return None
```

**Notebook output after testing the function:**

```text
Transaction data loaded and cleaned successfully.
Rows loaded: 25
```

**What this showed:**  
The function successfully loaded the project CSV and produced a cleaned DataFrame. It also gave a clear success message and row count, making the output easy to verify.

**Result:**  
The cleaned transaction data became the foundation for all later features, including spending analysis, RAG context retrieval, chatbot responses, Gradio output, and testing.

---

## Artifact 3: Debugging Notebook Execution Order

**Development issue:**  
When I first tried to run the data loading function, the notebook produced an error because pandas had not been imported in the current kernel session.

**Error encountered:**

```text
Error loading file: name 'pd' is not defined
```

**Prompt summary:**  
I reported the error to AI and asked what caused it.

**AI response summary:**  
AI explained that `pd` is created by this import:

```python
import pandas as pd
```

The issue was not the function itself. The problem was that the Initial Setup cell had not been run, or the kernel had restarted and lost the earlier imports.

**Fix applied:**  
I reran the notebook cells in the correct order:
1. Initial Setup / imports
2. Data loading and cleaning function
3. CSV loading test cell

**Result:**  
The CSV loading function worked correctly after the setup cell was run first.

**What I learned:**  
In a notebook, code cells depend on execution order. A function can be written correctly but still fail if imports or variables from earlier cells have not been run in the active kernel.

---

## Artifact 4: Spending Analysis Function

**Development issue:**  
After loading the data, I needed to calculate useful financial metrics rather than only display the raw transactions.

**Prompt summary:**  
I asked AI to help create a spending analysis function that could calculate spending totals, separate refunds, group spending by category, and identify the largest category.

**AI response summary:**  
AI helped create the `analyze_spending_patterns()` function. The function:
- creates lowercase helper text for classification;
- identifies refund transactions using keywords such as `refund`, `reimbursement`, and `cashback`;
- separates refund rows from normal expense rows;
- calculates gross expenses;
- reports refunds separately;
- calculates net expenses used for analysis;
- groups expenses by category;
- identifies the highest spending category;
- calculates the average expense transaction.

**Important code snippet:**

```python
refund_keywords = ["refund", "reimbursement", "cashback"]

def is_refund_transaction(row):
    combined_text = row["category_lower"] + " " + row["description_lower"]
    return contains_keyword(combined_text, refund_keywords)

data["Is_Refund"] = data.apply(is_refund_transaction, axis=1)

refund_data = data[data["Is_Refund"] == True]
expense_data = data[(data["Is_Income"] == False) & (data["Is_Refund"] == False)]
```

**Notebook output:**

```text
Finance Summary
----------------------------------------
Number of transactions: 25
Income transactions: 0
Refund transactions: 2
Expense transactions: 23
Total income: $0.00
Gross expenses before refunds: $1208.65
Total refunds: $37.50
Net expenses after refunds: $1208.65
Net savings: $-1208.65
Savings rate: 0.00%
Highest spending category: Groceries ($540.30)
Average expense transaction: $52.55
```

**Spending by category output:**

```text
Category
Groceries        540.30
Entertainment    206.95
Utilities        159.80
Dining           148.85
Transport        108.00
Coffee            44.75
Name: Amount, dtype: float64
```

**Result:**  
The assistant could now summarise the transaction dataset in a useful way. It identified Groceries as the largest category and gave clear spending totals.

---

## Artifact 5: Refund Logic Correction

**Development issue:**  
The first version of the refund logic was not fully reliable. Refunds risked either being counted as normal spending or being subtracted twice.

**Prompt summary:**  
I asked AI to check whether the spending analysis output was correct.

**AI response summary:**  
AI explained that refunds needed careful treatment because the sample CSV stores all amounts as positive values. If refund rows are already excluded from expenses, subtracting them again would double-count the adjustment.

**Correction made:**  
The logic was changed so that:
- refund rows are identified separately;
- refund rows are excluded from expense category totals;
- refunds are reported separately;
- net expenses use the normal expense rows only;
- refunds are not subtracted a second time after exclusion.

**Corrected interpretation:**

```text
Original total including refund rows = $1246.15
Refunds identified = $37.50
Normal expenses excluding refund rows = $1208.65
Net expenses used for analysis = $1208.65
```

**Why this matters:**  
If refunds were treated incorrectly, the assistant would give misleading budgeting advice. For example, including refunds as a spending category would make spending look higher than it really was. Subtracting refunds twice would make spending look lower than it really was.

**Result:**  
The final output treated refunds consistently and gave a more reliable spending summary.

---

## Artifact 6: Business Insights Generator

**Development issue:**  
The project needed more than raw calculations. It needed to convert spending numbers into user-friendly financial insights.

**Prompt summary:**  
I asked AI to help create a business insights function that would explain the spending results in plain English.

**AI response summary:**  
AI helped create `generate_business_insights()`. This function:
- reports net expenses;
- reports refunds;
- identifies the largest spending category;
- calculates estimated savings if monthly income is provided;
- calculates estimated savings rate;
- gives category-level observations;
- suggests a practical starting action.

**Notebook output example:**

```text
Budget Buddy Finance Insights
----------------------------------------
Net expenses after refunds: $1208.65
Refunds identified: $37.50
Average expense transaction: $52.55
Largest spending category: Groceries ($540.30)

Monthly income entered: $2000.00
Estimated savings after expenses: $791.35
Estimated savings rate: 39.57%
Your estimated savings rate is strong. You may have room to build savings or invest after keeping an emergency fund.
```

**Result:**  
The project moved from simple transaction analysis to business-friendly financial interpretation. The insights became easier for a user to understand and act on.

---

## Reflection

This session showed that data analysis is not only about writing code that runs. The outputs also need to make financial sense. The biggest learning point was the refund issue: the code could technically work but still produce misleading results if the business logic was wrong.

AI helped draft the functions and suggest improvements, but I had to test the code with the real CSV file, inspect the outputs, and question whether the numbers made sense. This helped make the transaction analysis foundation more reliable before moving on to RAG, chatbot integration, Gradio, and testing.