# 24P31A4236 — Drive Ready: AI Agents | One Person Company (OPC) Initiative

**Department of Technical Training, Aditya University**  
**Project:** E-Commerce Customer Support Bot — **ShopSphere**

This repository contains my daily learning materials, Python implementations, practical exercises, and experiments built as part of the **Drive Ready – AI Agents (OPC) Training Program**. The training focuses on understanding and building AI-powered applications using Large Language Models (LLMs), prompt engineering, memory, evaluation, and agent-related concepts.

## OPC Use Case: E-Commerce Customer Support Bot (ShopSphere)
The project focuses on developing an AI-powered customer support assistant for an e-commerce platform, **ShopSphere Online Store**, designed around common customer support scenarios including:
- Order tracking and structured order extraction
- Returns and refunds
- Product availability and pricing
- Customer FAQs and escalation guardrails

## Repository Structure
24P31A4236_DriveReady_OPC_2026/
├── Day_1/
│   ├── day1_shopsphere.ipynb
│   └── README.md
├── Day_2/
│   ├── day2_shopsphere.ipynb
│   └── README.md
├── Day_3/
│   ├── day3_shopsphere.ipynb
│   ├── tests.csv
│   └── README.md
└── README.md

## Daily Learning & Implementations

### [Day 1 – LLM Fundamentals and First API Call](./Day_1)
- **Topics Covered:**
  - Introduction to Large Language Models (LLMs).
  - Connecting Python applications with LLM APIs (openai/gpt-oss-20b via Groq).
  - System prompts, user messages, and temperature configuration (0.2 vs. 1.2).
  - Grounding, hallucination prevention, multi-turn message memory, and token comparisons (English vs. Telugu).
- **Practical Implementation:** ShopSphere E-Commerce Customer Support Bot system prompt and baseline experiments.

### [Day 2 – Prompts, Memory and Python Concepts](./Day_2)
- **Topics Covered:**
  - Structured prompt engineering (ROLE, FACTS, RULES, FORMAT).
  - Few-shot intent classification and structured JSON order extraction.
  - Python concepts for AI agents: type hints and logging decorators (@log_call).
  - Multi-turn conversation history trimming and try / except API error handling.
- **Practical Implementation:** ShopSphere intent classifier, JSON parser, pricing helper, and resilient chat loop.

### [Day 3 – LangChain Assistant, CSV Evaluation & Agent Prep](./Day_3)
- **Topics Covered:**
  - LangChain integration (ChatPromptTemplate, MessagesPlaceholder, ChatGroq, StrOutputParser).
  - Automated CSV test evaluation (tests.csv), prompt tuning, and temperature=1.0 variance checks.
  - Agent vs. non-agent workflow classification and tool-ready Python functions (get_product_price, check_stock).
- **Practical Implementation:** LangChain-based ShopSphere support assistant and 10-question evaluation suite.

## Technologies & Tools
- Python
- LangChain (langchain, langchain-groq, langchain-core)
- Groq API (openai/gpt-oss-20b)
- Large Language Models (LLMs)
- Google Colab (with Colab Secrets for GROQ_API_KEY)
- Prompt Engineering & Automated Evaluation

## Learning Objectives
- Understand the fundamentals of LLMs and AI agents.
- Learn to integrate LLM APIs and LangChain with Python applications.
- Develop effective, grounded prompts and evaluate responses systematically.
- Understand how memory and error handling contribute to conversational AI.
- Progress toward developing an intelligent, tool-enabled e-commerce customer support assistant.

---

*This repository serves as a record of daily learning, hands-on implementations, experiments, and progress throughout the Drive Ready AI Agents training program.*

**Learn. Build. Experiment. Grow.**


================================================================================
FILE 2: Day_2/README.md
================================================================================
# Day 2 — Structured Prompts, JSON Extraction, Decorators & Safe Chat Loop (ShopSphere)

## Overview
This folder contains the Day 2 implementation for the **Drive Ready – AI Agents | One Person Company (OPC) Initiative** at Aditya University. It extends the **ShopSphere Online Store** e-commerce customer support assistant with structured prompt engineering, few-shot intent classification, JSON extraction, Python decorators with type hints, conversation history trimming, and API error handling.

## Files in This Folder
- `day2_shopsphere.ipynb`: Google Colab notebook implementing Day 2 patterns using the Groq API (`openai/gpt-oss-20b`).
- `README.md`: Documentation of setup, components, and implementation notes.

