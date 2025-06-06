# PizzaHut-SQL-Sales-Analysis
This is a Sales Analysis project conducted using MySQL, leveraging a dataset sourced from Kaggle. The project focuses on executing SQL operations to extract key business insights, optimize sales performance evaluation, and support data-driven decision-making.

---
## Questions:
1. Retrieve the total number of orders placed. Calculate the total revenue generated from pizza sales.
2. Identify the highest-priced pizza. Identify the most common pizza size ordered.
3. List the top 5 most ordered pizza types along with their quantities.
4. Join the necessary tables to find the total quantity of each pizza category ordered.
5. Determine the distribution of orders by hour of the day. Join relevant tables to find the category-wise distribution of pizzas.
6. Group orders by date to find the average pizzas ordered per day and list the top 5 dates with the highest pizza orders.
7. Determine the top 3 most ordered pizza types based on revenue.
8. Calculate the percentage contribution of each pizza type to total revenue.
9. Analyze the cumulative revenue generated over time.
10. Determine the top 3 most ordered pizza types based on revenue for each pizza category.
---

**1. Retrieve the total number of orders placed. Calculate the total revenue generated from pizza sales.**

**SQL Query Performed:**

SELECT

    COUNT(DISTINCT orders.order_id) AS total_orders,
    
    SUM(order_details.quantity * pizzas.price) AS total_revenue
    
FROM 

    orders
    
JOIN 

    order_details ON orders.order_id = order_details.order_id
    
JOIN 

    pizzas ON order_details.pizza_id = pizzas.pizza_id;

**Total Orders Placed:** 21,350

**Total Revenue Generated:** $817,860.05

**✅Insights:** The business shows healthy order volume and substantial revenue generation. This indicates strong demand and consistent customer engagement.

**💡 Recommendation:** Consider introducing loyalty or referral programs to further capitalize on the high order frequency and increase average revenue per order.

---
**2. Identify the highest-priced pizza. Identify the most common pizza size ordered.**

**SQL Query For Highest Price Pizza:**

SELECT

    pizza_types.name, pizzas.price

FROM

    pizza_types join pizzas

ON

    pizza_types.pizza_type_id = pizzas.pizza_type_id

ORDER BY

    pizzas.price desc limit 1;

---    
**SQL Query for Most Common Pizza Size Ordred:**

SELECT

    size, sum(quantity) as order_count

FROM

    pizzas join order_details on order_details.pizza_id = pizzas.pizza_id

GROUP BY

    size

ORDER BY

    order_count desc;

**Most Expensive Pizza:** The Greek Pizza priced at $35.95

**Common Pizza Size Ordered:** Large (18956 orders)

**✅ Insight:** High-priced gourmet options like The Greek Pizza suggest there’s a premium customer segment.

**💡 Recommendation:** Highlight premium pizzas more prominently in marketing materials or bundle them with offers to boost high-margin sales.

---
**3. List the top 5 most ordered pizza types along with their quantities.**

**SQL Query Performed:**

SELECT

    pizza_types.name, sum(order_details.quantity) as quantity
    
FROM

    pizza_types join pizzas on pizza_types.pizza_type_id = pizzas.pizza_type_id join order_details
    
ON

    order_details.pizza_id = pizzas.pizza_id
    
GROUP BY

    pizza_types.name
    
ORDER BY

    quantity desc limit 5;

| Pizza Type    | Quantity|
| ------------- | ------- |
| The Classic Deluxe Pizza  | 2,453 |
| The Barbecue Chicken Pizza | 2,432 |
| The Hawaiian Pizza  | 2,422|
| The Pepperoni Pizza  | 2,418 |
| The Thai Chicken Pizza  | 2,371 |

**✅ Insight:** The sales distribution is quite balanced among the top 5, showing a broad appeal across both classic and specialty flavors.

**💡 Recommendation:** Focus promotions on these top sellers with limited-time deals or combo meals to drive volume further.

---
**4. Join the necessary tables to find the total quantity of each pizza category ordered.**

**SQL Query Performed:**

SELECT

    pizza_types.category, sum(order_details.quantity) as quantity
    
FROM

    pizza_types join pizzas
    
ON

    pizza_types.pizza_type_id = pizzas.pizza_type_id
    
JOIN

    order_details
    
ON

    order_details.pizza_id = pizzas.pizza_id
    
GROUP BY

    pizza_types.category
    
ORDER BY 

    quantity desc;

| Category | Quantity|
| ---------| ------- |
| Classic |14,888|
| Supreme |11,987|
| Veggie |11,649|
| Chicken |11.050|

**✅ Insight:** Classic pizzas are the most preferred category, followed closely by Supreme and Veggie options. This suggests a preference for familiar and flavorful options.

**💡 Recommendation:** Ensure sufficient inventory and production for Classic pizzas during peak hours. Explore new variants under the Classic category to drive repeat sales.

---
**5. Determine the distribution of orders by hour of the day. Join relevant tables to find the category-wise distribution of pizzas.**

**SQL Query Performed:**

SELECT

    hour(order_time) as Hour, count(order_id) as Order_Count
    
FROM

    orders
    
GROUP BY
    hour(order_time)

|Hour|Order_Count|
| ---------| ------- |
|9|1|
|10|8|
|11|1231|
|12|2520|
|13|2455|
|14|1472|
|15|1468|
|16|1920|
|17|2336|
|18|2399|
|19|2009|
|20|1642|
|21|1198|
|22|663|
|23|28|

• **Peak Hours:** 12 PM – 2 PM and 5 PM – 7 PM

• **Highest Orders:** 12 PM (2,520), 1 PM (2,455), 6 PM (2,399)

**✅ Insight:** Lunchtime and dinner hours drive the highest volume of orders, reflecting traditional mealtime peaks.

