# E-commerce Analytics with SQL & Python

This project simulates a small e-commerce database and uses **SQL + Python (SQLite + pandas)** to answer typical business questions from a Business Analyst point of view.

The analysis is implemented in a Google Colab notebook, but can also be run locally with any Python environment that supports SQLite.

---

## 1. Project Overview

**Goal:**  
Build a minimal but realistic e-commerce dataset and use SQL to analyse:

- Monthly revenue
- Revenue by marketing channel
- Revenue by product category
- Average Order Value (AOV)
- Share of repeat customers (customers with 2+ completed orders)

**Tech stack:**

- Python (Google Colab / Jupyter)
- SQLite (via `sqlite3`)
- `pandas` for tabular analysis
- `matplotlib` for quick charts

---

## 2. Data Model

The database models a very small online store with the following tables:

- `product_category` – product categories (e.g. Electronics, Home Office)
- `product` – individual products, brands and base prices
- `customer` – customers and basic attributes (country, gender, age group)
- `orders` – order header (who bought, when, which marketing channel)
- `order_item` – order lines (products, quantities and unit prices at purchase time)
- `payment` – payment information (date, amount, method, status)

High-level relationships:

- One **customer** can place many **orders**
- One **order** can contain many **order_items**
- One **product_category** can have many **products**
- One **product** can appear in many **order_items**
- One **order** can have one or more **payments** (in this dataset, effectively one)

---
