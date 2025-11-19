This project simulates a small e-commerce database and uses **SQL + Python** (SQLite + pandas) to answer typical business questions from a **Business Analyst** point of view.

The analysis is implemented in a Google Colab notebook, but can also be run locally with any Python environment that supports SQLite.

---

## 1. Project Overview

**Goal**

Build a minimal but realistic e-commerce dataset and use SQL to analyse:

- Monthly revenue  
- Revenue by marketing channel  
- Revenue by product category  
- Average Order Value (AOV)  
- Share of repeat customers (customers with 2+ completed orders)

**Tech stack**

- Python (Google Colab / Jupyter)  
- SQLite (via `sqlite3`)  
- `pandas` for tabular analysis  
- `matplotlib` for quick charts  

---

## 2. Data Model

The database models a very small online store with the following tables.

### 2.1 Tables

| Table             | Description                                                       | Example key fields                                      |
|-------------------|-------------------------------------------------------------------|---------------------------------------------------------|
| `product_category`| Product categories (e.g. Electronics, Home Office)               | `category_id`, `category_name`                          |
| `product`         | Individual products, brands and base prices                      | `product_id`, `category_id`, `product_name`, `unit_price` |
| `customer`        | Customers and basic attributes                                   | `customer_id`, `full_name`, `country`, `age_group`      |
| `orders`          | Order header: who bought, when, via which marketing channel      | `order_id`, `customer_id`, `order_date`, `status`, `marketing_channel` |
| `order_item`      | Order lines: which products, quantities and unit prices          | `order_item_id`, `order_id`, `product_id`, `quantity`, `unit_price` |
| `payment`         | Payment information: date, amount, method, status                | `payment_id`, `order_id`, `payment_date`, `amount`, `status` |

### 2.2 Relationships

High-level relationships:

| From                | To               | Type         | Notes                              |
|---------------------|------------------|--------------|------------------------------------|
| `customer`          | `orders`         | 1 → N        | One customer can place many orders |
| `orders`            | `order_item`     | 1 → N        | One order can contain many items   |
| `product_category`  | `product`        | 1 → N        | One category can have many products|
| `product`           | `order_item`     | 1 → N        | One product appears in many orders |
| `orders`            | `payment`        | 1 → N (here 1)| One order can have payments (here effectively one) |

---

## 3. Example Insights from the Sample Data

This section summarises what we can already see from the small synthetic dataset included in the project.

### 3.1 Monthly revenue

Only payments with status `paid` are counted as revenue.

| Month   | Revenue (EUR) |
|---------|---------------|
| 2024-03 | 780.00        |
| 2024-04 | 89.00         |

> **Insight:** Most revenue in this sample comes from March 2024, with a smaller contribution in April.

---

### 3.2 Revenue by marketing channel

Joining `payment` and `orders` and keeping only `paid` payments:

| Marketing channel | Revenue (EUR) |
|-------------------|--------------:|
| Email             | 477.00        |
| Organic           | 288.00        |
| Paid Search       | 104.00        |

> **Insight:** In this dataset, Email campaigns drive the highest revenue, followed by Organic traffic and then Paid Search.

---

### 3.3 Revenue by product category

Using completed orders only (`orders.status = 'completed'`), joining `orders`, `order_item`, `product`, and `product_category`:

| Product category | Revenue (EUR) |
|------------------|--------------:|
| Home Office      | 598.00        |
| Electronics      | 271.00        |
| Sports & Fitness | 0.00          |

> **Insight:** Home Office products dominate revenue in this sample, while the Sports & Fitness category has no completed-order revenue yet (only a cancelled order).

---

### 3.4 Average Order Value (AOV)

Considering only completed orders:

- Completed orders: 4  
- Order totals: 104.00, 199.00, 477.00, 89.00 EUR  
- **Average Order Value (AOV): 217.25 EUR**

> **Insight:** On average, each completed order generates around **€217** in revenue.

---

### 3.5 Repeat customers

Definition: customers with **2 or more completed orders** divided by all customers with at least one completed order.

- Customers with at least one completed order: 3  
- Customers with 2+ completed orders: 1  
- **Repeat rate:** ~33.3%

> **Insight:** One out of three customers in this sample made more than one completed purchase, indicating early signs of repeat behaviour.

---

## 4. How to Run

- Open the notebook (`E_commerce_Analytics.ipynb`) in **Google Colab** or Jupyter.  
- Run the cells in order:
  1. Setup (imports + SQLite connection)  
  2. Schema creation  
  3. Sample data insertion  
  4. Business analysis queries and charts  
