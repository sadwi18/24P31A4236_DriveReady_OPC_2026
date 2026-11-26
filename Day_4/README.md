# Day 4 — Tools, Structured Output & Google Sheets (ShopSphere)

## Overview
This folder contains the Day 4 implementation for the **Drive Ready – AI Agents | One Person Company (OPC) Initiative** at Aditya University. It upgrades the **ShopSphere Online Store** assistant into a tool-using agent built with `create_agent`, adds Pydantic schema validation (`with_structured_output`), and logs confirmed customer orders directly to **Google Sheets** via `gspread`.

## Files in This Folder
- `task.py`: Complete Day 4 implementation including `@tool` functions, Pydantic `ShopSphereOrder` extraction, Google Sheets order logging, docstring break-test, and agent trace inspection.
- `README.md`: Documentation of setup, architecture, and Day 4 findings.

## Setup & Execution
1. Run in **Google Colab** with `GROQ_API_KEY` saved in **Colab Secrets**.
2. Install dependencies (`langchain`, `langchain-groq`, `gspread`, `pydantic`) and authenticate your Google account when prompted by `auth.authenticate_user()`.
3. Execute `task.py` (or run the cells sequentially) and verify new order rows in the **ShopSphere Orders** spreadsheet.

## Key Tasks & Observations
1. **ShopSphere `@tool` Definitions**
   - Converted `get_product_price` and `check_stock` into LangChain `@tool` functions with clear snake_case names, type hints, docstrings, and helpful fallback messages for unknown items or invalid quantities.

2. **Structured Output with Pydantic (`ShopSphereOrder`)**
   - Defined `ShopSphereOrder` (`customer_name`, `product` via `Literal`, `qty >= 1`, `city`, `cod`) and verified `llm.with_structured_output(ShopSphereOrder)` across 5 messy customer messages.

3. **Google Sheets Persistence (`log_order`)**
   - Connected Colab to Google Sheets using `gspread` and built `log_order`, which validates fields with Pydantic before appending a clean row (`customer_name`, `product`, `qty`, `city`, `cod`, `total`) to `ShopSphere Orders`.

4. **Docstring Break-Test (`tool1`)**
   - Temporarily renamed `check_stock` to `tool1` with the vague docstring `"Does stuff."`, confirming how unclear tool names and descriptions degrade tool selection compared to explicit docstrings.

5. **Agent Trace Analysis (`pretty_print()`)**
   - Inspected `result["messages"]` to count model rounds and tool invocations, verifying that the agent asks for missing order details before calling `log_order`.
