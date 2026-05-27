# Session 04 Evidence – Custom Tool, Gradio Interface, and Testing

## Artifact 1: Custom Savings Goal Tool

**Development issue:**  
The assignment required one custom financial tool. I needed a tool that was simple enough to test but still useful for a personal finance assistant.

**Prompt summary:**  
I asked AI to help create a custom financial tool that would fit the Budget Buddy project.

**AI response summary:**  
AI suggested a savings goal calculator because it connects naturally to budgeting and can be tested clearly. The tool asks for:
- goal amount;
- current savings;
- monthly contribution.

It then calculates:
- remaining amount;
- number of months needed;
- approximate time in years and months.

**Important code snippet:**

```python
def calculate_savings_goal(goal_amount, current_savings, monthly_contribution):
    try:
        goal_amount = float(goal_amount)
        current_savings = float(current_savings)
        monthly_contribution = float(monthly_contribution)
    except ValueError:
        return "Please enter valid numbers for the goal amount, current savings, and monthly contribution."

    if goal_amount <= 0:
        return "The goal amount must be greater than zero."

    if current_savings < 0:
        return "Current savings cannot be negative."

    if monthly_contribution <= 0:
        return "Monthly contribution must be greater than zero."

    remaining_amount = goal_amount - current_savings
    months_needed = int(np.ceil(remaining_amount / monthly_contribution))
```

**Test input:**

```text
Goal amount: $5000
Current savings: $1200
Monthly contribution: $400
```

**Output:**

```text
Savings Goal Calculator
----------------------------------------
Goal amount: $5,000.00
Current savings: $1,200.00
Monthly contribution: $400.00
Remaining amount: $3,800.00
Months needed: 10
Approximate time: 10 month(s)
```

**Result:**  
The project gained a working custom financial tool that supports the main budgeting theme.

---

## Artifact 2: Building the Gradio Backend Function

**Development issue:**  
The individual components worked separately, but they needed to be connected into one interface. The Gradio app needed a single backend function that could load the CSV, analyse spending, answer the user’s question, and run the savings tool.

**Prompt summary:**  
I asked AI to help connect the data analysis, RAG context, chatbot, and savings calculator into one Gradio backend function.

**AI response summary:**  
AI helped create `run_budget_buddy_interface()`. This function:
- gets the uploaded CSV file path;
- loads and cleans the transaction data;
- creates the spending summary;
- converts monthly income into a number;
- generates business insights;
- calls the Budget Buddy chatbot;
- runs the savings goal calculator;
- returns three outputs to Gradio.

**Important code snippet:**

```python
def run_budget_buddy_interface(
    uploaded_file,
    monthly_income,
    user_question,
    goal_amount,
    current_savings,
    monthly_contribution
):
    file_path = get_file_path(uploaded_file)

    cleaned_transactions = load_and_clean_transaction_data(file_path)

    if cleaned_transactions is None:
        return (
            "The transaction file could not be loaded. Please check that it has Date, Amount, Category, and Description columns.",
            "No chatbot response is available because the data could not be loaded.",
            "No savings goal calculation is available because the data could not be loaded."
        )

    current_summary = analyze_spending_patterns(cleaned_transactions)

    analysis_output = generate_business_insights(
        current_summary,
        monthly_income=monthly_income
    )

    chatbot_output = budget_buddy_chatbot(
        user_question=user_question,
        df=cleaned_transactions,
        spending_summary=current_summary,
        monthly_income=monthly_income
    )

    savings_tool_output = calculate_savings_goal(
        goal_amount=goal_amount,
        current_savings=current_savings,
        monthly_contribution=monthly_contribution
    )

    return analysis_output, chatbot_output, savings_tool_output
```

**Result:**  
The main backend pipeline connected the project components together.

---

## Artifact 3: Gradio Interface Layout

**Development issue:**  
The project needed an interactive user interface that allowed a user to provide inputs and view outputs without editing code.

**Prompt summary:**  
I asked AI to help build a Gradio interface for the Budget Buddy assistant.

**AI response summary:**  
AI helped create a `gr.Blocks()` interface with:
- optional transaction CSV upload;
- monthly income input;
- finance question textbox;
- savings goal inputs;
- a button to run Budget Buddy;
- output box for spending analysis;
- output box for chatbot response;
- output box for savings goal result.

**Important code snippet:**

```python
with gr.Blocks(title="Budget Buddy Smart Finance Assistant") as budget_buddy_app:
    gr.Markdown("# Budget Buddy Smart Finance Assistant")
    gr.Markdown(
        "Upload a transaction CSV or use the default sample file. "
        "The assistant analyses spending, answers finance questions, "
        "and calculates a savings goal."
    )

    uploaded_file = gr.File(
        label="Upload transaction CSV (optional)",
        file_types=[".csv"]
    )

    monthly_income = gr.Number(
        label="Monthly income",
        value=2000
    )

    user_question = gr.Textbox(
        label="Ask a finance question",
        value="How can I reduce my grocery spending?",
        lines=2
    )
```

**Result:**  
The app became usable through a visual interface rather than only through notebook cells.

---

## Artifact 4: Gradio Output Test

**Development issue:**  
After building the interface, I needed to confirm that all three main outputs actually appeared in the Gradio app.

**Prompt summary:**  
I showed AI screenshots of the Gradio interface and asked whether the output looked correct.

**AI response summary:**  
AI confirmed that the interface was rendering correctly and told me to test whether the three output boxes filled in after clicking the button.

