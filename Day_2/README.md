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