**💡 Recommendation:** Increase kitchen staff and delivery capacity during peak hours. Consider introducing time-bound discounts in low-order hours (e.g., post 9 PM) to smoothen demand.

---
**6. Group orders by date to find the average pizzas ordered per day and list the top 5 dates with the highest pizza orders.**

**SQL Query Performed:**

SELECT

    ROUND(AVG(quantity), 0) as Average_Pizza_Order_Per_Day
    
FROM

    (SELECT 
        orders.order_date, SUM(order_details.quantity) AS quantity
        
FROM
    
        orders
        
JOIN

        order_details ON orders.order_id = order_details.order_id
        
GROUP BY 

        orders.order_date) AS order_quantity;

**Average:** 138 pizzas per day

**✅ Insight:** Steady daily sales indicate a healthy baseline demand.

**💡 Recommendation:** Use this figure to forecast inventory, staffing, and marketing spend. Consider setting targets to raise this daily average through loyalty programs or weekday promotions.

---
**7. Determine the top 3 most ordered pizza types based on revenue.**

**SQL Query Performed:**

SELECT

        pizza_types.name, sum(order_details.quantity*pizzas.price) as Revenue
        
FROM

        pizza_types join pizzas
        
ON

        pizzas.pizza_type_id = pizza_types.pizza_type_id
        
JOIN

        order_details
        
ON

        order_details.pizza_id = pizzas.pizza_id
        
GROUP BY

        pizza_types.name order by Revenue desc limit 3;

|Pizza_Type|Revenue|
| ---------| ------- |
|The Thai Chicken Pizza|43,434.25|
|The Barbecue Chicken Pizza|42,768.00|
|The California Chicken Pizza|41,409.58|

**✅ Insight:** Chicken-based pizzas dominate revenue, suggesting strong customer preference for protein-heavy or specialty flavors.

**💡 Recommendation:** Consider bundling these high-performing pizzas in meal deals or highlighting them in featured menus to push even greater sales.

---
**8. Calculate the percentage contribution of each pizza type to total revenue.**

**SQL Query Performed:**

SELECT

        pizza_types.category, ROUND(
        SUM(order_details.quantity * pizzas.price) / 
        (
            SELECT SUM(order_details.quantity * pizzas.price)
            FROM order_details
            JOIN pizzas ON pizzas.pizza_id = order_details.pizza_id
        ) * 100, 2
    ) AS revenue
    
FROM

        pizza_types 
        
JOIN

        pizzas ON pizza_types.pizza_type_id = pizzas.pizza_type_id
        
JOIN

        order_details ON order_details.pizza_id = pizzas.pizza_id
        
GROUP BY 

        pizza_types.category
        
ORDER BY

        revenue DESC;


|Category|Revenue Share%|
| -------| ------------ |
|Classic|26.91|
|Supreme|25.46|
|Chicken|23.96|
|Veggie|23.68|

**✅ Insight:** Revenue is well-distributed across all four categories, with no extreme reliance on one.

**💡 Recommendation:** Maintain menu diversity and continue offering balanced marketing to cater to varied customer preferences.

---

**9. Analyze the cumulative revenue generated over time.**

**SQL Query Performed:**

SELECT

        order_date, sum(revenue) over (order by order_date) as cum_revenue
        
FROM

        (select orders.order_date, sum(order_details.quantity * pizzas.price) as revenue
        
FROM

        order_details join pizzas
        
ON

        order_details.pizza_id = pizzas.pizza_id
        
JOIN

        orders
        
ON

        orders.order_id = order_details.order_id
        
GROUP BY

        orders.order_date) as sales;

**Total Revenue by Year-End (Dec 31):** $817,860.05

**✅ Insight:** There is a smooth upward trend in revenue generation, with strong performance toward the end of the year.

**💡 Recommendation:** Identify any seasonal campaigns or events that may have boosted year-end sales and consider replicating them annually.

---

**10. Determine the top 3 most ordered pizza types based on revenue for each pizza category.**

**SQL Query Performed:**

SELECT

        category, name, revenue, rn
        
FROM

        (select category, name, revenue, rank() over (partition by category order by revenue desc) as rn
        
FROM

        (select pizza_types.category, pizza_types.name, sum(order_details.quantity*pizzas.price) as revenue
        from pizza_types join pizzas
        on pizza_types.pizza_type_id = pizzas.pizza_type_id
        join order_details
        on order_details.pizza_id = pizzas.pizza_id
        group by pizza_types.category, pizza_types.name) as a) as b

WHERE

        rn <= 3;

|Category|Top Pizza Type|Revenue ($)|
| -------| ------------ |------|
|Chicken|The Thai Chicken Pizza|43,434.25|
|Classic|The Classic Delux Pizza|38,180.50|
|Supreme|The Spicy Italian Pizza|34,831.25|
|Veggie|The Four Cheese Pizza|32,265.70|

**✅ Insight:** Each category has standout performers significantly driving their respective revenues.

**💡 Recommendation:** Prioritize these top-sellers in seasonal promotions or combo meals. Also, analyze customer feedback on these for ideas when launching new flavors in each category.

---
**Conclusion:** This analysis of PizzaHut sales data highlights strong overall performance, with 21,350+ orders and revenue exceeding $817K. Sales are well-distributed across categories, with Classic and Chicken pizzas leading in both volume and revenue. Top performers like The Thai Chicken Pizza and The Classic Deluxe offer key opportunities for focused promotions.

Orders peak during lunch and dinner hours, and the daily average of 138 pizzas reflects steady demand. Revenue trends show consistent growth, with strong year-end sales.

These insights support actionable strategies in menu optimization, staffing, and targeted marketing, helping drive better customer engagement and operational efficiency.
