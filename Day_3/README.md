# Day 3 — LangChain Assistant, Automated CSV Evaluation & Agent Prep (ShopSphere)

## Overview
This folder contains the Day 3 implementation for the **Drive Ready – AI Agents | One Person Company (OPC) Initiative** at Aditya University. It builds a LangChain conversational assistant for **ShopSphere Online Store**, evaluates it against a 10-question CSV test suite (`tests.csv`), analyzes prompt iteration and temperature variance (`1.0`), classifies agent vs. non-agent workflows, and defines tool-ready Python functions for Day 4.

## Files in This Folder
- `day3_shopsphere.ipynb`: Google Colab notebook implementing the LangChain chain, CSV test runner, prompt refinement, temperature `1.0` runs, agent analysis, and tool-ready functions.
- `tests.csv`: 10 e-commerce evaluation questions with `must_include` assertions covering pricing, totals, return policy, and escalation behavior.
- `README.md`: Documentation of setup, test workflow, and key findings.

## Setup & Execution
1. Open `day3_shopsphere.ipynb` in Google Colab and load `tests.csv` into the working directory (e.g., via `!wget` from this repository).
2. Store your Groq API key in **Colab Secrets** under the name `GROQ_API_KEY` (no API keys are hardcoded in the notebook).
3. Run all cells sequentially from top to bottom.

## Key Tasks & Observations
1. **LangChain Prompt & Multi-Turn Memory**
   - Built a modular chain using `ChatPromptTemplate`, `MessagesPlaceholder("history")`, `ChatGroq` (`openai/gpt-oss-20b`), and `StrOutputParser()` with a `{business}` context block.

2. **Automated CSV Evaluation (`run_tests`)**
   - Evaluated 10 ShopSphere queries (`question,must_include`) with fresh conversation history per row and calculated overall pass rates.
   - Observed how strict substring checks (`must_include`) can penalize valid synonyms or punctuation differences (such as `"7-day"` vs. `"7 days"`, or `"confirm with the support team"` vs. `"check"`).

3. **Prompt Tuning & Temperature `1.0` Variance**
   - Refined the business prompt and compared pass rates against the baseline, noting how stricter phrasing and short word limits affect exact keyword matches.
   - Ran the evaluation suite twice at `temperature=1.0` to observe response variability and pass-rate consistency compared to `temperature=0.2`.

4. **Agent vs. Non-Agent Classification**
   - Contrasted **plain code** (deterministic cart subtotal math), a **single LLM call** (festive sale caption generation), and a **multi-step agent** (checking stock, calculating bundle totals, verifying return rules, and logging an order).

5. **Day 4 Tool-Ready Functions**
   - Implemented type-annotated `get_product_price(product_name: str, quantity: int = 1)` and `check_stock(product_name: str)` with clear docstrings ready for tool binding.
