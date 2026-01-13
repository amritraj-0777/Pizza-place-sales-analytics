# Pizza Place Sales Analytics & Automation

This project delivers an end-to-end analytics and automation solution for a pizza restaurant using SQL Server, Microsoft Forms, Power Automate, and Tableau.

## Executive Summary

The restaurant had over 21,350 orders, 49,574 pizzas sold, and total revenue of \$817,860 but limited visibility into demand patterns, product performance, and peak operating hours. This solution combines a relational SQL database and interactive Tableau dashboards to answer key business questions such as when customers buy the most, which categories drive the most revenue, and how demand varies by size, time, and category.

Key insights include:
- Clear monthly seasonality, with revenue peaking around \$72,558 in July and dipping to roughly \$64,028 in October.
- Lunch and early evening peaks in hourly demand (12:00–14:00 and 17:00–19:00).
- Classic pizzas generating over \$220k and ~26.9% of total revenue as the most profitable category.
- Top 5 pizzas each contributing about \$30k–\$45k in revenue.

These insights support decisions on staffing, inventory planning, pricing, promotions, and menu optimization.

## Architecture & Data Flow

The solution implements an automated pipeline:

1. **Microsoft Forms**  
   - Front-end “Pizza Place Order” form for capturing customer orders.

2. **Power Automate**  
   - Flow triggers on new form submission and inserts data into SQL Server in real time.

3. **Azure SQL Database**  
   - Core tables: `orders`, `order_details`, `pizzas`, `pizza_types`.  
   - Additional `customer_orders` learning table used as the target for automated inserts from Forms.  
   - Enforces business rules around order IDs, pizza IDs, sizes, categories, pricing, and data quality.

4. **Tableau Dashboards**  
   - Connect to the SQL database for live/refreshable reporting.  
   - KPI dashboard and storyboard for management and operational users.

## Use Cases

**Business Managers (Strategic)**  
- Track monthly revenue trends for budgeting and seasonal promotions.  
- Evaluate product and category performance (e.g., Classic vs Veggie vs Supreme) for menu engineering.  
- Plan staffing using hourly demand charts.  
- Monitor KPIs such as total revenue, total orders, average order value, and average quantity per order.

**Operational Staff (Day-to-Day)**  
- Prepare inventory based on category/time-of-day demand patterns.  
- Monitor real-time order volume to manage kitchen workload.  
- Use top-selling pizzas for upselling and recommendations.  
- Reduce waste and stockouts by aligning prep with observed demand.

## Database Overview

- `orders`: one record per order (order header, date, time, store, customer).  
- `order_details`: line items per order with `pizza_id` and `quantity`.  
- `pizzas`: SKU-level data (size, price, pizza_id).  
- `pizza_types`: pizza name, category, ingredients.  
- `customer_orders`: learning table populated automatically from Microsoft Forms via Power Automate.

Business rules include:
- Strict referential integrity between orders, order_details, pizzas, and pizza_types.
- One size per pizza, with size + type uniquely identifying a pizza.
- LineAmount = Quantity × UnitPrice stored in order_details; order TotalAmount is the sum of its LineAmount values.
- Time modeled at date and hour level for trend and peak analysis.

## Live Assets

- **Pizza Sales Dashboard (Tableau):**  
  Main KPI dashboard with monthly trends, category performance, and hourly demand.  
  Link: https://public.tableau.com/app/profile/amrit.raj3866/viz/FinalProject_510_17649706830990/PizzaSalesDashboard?publish=yes] 

- **Pizza Sales Storyboard (Tableau):**  
  Step-by-step narrative view for stakeholders, walking through key insights.  
  Link: https://public.tableau.com/app/profile/amrit.raj3866/viz/FinalProject_510_17649706830990/PizzaSalesOverviewStory?publish=yes
  
- **Pizza Order Form (Microsoft Forms):**  
  Front-end form integrated with Power Automate and Azure SQL (`customer_orders` table) to capture new orders in real time.  
  Link: https://forms.microsoft.com/pages/responsepage.aspx?id=W9229i_wGkSZoBYqxQYL0r7haT6USN9PtLfZdI4CFWpUQUI5M1dOWVk1NFZMSzVLN0swQ1JWQzY1VC4u&route=shorturl

These dashboards present:
- Monthly sales and order trends.  
- Revenue by category, size, and pizza.  
- Hourly demand patterns.  
- Key KPIs at the top of the view.

## Data Source

Original dataset: [Pizza Sales Dataset (Kaggle)](https://www.kaggle.com/datasets/aaditya555/pizza-sales-dataset)

The dataset was loaded into Azure SQL and extended with additional structures for forms-based order capture and automation.

## Proof of Concept

The POC validates:
- End-to-end integration from Forms → Power Automate → SQL → Tableau.
- Successful real-time insertion of orders with no failed flow runs during testing.
- Correct mapping and storage of key fields (customer name, pizza_id, quantity, timestamps).
- Accurate Tableau updates after refresh.

The architecture is ready for scaling to additional stores or locations with minimal changes.

## Disclaimer

All data modeling and dashboards are created for educational and demonstration purposes and are not production-hardened. Credentials and sensitive connection details are not included in this repository.
