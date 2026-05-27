# Session 03 Evidence – CSV-Based RAG and hands-on-ai Chatbot Integration

## Artifact 1: CSV-Based Retrieval Function

**Development issue:**  
The chatbot needed access to relevant transaction rows from the CSV file. Without retrieval, the chatbot would only give generic budgeting advice.

**Prompt summary:**  
I asked AI to help create a CSV-based retrieval system that could find transactions relevant to the user’s finance question.

**AI response summary:**  
AI helped create two functions:
- `retrieve_relevant_transactions()`
- `create_transaction_context()`

The retrieval function checks the user’s question, matches keywords against transaction `Category` and `Description`, and returns relevant transaction rows. The context function then combines the retrieved rows with the spending summary so the chatbot has useful financial context.

**Important code snippet:**

```python
def retrieve_relevant_transactions(df, user_question, max_rows=5):
    if df is None or len(df) == 0:
        return pd.DataFrame()

    data = df.copy()

    question_words = (
        str(user_question)
        .lower()
        .replace("?", "")
        .replace(",", "")
        .replace(".", "")
        .split()
    )
```

**Result:**  
The assistant gained a basic RAG-style retrieval system using the transaction CSV file.

---

## Artifact 2: Improving RAG Matching for Grocery Questions

**Development issue:**  
The first retrieval output for the question “How can I reduce my grocery spending?” included some unrelated rows such as Entertainment and Utilities. This happened because the retrieval logic did not fully handle the difference between singular and plural words such as `grocery` and `groceries`.

**Prompt summary:**  
I showed the RAG output to AI and asked whether it was correct.

**AI response summary:**  
AI identified that the retrieval fallback was too broad and suggested improving the keyword matching logic. The function was updated to handle simple singular and plural variations.

**Code improvement:**

```python
if word.endswith("ies"):
    search_words.append(word[:-3] + "y")
elif word.endswith("y"):
    search_words.append(word[:-1] + "ies")
elif word.endswith("s"):
    search_words.append(word[:-1])
else:
    search_words.append(word + "s")
```

**Output after improvement:**

```text
Relevant transactions for the user's question:
- 2024-08-01 | Groceries | $45.50 | Woolworths Weekly Shop
- 2024-08-05 | Groceries | $120.00 | Coles Weekly Shop
- 2024-08-12 | Groceries | $156.00 | Woolworths Fortnightly Shop
- 2024-08-18 | Groceries | $73.20 | IGA Local Shop
- 2024-08-25 | Groceries | $145.60 | Monthly Bulk Shopping - Costco
```

**Result:**  
The RAG function became more accurate. For a grocery question, it retrieved only grocery-related transactions.

---

## Artifact 3: Creating the Transaction Context for the Chatbot

**Development issue:**  
The chatbot needed a clear prompt context containing both the overall finance summary and the relevant transactions.

**Prompt summary:**  
I asked AI to help structure the RAG output so it could be used by the chatbot.

**AI response summary:**  
AI helped create `create_transaction_context()`, which produces a readable text block including:
- number of transactions;
- expense transactions;
- refund transactions;
- net expenses;
- refunds identified;
- highest spending category;
- monthly income;
- estimated savings;
- estimated savings rate;
- spending by category;
- relevant transactions for the user’s question.

**Example context output:**

```text
TRANSACTION DATA CONTEXT
----------------------------------------
Number of transactions: 25
Expense transactions: 23
Refund transactions: 2
Net expenses: $1,208.65
Refunds identified: $37.50
Highest spending category: Groceries ($540.30)
Monthly income entered: $2,000.00
Estimated savings: $791.35
Estimated savings rate: 39.57%

Spending by category:
- Groceries: $540.30
- Entertainment: $206.95
- Utilities: $159.80
- Dining: $148.85
- Transport: $108.00
- Coffee: $44.75
```

**Result:**  
The assistant could now create a structured finance context that could be passed into the chatbot prompt.

