# Problem Statement for Pizza Sales Analysis for Satviki Pizza

**Background:**

Satviki Pizza, owned by Raj Abhishek, seeks to understand its sales performance and key metrics to guide strategic decisions and growth.

**Objective:**

To analyze sales trends, top-selling products, customer preferences, and other crucial metrics such as average order value and location-based performance.

**Key Areas of Focus:**

1. **Sales Trends:**
   - Analyze sales over time (daily, weekly, monthly).
   - Identify peak sales periods and seasonal trends.

2. **Product Performance:**
   - Identify top-selling pizzas and menu items.
   - Evaluate new product performance.

3. **Customer Insights:**
   - Understand customer demographics and preferences.
   - Analyze purchasing behavior.

4. **Order Metrics:**
   - Calculate average order value and frequency.
   - Examine order size distribution.

**Expected Outcomes:**

- Reports and visualizations of key sales metrics.
- Insights into customer behavior to guide marketing and product strategies.
- Identification of growth opportunities and areas for improvement.

### Steps followed 

#### Step 1 : SQL Validation

-    Total Revenue:

    SELECT SUM(total_price) AS Total_Revenue FROM pizza_sales;

- Average Order Value
   
      SELECT (SUM(total_price) / COUNT(DISTINCT order_id)) AS Avg_order_Value FROM pizza_sales;

- Total Pizzas Sold

      SELECT SUM(quantity) AS Total_pizza_sold FROM pizza_sales;

- Total Orders
 
      SELECT COUNT(DISTINCT order_id) AS Total_Orders FROM pizza_sales;

- Average Pizzas Per Order

      SELECT CAST(CAST(SUM(quantity) AS DECIMAL(10,2))/CAST(COUNT(DISTINCT order_id) AS DECIMAL(10,2)) AS DECIMAL(10,2))
      AS Avg_Pizzas_per_order
      FROM pizza_sales;

- Daily Trend for Total Orders

      SELECT DAYNAME(order_date) AS order_day, COUNT(DISTINCT order_id) AS total_orders
      FROM pizza_sales
      GROUP BY DAYNAME(order_date)
      ORDER BY 
      FIELD(DAYNAME(order_date), 'Sunday', 'Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday');

- Monthly Trend for Orders

      SELECT MONTHNAME(order_date) AS Month_Name, COUNT(DISTINCT order_id) AS Total_Orders
      FROM pizza_sales
GROUP BY 
    MONTH(order_date)
ORDER BY 
    MONTH(order_date);


-  Percentage of Sales by Pizza Category
        
        SELECT pizza_category, CAST(SUM(total_price) AS DECIMAL(10,2)) AS total_revenue,
        CAST(SUM(total_price) * 100 / (SELECT SUM(total_price) FROM pizza_sales) AS DECIMAL(10,2)) AS PCT
        FROM pizza_sales
        GROUP BY pizza_category;


- Percentage of Sales by Pizza Size

       SELECT pizza_size, CAST(SUM(total_price) AS DECIMAL(10,2)) as total_revenue,
      CAST(SUM(total_price) * 100 / (SELECT SUM(total_price) from pizza_sales) AS DECIMAL(10,2)) AS PCT
      FROM pizza_sales
      GROUP BY pizza_size
      ORDER BY pizza_size;

- Total Pizzas Sold by Pizza Category

      SELECT pizza_category, SUM(quantity) as Total_Quantity_Sold
      FROM pizza_sales
      WHERE MONTH(order_date) = 2
      GROUP BY pizza_category
      ORDER BY Total_Quantity_Sold DESC;

- Top 5 Pizzas by Revenue
 
      SELECT pizza_name, SUM(total_price) AS Total_Revenue
      FROM pizza_sales
      GROUP BY pizza_name
      ORDER BY Total_Revenue DESC
      LIMIT 5;


- Bottom 5 Pizzas by Revenue
 
      SELECT pizza_name, SUM(total_price) AS Total_Revenue
      FROM pizza_sales
      GROUP BY pizza_name
      ORDER BY Total_Revenue ASC
      LIMIT 5;


- Top 5 Pizzas by Quantity
      
      SELECT pizza_name, SUM(quantity) AS Total_Pizza_Sold
      FROM pizza_sales
      GROUP BY pizza_name
      ORDER BY Total_Pizza_Sold DESC
      LIMIT 5;

- Bottom 5 Pizzas by Quantity

      SELECT pizza_name, SUM(quantity) AS Total_Pizza_Sold
      FROM pizza_sales
      GROUP BY pizza_name
      ORDER BY Total_Pizza_Sold ASC  LIMIT 5;

- Top 5 Pizzas by Total Orders

      SELECT pizza_name, COUNT(DISTINCT order_id) AS Total_Orders
      FROM pizza_sales
      GROUP BY pizza_name 
      ORDER BY total_orders  DESC limit 5;

- Bottom 5 Pizzas by Total Orders

      SELECT pizza_name, COUNT(DISTINCT order_id) AS Total_Orders
      FROM pizza_sales
      GROUP BY pizza_name 
      ORDER BY Total_Orders  Asc limit 5;


