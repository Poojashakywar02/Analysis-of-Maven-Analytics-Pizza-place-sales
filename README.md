# Analysis-of-Maven-Analytics-Pizza-place-sales
Project Title: Sales Performance Analysis for Plato’s Pizza using Power BI
Business Problem

Plato’s Pizza, a Greek-inspired pizza restaurant in New Jersey, aimed to increase overall sales and improve business performance. However, they lacked clear visibility into:

Sales trends across time (daily, weekly, hourly)
Best and worst performing pizzas
Customer ordering behavior and preferences
Contribution of pizza categories, sizes, and ingredients to revenue

The objective of this project was to analyze transactional data and deliver actionable insights that could help optimize sales, improve menu strategy, and enhance operational decision-making.

Dataset Overview


The dataset consisted of four CSV files:

Orders Table – Contains order ID, date, and time of each transaction
Order Details Table – Includes pizza IDs and quantities for each order
Pizzas Table – Provides pizza size, price, and associated pizza type
Pizza Types Table – Contains pizza names, categories, and ingredients
Approach & Methodology
1. Data Extraction & Transformation (Power Query)
Imported multiple CSV files into Power BI
Cleaned and transformed data by:
Handling missing/null values
Removing duplicates
Standardizing data types
Renaming columns for consistency
Merged datasets to prepare for relational modeling
2. Data Modeling
Established relationships between tables using one-to-many relationships
Created a star schema model to improve performance and usability
Ensured referential integrity across fact and dimension tables
3. DAX Calculations & Metrics

Developed key performance indicators (KPIs) and measures using DAX:

Total Sales
Sales = SUMX('order_details', order_details[quantity] * RELATED(Pizzas[price]))
Top 5 Best-Selling Pizzas
Top 5 Sales = 
VAR TopSalesTable =
    TOPN(
        5,
        SUMMARIZE('order_details', 'order_details'[pizza_id], "TotalSales", [Sales]),
        [TotalSales], DESC
    )
RETURN SUMX(TopSalesTable, [TotalSales])
Bottom 5 Worst-Selling Pizzas
Bottom 5 Sales = 
VAR BottomSalesTable =
    TOPN(
        5,
        SUMMARIZE('order_details', 'order_details'[pizza_id], "TotalSales", [Sales]),
        [TotalSales], ASC
    )
RETURN SUMX(BottomSalesTable, [TotalSales])
Average Pizzas per Order
Avg Pizzas per Order =
DIVIDE(COUNT(order_details[order_details_id]), DISTINCTCOUNT(order_details[order_id]))
Average Orders per Day
Avg Orders per Day =
DIVIDE(DISTINCTCOUNT(order_details[order_id]), DISTINCTCOUNT(Orders[date]))
Time Slot Segmentation
Time Slot =
SWITCH(
    TRUE(),
    Orders[time] >= TIME(9,0,0) && Orders[time] < TIME(12,0,0), "9 AM - 12 PM",
    Orders[time] >= TIME(12,0,0) && Orders[time] < TIME(15,0,0), "12 PM - 3 PM",
    Orders[time] >= TIME(15,0,0) && Orders[time] < TIME(18,0,0), "3 PM - 6 PM",
    Orders[time] >= TIME(18,0,0) && Orders[time] < TIME(21,0,0), "6 PM - 9 PM",
    "9 PM - 12 AM"
)
4. Dashboard Development

Built an interactive Power BI dashboard with:

Sales trends over time (daily, weekly)
Peak order hours and busiest days
Category-wise and size-wise performance
Top and bottom-performing pizzas
KPI cards for quick insights (Total Sales, Orders, Avg Orders, etc.)
Filters/slicers for dynamic analysis (category, size, time)

5. Key Insights & Findings
Total Revenue: ~$817.86K generated annually
Total Orders: ~21.35K orders processed
Daily Performance: ~60 pizzas sold on average per day
Peak Sales Period: Fridays between 12 PM – 3 PM
Top Performing Categories: Chicken, Classic, and Supreme
Lowest Performing Categories: Veggie and some Supreme pizzas
Most Preferred Size: Large pizzas accounted for ~45.89% of total sales
Most Popular Category: Classic pizzas