---

## Artifact 4: hands-on-ai Server Setup Issue

**Development issue:**  
The original hands-on-ai setup/test cell was confusing. It could print a success message even when the response actually contained an error. There was also an API key issue because one character was read incorrectly.

**Prompt summary:**  
I asked AI why the stricter hands-on-ai setup worked differently from the teacher-provided connection test.

**AI response summary:**  
AI explained that the teacher-provided connection test only checked whether the Python function raised an exception. In my case, `get_response()` could return an error string such as `Connection error` or `401 Authorization Required` without raising an exception. This meant the test could incorrectly print a success message.

AI also helped identify that the API key had been entered incorrectly because a `5` had been mistaken for an `S`.

**Corrected setup code:**

```python
import os

os.environ["HANDS_ON_AI_SERVER"] = "https://ollama.locollm.org"
os.environ["HANDS_ON_AI_MODEL"] = "gemma3:4b"
os.environ["HANDS_ON_AI_API_KEY"] = "Curtin2026ISYS20015002"

print("Hands-on-AI configuration:")
print("Server:", os.environ["HANDS_ON_AI_SERVER"])
print("Model:", os.environ["HANDS_ON_AI_MODEL"])
print("API key length:", len(os.environ["HANDS_ON_AI_API_KEY"]))
```

**Result:**  
The hands-on-ai configuration was corrected, and the notebook could use the external chat server.

---

## Artifact 5: Replacing the Local Chatbot with a hands-on-ai Chatbot

**Development issue:**  
The first chatbot version was rule-based and produced a useful response, but I wanted the final project to use the `hands-on-ai` chat system more directly.

**Prompt summary:**  
I asked AI to help replace the local chatbot with a hands-on-ai chatbot that still used my transaction data and RAG context.

**AI response summary:**  
AI helped rewrite `budget_buddy_chatbot()` so that it:
- receives the user question;
- creates CSV-based RAG context;
- builds a prompt for the hands-on-ai model;
- instructs the model to use only the provided transaction data;
- asks for a warm and practical response;
- appends the relevant transaction context below the chatbot response.

**Important code snippet:**

```python
prompt = f"""
You are Budget Buddy, a warm and practical personal finance assistant for students and young adults.

Use ONLY the transaction data and summary provided below. Do not invent extra transactions or figures.

User question:
{user_question}

Transaction and finance context:
{rag_context}

Write a helpful response in 3 to 6 short paragraphs or bullet points.
"""

ai_response = get_response(prompt)
```

**Result:**  
The chatbot became a hands-on-ai powered response while still being grounded in the transaction analysis and CSV retrieval context.

---

## Artifact 6: Gradio Not Updating the Chatbot Response

**Development issue:**  
After replacing the chatbot function, the Gradio interface did not immediately show the new chat response.

**Prompt summary:**  
I asked AI why the UI was not integrating the new chatbot output.

**AI response summary:**  
AI explained that the Gradio app had likely been created before the chatbot function was replaced. In notebooks, replacing a function cell does not automatically update a Gradio interface that is already running. The app needed to be stopped and relaunched after rerunning the updated chatbot and Gradio cells.

**Fix applied:**  
I restarted the notebook/app state and relaunched the Gradio interface.

**Result:**  
The Gradio interface correctly displayed the new hands-on-ai chatbot response.

---

## Reflection

This session showed how RAG improves a chatbot by grounding its answer in actual data. The first retrieval version was not accurate enough because it returned unrelated rows for a grocery question. After improving the matching logic, the context became more relevant and useful.

The hands-on-ai integration also showed that external AI tools need careful testing. A connection test can look successful even when the returned text is actually an error. I learned to check the content of the response, not just whether code runs. I also learned that notebook state matters: after changing a function, the Gradio interface may need to be restarted before it uses the updated function.

Overall, this section connected the project’s transaction analysis to an AI chatbot, making the assistant more useful than a simple calculator or summary table.