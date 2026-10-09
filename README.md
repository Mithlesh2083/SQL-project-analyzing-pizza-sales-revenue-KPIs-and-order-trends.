# SQL-project-analyzing-pizza-sales-revenue-KPIs-and-order-trends.
SQL project analyzing pizza sales using Microsoft SQL Server, covering KPIs, revenue trends, order patterns, pizza categories, sizes, and top/bottom-selling pizzas.
Project OverviewThis project provides a comprehensive analysis of pizza sales performance using SQL. The objective is to calculate key performance indicators (KPIs) and extract actionable insights regarding sales trends, category performance, product size distribution, and top/bottom-performing items.   Key Performance Indicators (KPIs)Total Revenue: $817,860.05   Average Order Value: $38.31   Total Pizzas Sold: 49,574   Total Orders: 21,350   Average Pizzas per Order: 2.32   Key Insights & SQL Analysis1. Order TrendsDaily Trend: Peak order volume occurs on Friday (3,538 orders) and Thursday (3,239 orders).   Hourly Trend: Peak order hours are during lunch (12 PM – 1 PM, ~2,500 orders) and dinner (5 PM – 6 PM, ~2,400 orders).   2. Category & Size BreakdownBy Category: Classic generates the highest revenue percentage (26.91%), closely followed by Supreme (25.46%), Chicken (23.96%), and Veggie (23.68%).   By Size: Large (L) size pizzas dominate sales, accounting for 45.89% of total revenue, followed by Medium (30.49%) and Small (21.77%).   3. Best & Worst SellersTop 5 Best Sellers: The Classic Deluxe Pizza (2,453), The Barbecue Chicken Pizza (2,432), The Hawaiian Pizza (2,422), The Pepperoni Pizza (2,418), and The Thai Chicken Pizza (2,371).   Bottom 5 Sellers: The Brie Carre Pizza (490), The Mediterranean Pizza (934), The Calabrese Pizza (937), The Spinach Supreme Pizza (950), and The Soppressata Pizza (961).   SQL Queries   SQL-- 1. Key Performance Indicators
SELECT SUM(total_price) AS Total_Revenue FROM pizza_sales;
SELECT (SUM(total_price) / COUNT(DISTINCT order_id)) AS Avg_order_Value FROM pizza_sales;
SELECT SUM(quantity) AS Total_pizza_sold FROM pizza_sales;
SELECT COUNT(DISTINCT order_id) AS Total_Orders FROM pizza_sales;
SELECT CAST(CAST(SUM(quantity) AS DECIMAL(10,2)) / CAST(COUNT(DISTINCT order_id) AS DECIMAL(10,2)) AS DECIMAL(10,2)) AS Avg_Pizzas_per_order FROM pizza_sales;

-- 2. Daily Trend for Total Orders
SELECT DATENAME(DW, order_date) AS order_day, COUNT(DISTINCT order_id) AS total_orders 
FROM pizza_sales
GROUP BY DATENAME(DW, order_date);

-- 3. Hourly Trend for Orders
SELECT DATEPART(HOUR, order_time) AS order_hours, COUNT(DISTINCT order_id) AS total_orders
FROM pizza_sales
GROUP BY DATEPART(HOUR, order_time)
ORDER BY DATEPART(HOUR, order_time);

-- 4. Percentage of Sales by Pizza Category
SELECT pizza_category, CAST(SUM(total_price) AS DECIMAL(10,2)) AS total_revenue,
CAST(SUM(total_price) * 100 / (SELECT SUM(total_price) FROM pizza_sales) AS DECIMAL(10,2)) AS PCT
FROM pizza_sales
GROUP BY pizza_category;

-- 5. Percentage of Sales by Pizza Size
SELECT pizza_size, CAST(SUM(total_price) AS DECIMAL(10,2)) AS total_revenue,
CAST(SUM(total_price) * 100 / (SELECT SUM(total_price) FROM pizza_sales) AS DECIMAL(10,2)) AS PCT
FROM pizza_sales
GROUP BY pizza_size
ORDER BY pizza_size;

-- 6. Top 5 Best Sellers by Total Pizzas Sold
SELECT TOP 5 pizza_name, SUM(quantity) AS Total_Pizza_Sold
FROM pizza_sales
GROUP BY pizza_name
ORDER BY Total_Pizza_Sold DESC;

-- 7. Bottom 5 Sellers by Total Pizzas Sold
SELECT TOP 5 pizza_name, SUM(quantity) AS Total_Pizza_Sold
FROM pizza_sales
GROUP BY pizza_name
ORDER BY Total_Pizza_Sold ASC;