**Outputs confirmed in the interface:**
- Spending Analysis and Business Insights
- Chatbot Response with CSV Context
- Savings Goal Tool Output

**Example chatbot output shown in the interface:**  
The chatbot answered the grocery question using the transaction summary and CSV context. It included the grocery amount, practical suggestions, and the relevant transaction data.

**Result:**  
The Gradio interface worked as the final user-facing application.

---

## Artifact 5: Main Project Tests

**Development issue:**  
The notebook needed a testing section that checked normal cases, error cases, and edge cases.

**Prompt summary:**  
I asked AI to help write meaningful tests for the completed project.

**AI response summary:**  
AI helped create `run_project_tests()`, which checks:
- valid CSV loading;
- row count;
- spending summary creation;
- largest spending category;
- net expense calculation;
- business insights output;
- RAG context output;
- chatbot response;
- savings goal calculator;
- missing file handling;
- missing column handling;
- invalid amount handling.

**Example test code:**

```python
valid_data = load_and_clean_transaction_data("sample_transactions.csv")
check_test("Valid CSV loads successfully", valid_data is not None)
check_test("Valid CSV has 25 rows", len(valid_data) == 25)

valid_summary = analyze_spending_patterns(valid_data)
check_test("Spending summary is created", valid_summary is not None)
check_test("Groceries is the largest spending category", valid_summary["highest_category"] == "Groceries")
check_test("Net expenses are calculated correctly", round(valid_summary["total_expenses"], 2) == 1208.65)
```

**Test output:**

```text
Budget Buddy Test Results
----------------------------------------
PASS: Valid CSV loads successfully
PASS: Valid CSV has 25 rows
PASS: Spending summary is created
PASS: Groceries is the largest spending category
PASS: Net expenses are calculated correctly
PASS: Business insights include Budget Buddy heading
PASS: Business insights include groceries
PASS: RAG context includes transaction data heading
PASS: RAG context includes grocery transactions
PASS: Chatbot returns Budget Buddy response
PASS: Chatbot returns a non-empty response
PASS: Chatbot includes relevant data section
PASS: Chatbot does not return a hands-on-ai error
PASS: Savings goal calculator returns months needed
PASS: Savings goal handles zero monthly contribution
PASS: Missing file returns None
PASS: Missing columns return None
PASS: Invalid amount data returns None
----------------------------------------
Tests failed: 0
All tests passed.
```

**Result:**  
The notebook included meaningful tests for both correct inputs and likely user/data errors.

---

## Artifact 6: Error Handling Tests

**Development issue:**  
A finance app should not crash when given a bad file or invalid data.

**Prompt summary:**  
I asked AI to help include tests for error cases, not only normal cases.

**AI response summary:**  
AI added tests for:
- missing files;
- missing required columns;
- invalid amount values;
- zero monthly savings contribution.

**Example error outputs:**

```text
Error: The transaction file could not be found.
PASS: Missing file returns None

Error: Missing required columns: ['Category']
PASS: Missing columns return None

Error: No valid transactions were found after cleaning.
PASS: Invalid amount data returns None
```

**Result:**  
The project demonstrated defensive programming and user-friendly error handling.

---

## Artifact 7: Advanced Integration Tests

**Development issue:**  
The original starter notebook had placeholder integration tests that did not test much. I needed a more meaningful integration test that checked the whole assistant pipeline.

**Prompt summary:**  
I asked AI whether the advanced integration testing section was redundant and how to improve it.

**AI response summary:**  
AI explained that the placeholder tests were weak and suggested replacing them with a full pipeline test using the same backend function as the Gradio interface.

**Integration test purpose:**  
The advanced test checks the full pipeline:

```text
CSV loading → spending analysis → RAG context → hands-on-ai chatbot → savings tool → Gradio backend output
```

**Example test checks:**

```python
analysis_output, chatbot_output, savings_output = run_budget_buddy_interface(
    uploaded_file=None,
    monthly_income=2000,
    user_question="How can I reduce my grocery spending?",
    goal_amount=5000,
    current_savings=1200,
    monthly_contribution=400
)

check_test(
    "Gradio backend returns analysis output",
    "Budget Buddy Finance Insights" in analysis_output
)

check_test(
    "Gradio backend returns chatbot output",
    "Budget Buddy Response" in chatbot_output
)

check_test(
    "Gradio backend returns savings tool output",
    "Savings Goal Calculator" in savings_output
)
```

**Result:**  
The advanced integration test became more meaningful because it checked whether the main parts of the application worked together.

---

## Artifact 8: Run All Confirmation

**Development issue:**  
Even if individual cells worked, the final notebook needed to run properly from top to bottom.

**Prompt summary:**  
I told AI that I ran all cells and no issue appeared.

**AI response summary:**  
AI confirmed that this was a strong technical checkpoint and recommended saving and committing the working version before making more changes.

**Result:**  
The notebook was confirmed to be technically stable after running from top to bottom.

---

## Reflection

This session moved the project from separate working functions into a complete application. The custom savings tool gave the assistant a clear agent-style finance function, while the Gradio interface made the project usable by someone who does not want to run code manually.

The most important learning point was integration. A function can work alone, but the assignment required the components to work together. The backend function and integration tests helped confirm that the CSV analysis, RAG context, hands-on-ai chatbot, savings tool, and Gradio interface were connected properly.

Testing also made the project more reliable. The error handling tests showed that the assistant can deal with missing files, missing columns, invalid amounts, and invalid savings inputs without crashing.