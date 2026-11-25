# ShopSphere AI — Drive Ready: AI Agents (OPC)

24P31A4236 • Department of Technical Training, Aditya University

An incremental engineering log building an AI-powered e-commerce customer support assistant for ShopSphere Online Store—evolving from raw LLM API calls to structured prompts, automated evaluation, and tool-ready agents.

---

## Use Case: ShopSphere Support Assistant
Designed to handle storefront queries accurately without hallucinating store policies:
- Catalog & Pricing: Wireless Earbuds (Rs. 1499), Smartwatch (Rs. 2499), Laptop Backpack (Rs. 999), and bundle totals
- Policies & Ops: 7-day returns, 3–5 business day shipping (free on Rs. 1500+), UPI / Cards / COD
- Guardrails: Grounded responses with escalation to support for unverified promises or out-of-stock items

## Repository Map
```
24P31A4236_DriveReady_OPC_2026/
├── Day_1/   # Groq API basics, temperature tuning, policy grounding & memory
├── Day_2/   # Structured prompts, JSON extraction, @log_call & safe chat loop
├── Day_3/   # LangChain pipeline, tests.csv evaluation & tool-ready functions
└── README.md
```

## Progression Log
- Day 1 — Foundations: Built baseline Groq calls (openai/gpt-oss-20b), tested temperature (0.2 vs. 1.2), grounded store rules, and compared token usage across prompts.
- Day 2 — Structure & Reliability: Designed ROLE / FACTS / RULES / FORMAT prompts, few-shot intent routing, JSON order parsing, @log_call decorators, history trimming, and try / except handling.
- Day 3 — LangChain & Evaluation: Built the LCEL chain (ChatPromptTemplate, MessagesPlaceholder, ChatGroq, StrOutputParser), ran a 10-case tests.csv suite, checked temperature=1.0 variance, and wrote get_product_price and check_stock.

## Stack & Security
- Tech: Python • LangChain • Groq API (openai/gpt-oss-20b) • Google Colab
- Credentials: Stored in Colab Secrets (GROQ_API_KEY) with zero hardcoded keys

---

Learn. Build. Experiment. Grow.
