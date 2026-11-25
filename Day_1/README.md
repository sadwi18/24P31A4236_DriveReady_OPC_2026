# Day 1 — E-Commerce Customer Support Assistant (ShopSphere)

## Overview
This folder contains the Day 1 implementation for the **Drive Ready – AI Agents | One Person Company (OPC) Initiative** at Aditya University. The project adapts foundational LLM API experiments to **ShopSphere Online Store**, an e-commerce customer support use case.

## Files in This Folder
- `day1_shopsphere.ipynb`: Google Colab notebook implementing Groq API calls (`openai/gpt-oss-20b`) and five core LLM behavior experiments for ShopSphere.
- `README.md`: Documentation of setup, experiments, and key observations.

## Setup & Execution
1. Open `day1_shopsphere.ipynb` in Google Colab.
2. Add your Groq API key in **Colab Secrets** under the name `GROQ_API_KEY` (no API keys are hardcoded in the notebook).
3. Run all cells sequentially.

## Experiments & Key Observations
1. **Temperature Comparison (`0.2` vs. `1.2`)**
   - Tested slogan generation for ShopSphere across three runs at each setting.
   - `temperature=0.2` produced consistent, focused slogans; `temperature=1.2` generated noticeably more varied and creative phrasing.

2. **Hallucination & Grounding Test**
   - Without store policies in the system prompt, the model risked guessing or generalizing delivery availability.
   - Adding explicit shipping facts and an escalation rule ("state that you will check with the support team") prevented fabricated guarantees.

3. **Stateless Calls vs. Conversation Memory**
   - Separate API calls do not retain user context across turns (`"What is my name?"` failed when sent alone).
   - Passing prior `user` and `assistant` turns inside a single `messages` list enabled accurate multi-turn recall.

4. **Token Usage Comparison (English vs. Telugu)**
   - Compared `usage.prompt_tokens` for identical customer queries in English and Telugu.
   - Non-English scripts required more prompt tokens for the same semantic query.

5. **ShopSphere E-Commerce System Prompt**
   - Configured a structured `system` prompt containing **5 store facts** (product catalog & prices, 7-day return window, 3–5 day shipping & free shipping threshold, accepted payment methods, support hours) and **3 strict rules** (fact-only answers, under-60-word clarity, and support escalation for unknown or order-specific queries).
   - Verified accurate responses across pricing, return eligibility, and out-of-scope order tracking queries.
