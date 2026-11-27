# Day 5 — RAG & Conversational Memory (ShopSphere)

## Overview
Day 5 implements retrieval-augmented policy lookup and per-user conversation memory for the ShopSphere customer support assistant using ChromaDB, LangChain, LangGraph, and Groq.

## Key Features
- **ChromaDB Knowledge Base:** Stores 12 ShopSphere store policies and FAQs in the `shopsphere_faq` collection.
- **FAQ Retrieval Tool (`search_faq`):** Queries relevant store policies for policy and FAQ questions.
- **Agent & Action Tools:** Combines `search_faq` with `get_product_info`, `track_order`, and `log_return`.
- **Conversational Memory:** Uses `InMemorySaver` and `thread_id` to maintain separate multi-turn state across users.
- **Validation Suite (`tests.csv`):** Includes baseline test cases for policy lookups and fallback responses.

## Folder Contents
- Day 5 Colab notebook / Python script
- `tests.csv`
- `README.md`