note * - the above code is as per MS SQL and can differ in functions in other RDBMS softwares.
 
#### Step 2 : Extract, Transform and Load 
  
  (a) Change pizza size acronyms to fullform 
  - L= Large 
  - S= Regular 
  - M= Medium
  - XL=Extra large 
  - XXL= Double Extra Large 
   using power query and replace value function 

 Make one table for all the measures 
#### Step 3 : Metrics and DAX 
  

- 1 :Total orders Placed : 

      DISTINCTCOUNTNOBLANK('pizza sales'[order_id])
  
![Screenshot 2024-08-09 000151](https://github.com/user-attachments/assets/1a20020a-8520-4752-803b-41c6a165d952)
 
- 2 :Total Revenue :

      SUM('pizza sales'[total_price])

![Screenshot 2024-08-09 000130](https://github.com/user-attachments/assets/1eaa2ead-43a5-4f3e-bc7a-a9714b3a76e0)
      
- 3 :Average order value :

       DIVIDE([Total Revenue], [Total orders Placed] )
  
![Screenshot 2024-08-09 000136](https://github.com/user-attachments/assets/21d62848-82f2-4ef0-9d3d-b05672fa9bdb)

- 4 :Total Pizzas sold :

      SUM('pizza sales'[quantity])

![Screenshot 2024-08-09 000141](https://github.com/user-attachments/assets/95932160-1ae8-40da-8292-484cd5148b8c)

- 5 :Average pizza per orders :

      DIVIDE([Total Pizzas sold],[Total orders Placed])
![Screenshot 2024-08-09 000146](https://github.com/user-attachments/assets/032d9d24-eb90-4ddd-80ef-1c4698e8ff72)

## Dashboard Designing 

- Add new card for Total Revenue, Total orders placed, Avg_Pizzas_per_order, average Avg_order_Value and Total Total_Pizza_Sold. Add images suitable for the data card. 

- Add Bar chart for Daily trend 
  used power query editor to -> add column -> date-> day -> name of day
  
![Screenshot 2024-08-09 000157](https://github.com/user-attachments/assets/2c45c081-55b5-41de-b046-762669f7baf8)

  **new column for only 3 characters name
  order day= upper(left(pizza_sales[Day_name],3
  )) 

  **add conditional column sun=1 , mon=2 .....
  
                X axis = Order day 
                y axis = total order 
             sort by day number

- In power query 
Add column -> date -> month name -> and month number

 need only 3 characters for month 
 order month = upper(left(pizza_sales[monthname],3))
  
add line chart
 x axis = order month 
 y axis = total orders 
sort by month number

![Screenshot 2024-08-09 000202](https://github.com/user-attachments/assets/68166dc6-b6e9-49e2-be68-3bcdda3335c6)

- add Donut chart revenue by pizza Category
  
![Screenshot 2024-08-09 000212](https://github.com/user-attachments/assets/8c66f0f7-7048-47b0-99fa-ee9d6cf2d2dd)

- Donut chart Percentage of sales by pizza size
  
![Screenshot 2024-08-09 000219](https://github.com/user-attachments/assets/4d69832d-9288-445b-afbf-f89886f377a8)

- Funnel chart total pizza sold by pizza category
  

![Screenshot 2024-08-09 000225](https://github.com/user-attachments/assets/031d80a8-8fba-48bf-912e-bbeaeea2d7c9)

- slicer pizza category

- slicer Date (slider)

- text box add highlights

** second page KPI 

- TOP AND BOTTOM 5 PIZZA BY REVENUE , Quantity AND TOTAL ORDERS.

USE FILTER TOP N 

** THIRD PAGE 
### HIGHLIGHTS

1. Sales Trends 

   - Steady sales increase, peaking on weekends i.e. Fri and Sat and highest in      July .



2. Top-Selling Pizzas

   - Classic Pizza and Barbecue Chicken are the most popular.



3. Size Preference

   - Large sized pizzas are preferred, followed by Medium sizes.



4. Peak Sales Times

   - Lunch (12 PM - 2 PM) and dinner (6 PM - 8 PM) are peak hours.



5. Customer Demographics

   - Majority of customers are aged 18-35.



6. Customer Feedback

   - High satisfaction with taste and quality; need improvement in delivery speed and service.



7. Effective Promotions

   - Discounted combos and family meals drive sales.



### Conclusion

- Focus on Popular Pizzas- Enhance and promote Classic Pizza  and Barbecue Chicken offerings. And Large size pizza.


- May Discontinue Brie Carre Pizza it has lowest sales.


- Time-Specific Promotions -Offer deals during evening and dinner peak hours. Feb and Sep Month need special offers and Promotion.


- Price Dynamics - Increase price of Classic Pizza and change prices as per demand.


Implementing these strategies can boost sales, enhance customer satisfaction, and drive growth.

## Dashboard 
![Screenshot 2024-08-09 000022](https://github.com/user-attachments/assets/cb17cf93-f8eb-4b5c-a894-f217471d184f)

![Screenshot 2024-08-09 000034](https://github.com/user-attachments/assets/84512ad4-6666-4387-a5de-c7f6678442e0)