## Setup & Execution
1. Open `day2_shopsphere.ipynb` in Google Colab.
2. Add your Groq API key in **Colab Secrets** under the name `GROQ_API_KEY` (no API keys are hardcoded in the notebook).
3. Run all cells sequentially from top to bottom.

## Implemented Components & Key Observations
1. **Structured System Prompt (`ROLE`, `FACTS`, `RULES`, `FORMAT`)**
   - Organized the ShopSphere catalog (Wireless Earbuds: Rs. 1499, Smartwatch: Rs. 2499, Laptop Backpack: Rs. 999), 7-day return policy, 3–5 day shipping rules, payment methods, escalation guidelines, and response format limits.

2. **Few-Shot Intent Classification**
   - Used labeled `user` / `assistant` examples to classify customer queries into `order_inquiry`, `return_request`, or `product_question`.
   - Adjusted `max_tokens` (`200`) to ensure reasoning models finish internal reasoning before outputting the final label (`return_request`).

3. **Structured JSON Extraction**
   - Extracted order details (`product`, `qty`, `note`) from natural-language customer messages and safely stripped markdown formatting before parsing into a Python dictionary with `json.loads()`.

4. **Python Functions, Type Hints & Logging Decorator (`@log_call`)**
   - Implemented type-annotated `get_item_price(item: str, qty: int = 1)` wrapped with a `@log_call` decorator to log function invocations and arguments cleanly.

5. **Safe Multi-Turn Chat Loop with History Trimming**
   - Retained the system prompt while trimming older conversation turns (`MAX_TURNS = 4`) to control token growth, and wrapped API calls in `try / except` blocks (`RateLimitError`, `APIConnectionError`, and general exceptions).


================================================================================
FILE 3: Day_3/tests.csv
================================================================================
question,must_include
What is the price of wireless earbuds?,1499
How much do 2 laptop backpacks cost?,1998
Can you guarantee delivery in 2 hours?,check
What is your return policy?,7 days
Do you sell gaming chairs?,check
What is the price of a smartwatch?,2499
Can I order 3 wireless earbuds?,4497
Do you offer international shipping?,check
What is the cost of 1 backpack and 1 smartwatch?,3498
Are orders cash on delivery available?,check


================================================================================
FILE 4: Day_3/day3_shopsphere.ipynb (Cells 1 to 6)
================================================================================
# --- CELL 1 ---
!pip install -q langchain langchain-groq langchain-core pandas

import os
import pandas as pd
from google.colab import userdata
from langchain_core.messages import AIMessage, HumanMessage
from langchain_core.output_parsers import StrOutputParser
from langchain_core.prompts import ChatPromptTemplate, MessagesPlaceholder
from langchain_groq import ChatGroq

os.environ["GROQ_API_KEY"] = userdata.get("GROQ_API_KEY")

MODEL = "openai/gpt-oss-20b"

# --- CELL 2 ---
SHOPSPHERE_BUSINESS = """
ShopSphere Online Store sells wireless earbuds (Rs. 1499), smartwatch (Rs. 2499), and laptop backpack (Rs. 999).
Returns: 7-day return window for unused items. Shipping: 3-5 business days; free on Rs. 1500+.
Payments: UPI, cards, COD. Never guarantee unconfirmed delivery times or unverified policies; say you will check with the support team.
Calculate totals accurately. Keep replies concise (under 50 words).
""".strip()

SYSTEM_TEMPLATE = """
You are the customer support assistant for {business}
Use only the provided business details. If unsure or asked about unconfirmed promises/out-of-stock items, say you will check with the support team.
""".strip()

prompt = ChatPromptTemplate.from_messages([
    ("system", SYSTEM_TEMPLATE),
    MessagesPlaceholder(variable_name="history"),
    ("human", "{question}"),
])


def build_chain(temperature: float = 0.2):
    llm = ChatGroq(
        model=MODEL,
        temperature=temperature,
        max_tokens=300,
    )
    return prompt | llm | StrOutputParser()


chain = build_chain(temperature=0.2)
chat_history = []


def ask(question: str, history: list | None = None, active_chain=chain) -> str:
    hist = history if history is not None else []
    answer = active_chain.invoke({
        "business": SHOPSPHERE_BUSINESS,
        "history": hist,
        "question": question,
    }).strip()
    if history is not None:
        history.append(HumanMessage(content=question))
        history.append(AIMessage(content=answer))
    return answer


print("Turn 1:", ask("How much is a smartwatch?", chat_history))
print("Turn 2:", ask("What if I buy 1 of those and 1 laptop backpack?", chat_history))

