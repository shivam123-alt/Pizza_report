# Pizza_report
## Table of Contents
- Problem Statement
- Data Analysis using MySQL
- Data Cleaning
- Build Dashboard or a Report using Tableau
- Tools, Software and Libraries
- References
## Problem Statement
## KPI’s REQUIREMENT
We need to analyze key indicators for our pizza sales data to gain insights into our business performance. Specifically, we want to calculate the following metrics:

1. Total Revenue: The sum of the total price of all pizza orders.
2. Average Order Value: The average amount spent per order, calculated by dividing the total revenue by the total number of orders.
3. Total Pizzas Sold: The sum of the quantities of all pizzas sold.
4. Total Orders: The total number of orders placed.
5. Average Pizzas Per Order: The average number of pizzas sold per order, calculated by dividing the total number of pizzas sold by the total number of orders.

## CHARTS REQUIREMENT
We would like to visualize various aspects of our pizza sales data to gain insights and understand key trends. We have identified the following requirements for creating charts:
1. Hourly Trend for Total Pizzas Sold: Create a stacked bar chart that displays the hourly trend of total orders over a specific time period. This chart will help us identify any patterns or fluctuations in order volumes on a hourly basis.
2. Weekly Trend for Total Orders: Create a line chart that illustrates the weekly trend of total orders throughout the year. This chart will allow us to identify peak weeks or periods of high order activity.
3. Percentage of Sales by Pizza Category: Create a pie chart that shows the distribution of sales across different pizza categories. This chart will provide insights into the popularity of various pizza categories and their contribution to overall sales.
4. Percentage of Sales by Pizza Size: Generate a pie chart that represents the percentage of sales attributed to different pizza sizes. This chart will help us understand customer preferences for pizza sizes and their impact on sales.
5. Total Pizzas Sold by Pizza Category: Create a funnel chart that presents the total number of pizzas sold for each pizza category. This chart will allow us to compare the sales performance of different pizza categories.
6. Top 5 Best Sellers by Revenue, Total Quantity and Total Orders: Create a bar chart highlighting the top 5 best-selling pizzas based on the Revenue, Total Quantity, Total Orders. This chart will help us identify the most popular pizza options.
7. Bottom 5 Best Sellers by Revenue, Total Quantity and Total Orders: Create a bar chart showcasing the bottom 5 worst-selling pizzas based on the Revenue, Total Quantity, Total Orders. This chart will enable us to identify underperforming or less popular pizza options.

## Data Analysis using MySQL
Utilized MySQL for data extraction and calculation of key metrics such as Total Revenue, Average Order Value, Total Pizzas Sold, Total Orders, and Average Pizzas Per Order.

## DATA IMPORT 
<img width="770" height="213" alt="2025-07-23 (2)" src="https://github.com/user-attachments/assets/70935572-1c38-475f-bf8c-c6ba35b66d27" />
<img width="767" height="621" alt="2025-07-23 (4)" src="https://github.com/user-attachments/assets/def8b9fa-ad0d-494b-9efd-c5ecaaf5071a" />
<img width="767" height="621" alt="2025-07-23 (4)" src="https://github.com/user-attachments/assets/a51b5ca4-f398-4dd4-a027-bab23eb63008" />
<img width="491" height="303" alt="2025-07-23 (5)" src="https://github.com/user-attachments/assets/185b4189-adce-46c1-9443-5a9824460603" />

## ANALYSIS OF DIFFERENT SQL STATEMENT ON DATA BASE
A. KPI’s
  1. Total Revenue:
     SELECT SUM(total_price)  AS Total_Revenue FROM pizza_sales;
     
  2. Average Order Value:
       SELECT SUM(total_price) / COUNT(DISTINCT order_id)  AS Avg_Order_Value
       FROM pizza_sales;
  3. Total Pizza Sold:
       SELECT SUM(quantity) AS Total_Pizza_Sold FROM pizza_sales;
  4. Total Order:
      SELECT COUNT(DISTINCT order_id) AS Total_Orders FROM pizza_sales;
  5. Average Pizza Per Order:
      SELECT CAST(SUM(quantity) AS DECIMAL(10,2)) / 
      CAST(COUNT(DISTINCT order_id) AS DECIMAL (10,2))
      AS Avg_Pizza_Per_Order FROM pizza_sales;
B.Hourly Trend For Pizza Sold
       SELECT HOUR(order_time) AS order_hour,
       SUM(quantity) AS Total_Pizza_Sold
       FROM pizza_sales
       GROUP BY HOUR(order_time)
       ORDER BY HOUR(order_time);
