# Sales Performance Analysis to Identify Revenue Growth Opportunities

## Objective
This project aims to analyze sales performance to identify key revenue drivers, high-value customers, and growth opportunities.
The goal is to provide actionable insights that can help improve business performance.

## Datasets
The dataset consists of transactional data, including:
Orders
Customers
Products
Order details
These datasets are used to analyze revenue, customer contribution, and product performance.

## SQL Analysis

### 1. Monthly Revenue, Orders and AOV
''' SQL
SELECT
    date_trunc('month', order_date) AS order_month,
    SUM(amount) AS total_revenue,
    COUNT(order_id) AS total_orders,
    ROUND(SUM(amount) / COUNT(order_id), 2) AS avg_order_value
FROM orders
GROUP BY date_trunc('month', order_date)
ORDER BY order_month;

insight: This analysis provides an overview of revenue trends, transaction volume, and customer spending behavior over time.

### 2. Top 5 Customers
''' SQL 
SELECT 
    c.customer_id,
    c.name,
    SUM(o.amount) AS total_spending,
    COUNT(o.order_id) AS total_orders
FROM customers c
JOIN orders o
    ON c.customer_id = o.customer_id
GROUP BY c.customer_id, c.name
ORDER BY total_spending DESC
LIMIT 5;

Insight: Identifies high-value customers who contribute the most to overall revenue.

### 3. Best Selling Products
''' SQL
SELECT 
    p.product_id,
    p.product_name,
    SUM(oi.quantity) AS total_units_sold,
    SUM(oi.quantity * oi.price) AS total_revenue
FROM products p
JOIN order_items oi
    ON p.product_id = oi.product_id
GROUP BY p.product_id, p.product_name
ORDER BY total_units_sold DESC
LIMIT 5;

Insight: Highlights the most frequently purchased products and their revenue contribution.

### 4. Revenue by Category
''' SQL
SELECT
    p.category,
    SUM(oi.quantity) AS total_units_sold,
    SUM(oi.quantity * oi.price) AS total_revenue,
    COUNT(DISTINCT oi.order_id) AS total_orders
FROM products p
JOIN order_items oi
    ON p.product_id = oi.product_id
GROUP BY p.category
ORDER BY total_revenue DESC;

Insight: Compares performance across product categories to identify the most profitable segments.

### 5. Monthly Revenue Growth
''' SQL
WITH monthly_revenue AS (
    SELECT 
        date_trunc('month', order_date) AS order_month,
        SUM(amount) AS total_revenue
    FROM orders
    GROUP BY date_trunc('month', order_date)
)
SELECT
    order_month,
    total_revenue,
    ROUND(
        (total_revenue - LAG(total_revenue) OVER (ORDER BY order_month)) /
        NULLIF(LAG(total_revenue) OVER (ORDER BY order_month), 0) * 100,
        2
    ) AS revenue_growth
FROM monthly_revenue
ORDER BY order_month;

Insight: Measures month-over-month revenue growth to evaluate business performance trends.

## Key Insight
- A small group of customers contributes a significant portion of total revenue, indicating a strong dependency on high-value customers.
- Certain product categories generate high revenue despite lower sales volume, suggesting higher profit margins.
- Revenue shows inconsistent monthly trends, which may indicate seasonality or unstable demand patterns.
- Average order value can be improved by targeting high-spending customer segments.

  
## Conclusion

The analysis shows that business performance is influenced by both customer purchasing behavior and product distribution.
Focusing on high-value customers and optimizing high-performing product categories can help improve overall revenue growth and business performance.

## Business Recommendations
- Focus on retaining high-value customers through loyalty programs or personalized offers.
- Increase marketing efforts on high-performing product categories to maximize revenue.
- Analyze low-performing months to identify potential causes and improve consistency.
- Implement upselling or bundling strategies to increase average order value.