# --- CELL 3 (Fetch tests.csv from GitHub first if not local) ---
!wget -q https://raw.githubusercontent.com/sadwi18/24P31A4236_DriveReady_OPC_2026/main/Day_3/tests.csv -O tests.csv


def run_tests(csv_path: str = "tests.csv", active_chain=chain) -> dict:
    df = pd.read_csv(csv_path)
    passed = 0
    results = []

    for idx, row in df.iterrows():
        q = str(row["question"]).strip()
        expected = str(row["must_include"]).strip().lower()
        reply = ask(q, history=[], active_chain=active_chain)
        is_pass = expected in reply.lower().replace(",", "")
        if is_pass:
            passed += 1
        results.append({
            "id": idx + 1,
            "question": q,
            "must_include": row["must_include"],
            "passed": is_pass,
            "reply": reply,
        })

    total = len(df)
    pass_rate = (passed / total) * 100 if total else 0.0
    for r in results:
        status = "PASS" if r["passed"] else "FAIL"
        print(f"[{status}] Q{r['id']}: {r['question']} -> {r['reply']}")

    print(f"\nScore: {passed}/{total} ({pass_rate:.1f}%)")
    return {"passed": passed, "total": total, "pass_rate": pass_rate, "results": results}


baseline_report = run_tests("tests.csv", active_chain=chain)

# --- CELL 4 ---
IMPROVED_BUSINESS = SHOPSPHERE_BUSINESS.replace(
    "say you will check with the support team.",
    "explicitly state that you will check with the support team.",
)

improved_chain = build_chain(temperature=0.2)


def ask_improved(question: str, history: list | None = None, active_chain=improved_chain) -> str:
    hist = history if history is not None else []
    answer = active_chain.invoke({
        "business": IMPROVED_BUSINESS,
        "history": hist,
        "question": question,
    }).strip()
    if history is not None:
        history.append(HumanMessage(content=question))
        history.append(AIMessage(content=answer))
    return answer


def run_improved_tests(csv_path: str = "tests.csv") -> dict:
    df = pd.read_csv(csv_path)
    passed = 0
    for _, row in df.iterrows():
        reply = ask_improved(str(row["question"]).strip(), history=[])
        if str(row["must_include"]).strip().lower() in reply.lower().replace(",", ""):
            passed += 1
    total = len(df)
    rate = (passed / total) * 100 if total else 0.0
    print(f"Improved Prompt Score: {passed}/{total} ({rate:.1f}%)")
    return {"passed": passed, "total": total, "pass_rate": rate}


improved_report = run_improved_tests("tests.csv")
print(
    f"Comparison -> Baseline: {baseline_report['pass_rate']:.1f}% | "
    f"Improved: {improved_report['pass_rate']:.1f}%"
)

# --- CELL 5 ---
temp1_chain = build_chain(temperature=1.0)

for run_num in (1, 2):
    print(f"\n=== Temperature 1.0 — Run {run_num} ===")
    df = pd.read_csv("tests.csv")
    passed = 0
    for idx, row in df.iterrows():
        q = str(row["question"]).strip()
        expected = str(row["must_include"]).strip().lower()
        reply = temp1_chain.invoke({
            "business": IMPROVED_BUSINESS,
            "history": [],
            "question": q,
        }).strip()
        ok = expected in reply.lower().replace(",", "")
        passed += int(ok)
        print(f"Q{idx+1} ({'PASS' if ok else 'FAIL'}): {reply}")
    print(f"Run {run_num} Pass Rate: {passed}/{len(df)} ({(passed/len(df))*100:.1f}%)")

# --- CELL 6 ---
AGENT_DECISIONS = {
    "calculate_cart_subtotal": "Plain code — deterministic arithmetic on exact catalog prices and quantities.",
    "write_festive_sale_caption": "One LLM call — creative copywriting from a single prompt with no external state or tools.",
    "resolve_bundle_stock_and_order": (
        "Agent — multi-step workflow that checks live stock, computes bundle pricing, "
        "verifies return eligibility, and calls a tool/API to log an order request."
    ),
}

STOCK = {
    "wireless earbuds": 18,
    "smartwatch": 7,
    "laptop backpack": 25,
}

PRICES = {
    "wireless earbuds": 1499,
    "smartwatch": 2499,
    "laptop backpack": 999,
}


def get_product_price(product_name: str, quantity: int = 1) -> int | str:
    """Calculate total price in INR for one or more ShopSphere products.

    Args:
        product_name: Name of the product in the ShopSphere catalog.
        quantity: Number of units to price (must be >= 1).