C.Weekly Trend For Orders 
      SELECT WEEK(STR_TO_DATE(order_date, '%m/%d/%Y'), 3) AS week_number,
      DATE_FORMAT(STR_TO_DATE('1/1/2015', '%m/%d/%Y'), '%x') AS order_year, 
      COUNT(DISTINCT order_id) AS total_orders
      FROM pizza_sales
      GROUP BY week_number, order_year
      ORDER BY week_number, order_year;

 D.	Percentage Of Sales by Pizza Category
     SELECT pizza_category, SUM(total_price) AS Total_Sales, SUM(total_price) * 100 /
     (SELECT SUM(total_price) FROM pizza_sales WHERE MONTH(STR_TO_DATE(order_date, '%m/%d/%Y')) = 1)  AS PCT
     FROM pizza_sales
     WHERE MONTH(STR_TO_DATE(order_date, '%m/%d/%Y')) = 1
     GROUP BY pizza_category;

 E.	% of Sales by Pizza Size
    SELECT pizza_size, CAST(SUM(total_price) AS DECIMAL(10,2)) AS Total_Sales, CAST(SUM(total_price) * 100 /
    (SELECT SUM(total_price) FROM pizza_sales) AS DECIMAL(10,2)) AS PCT
    FROM pizza_sales
    GROUP BY pizza_size
    ORDER BY PCT DESC;
    
 F.	Top 5 Pizza by Revenue
    SELECT pizza_name, SUM(total_price) AS Total_Revenue FROM pizza_sales
    GROUP BY pizza_name
    ORDER BY Total_Revenue DESC
    LIMIT 5;

 G.	Bottom 5 by Revenue
    SELECT pizza_name, SUM(total_price) AS Total_Revenue FROM pizza_sales
    GROUP BY pizza_name
    ORDER BY Total_Revenue
    LIMIT 5;
    
 H.	Top 5 Pizza by Quantity
    SELECT pizza_name, SUM(quantity) AS Total_quantity FROM pizza_sales
    GROUP BY pizza_name
    ORDER BY Total_quantity DESC
    LIMIT 5;

 I.	Bottom 5 Pizza by Quantity
    SELECT pizza_name, SUM(quantity) AS Total_quantity FROM pizza_sales
    GROUP BY pizza_name
    ORDER BY Total_quantity 
    LIMIT 5;

 J.	Top 5 Pizza by Order_id
    SELECT pizza_name, COUNT(DISTINCT order_id) AS Total_orders FROM pizza_sales
    GROUP BY pizza_name
    ORDER BY Total_orders DESC 
    LIMIT 5;

 K.	Bottom 5 Pizza by Order_id
    SELECT pizza_name, COUNT(DISTINCT order_id) AS Total_orders FROM pizza_sales
    GROUP BY pizza_name
    ORDER BY Total_orders 
    LIMIT 5;
## Data Cleaning
Pizza size category we have in our database is abbreviated and for dashboard we need it in full expanded form. For eg. L= large, M= medium etc, so we will create an alias to temporary change its name in required format.

<img width="371" height="312" alt="2025-07-25 (1)" src="https://github.com/user-attachments/assets/7b40b179-a913-4d1a-8944-ed082d7b8e83" />

# Build Dashboard or a Report using Tableau
Created a comprehensive dashboard in Tableau featuring key metrics and charts, including Hourly Trend, Weekly Trend, Sales by Category, Sales by Size, Total Pizzas Sold by Category, Top 5 Best Sellers, and Bottom 5 Worst Sellers.

## KPI’S
- Total Revenue SUM([order id])
- Total Orders COUNTD([order id])
- Average Order Value [total revenue] / [total orders]
- Total Pizzas Sold SUM([quantity])
- Average Pizzas Per Order [total pizzas sold] / [total orders]
<img width="771" height="83" alt="2025-07-24 (6)" src="https://github.com/user-attachments/assets/f5039f40-773e-4a07-ac3a-f75f966ce891" />

## KEY INSIGHTS
<img width="151" height="181" alt="2025-07-24 (7)" src="https://github.com/user-attachments/assets/c940e2dd-6dff-410b-9cfd-80d9580deaeb" />
<img width="155" height="204" alt="2025-07-24 (8)" src="https://github.com/user-attachments/assets/c02152f1-a431-41cb-8f65-2a3f68f8f087" />

<img width="167" height="211" alt="2025-07-24 (9)" src="https://github.com/user-attachments/assets/9767645a-d822-4216-b6cb-9aaf367c758c" />
<img width="175" height="207" alt="2025-07-24 (19)" src="https://github.com/user-attachments/assets/79621c52-6334-4f2d-9aeb-6a06cba1845d" />

## DASHBOARD
<img width="935" height="526" alt="2025-07-24 (6)" src="https://github.com/user-attachments/assets/4dedd8f6-1f78-49d4-a393-8c34cdd4fba4" />
<img width="867" height="481" alt="2025-07-24 (7)" src="https://github.com/user-attachments/assets/4403c5dc-228d-4d87-9ddd-f731d4989c1e" />

## Tools, Software, and Libraries
· MySQL Workbench 8.0 CE
       for data analysis and storage
· Tableau Public 2025.2.0
      for dashboard creation and visualization
· Excel version 2024
      for initial data exploration and manipulation
## Reference
-	https://www.youtube.com/watch?v=lrl0vz-p-yc
