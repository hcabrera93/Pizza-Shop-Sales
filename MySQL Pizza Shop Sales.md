##### My name is Hector Cabrera and I will be clenaing and analzying the data set called "Pizza Place Sales" courtesy of Maven Anlaytics. In this project I will be answering a few quesitions which are:

##### 1. What is the most popular item on the menu? Which item is the least popular?
##### 2. Average pizza order (how many pizza pies do most people buy per order?)
##### 3. Average price of pizza.
##### 4. Is there a trend in revenue/total orders? Which month made the most money/most orders? Which month made the least money/orders?
##### 5. Which size of pizza is most ordered?
##### 6. What are considered peak hours for the pizza shop?
##### 7. What day of the week is most popular for orders? Which day of the week is least popular?

##### Imported 4 different csv files which are named order_details, orders, pizza_types, and pizzas 

USE pizza_place_sales

##### Lets look at the different tables

SHOW tables

![all tables](https://github.com/hcabrera93/Pizza-Shop-Sales/assets/131682605/f26fcd38-444d-4412-bf6f-651373f5ff2c)

##### We have 4 different tables which are order_details, orders, pizza_types, and pizzas. Lets see what each table contains.
 
SELECT * 
	FROM order_details;
 
![order detail table](https://github.com/hcabrera93/Pizza-Shop-Sales/assets/131682605/fb3a119b-b550-40ef-8be4-0ba882b16013)

SELECT *
	FROM orders;

 ![orders table](https://github.com/hcabrera93/Pizza-Shop-Sales/assets/131682605/38dbb1a1-d78d-4f53-b489-2a28de85e784)

SELECT *
	FROM pizza_types;

 ![pizza type table](https://github.com/hcabrera93/Pizza-Shop-Sales/assets/131682605/52e17984-6324-4a61-bf52-43b31cb4eb64)
    
SELECT * 
	FROM pizzas;

 ![pizzas table](https://github.com/hcabrera93/Pizza-Shop-Sales/assets/131682605/f8d8105e-8818-479a-9da2-bc274f3ae11a)

##### Lets check out how big the data is!

##### Date range

SELECT MIN(date) AS nearest_date, MAX(date) AS further_date
FROM orders;

![date range](https://github.com/hcabrera93/Pizza-Shop-Sales/assets/131682605/652f8f37-2a89-4f17-b568-548be813aa0d)

##### So we are dealing with a year's worth of data. Lets start with answering question 

#---------------------------------------------------------------------------------------------------------------------

##### Question 1. What is the most popular item on the menu? Which item is the least popular?

SELECT pizza_id, COUNT(pizza_id) AS total_count
	FROM order_details
    GROUP BY pizza_id
    ORDER BY total_count DESC;

![question 1 max](https://github.com/hcabrera93/Pizza-Shop-Sales/assets/131682605/ff497792-3c59-410c-90df-78240a3743d8)
![question 1 min](https://github.com/hcabrera93/Pizza-Shop-Sales/assets/131682605/2f8e4888-28ad-4b12-a76e-bb1e6df24715)


##### By using the pizza_id from the order table, we can see the most bought pizza is the big meat pizza size small, while the least bought pizza is The Greek XXL pizza. 

#---------------------------------------------------------------------------------------------------------------------

##### Qeustion 2. Average pizza order (how many pizza pies do most people buy per order?)

SELECT MAX(order_id) AS total_orders
	FROM order_details;

 ![question 2 average total orders](https://github.com/hcabrera93/Pizza-Shop-Sales/assets/131682605/1d55cc61-807a-4c1d-84f8-b8f864d36201)
    
##### As we see there is 21,350 orders with the sum for each of their respective orders. It would be difficult to add all of these numbers one by one.
##### But how about we take the sum of the quantity for the orders and divide that with the number of orders.

SELECT SUM(quantity) AS total_quantity
	FROM order_details;

 ![question 2 total pizzas sold](https://github.com/hcabrera93/Pizza-Shop-Sales/assets/131682605/00456271-f48c-4a4c-ae28-295741f7a114)

##### We get 49,574. Lets divide that by the total number of orders!

SELECT SUM(quantity)/MAX(order_id) AS average_pizza_per_order
	FROM order_details;

 ![question 2 average pizzas per order](https://github.com/hcabrera93/Pizza-Shop-Sales/assets/131682605/6083b38b-35c9-4b3b-808f-c5b20749e778)
    
##### We get an average of 2.3 pizzas per order.
#---------------------------------------------------------------------------------------------------------------------

##### Question 3. Average Price of a pizza.

SELECT AVG(price) AS avg_price
	FROM pizzas;
    
##### We can see that the avergage price of the pizza is approximately $16.44.

#---------------------------------------------------------------------------------------------------------------------

##### Question 4. Is there a trend in revenue/total orders? Which month made the most money/most orders? Which month made the least money/orders?

SELECT DISTINCT(MONTHNAME(date)) AS months, ROUND(SUM(price)) AS total_value, COUNT(order_details.order_id) AS total_orders
	FROM pizzas
    INNER JOIN order_details ON pizzas.pizza_id = order_details.pizza_id
	INNER JOIN orders ON order_details.order_id = orders.order_id
    GROUP BY months;

![Question 4](https://github.com/hcabrera93/Pizza-Shop-Sales/assets/131682605/197b3aa2-e53a-4442-be02-1bf1f60e19d1)
    
#### From this query we were able to pull the total orders and total price for each month. If we simply click the "total_value" icon we can see that July made the most orders
#### and revenue while October made the least amount of orders and revenue.

#---------------------------------------------------------------------------------------------------------------------

##### Question 5. Which size of pizza is most ordered?
    
SELECT DISTINCT size, COUNT(order_id) AS total_orders
	FROM order_details
    INNER JOIN pizzas on order_details.pizza_id = pizzas.pizza_id
    GROUP BY size
    ORDER BY size;

![question 5](https://github.com/hcabrera93/Pizza-Shop-Sales/assets/131682605/08136fc9-a5ce-4870-9616-552a595164ce)

##### From this query, we see that most customers purchases large orders!

#---------------------------------------------------------------------------------------------------------------------

##### Question 6. What are considered peak hours for the pizza shop?

SELECT HOUR(time) AS order_hour, COUNT(order_id) AS total_orders
FROM orders
GROUP BY order_hour
ORDER BY order_hour;

![Question 6](https://github.com/hcabrera93/Pizza-Shop-Sales/assets/131682605/1f2b2d8f-f3b7-4a97-9226-47981e6142f5)

##### Based on these results, peak hours are from the hours of roughly 12pm - 2:00pm. I grouped the order time to hours because it makes
##### the answer more cohesive instead of having each individual time per hour such as (11:38:36 AM and 11:57:40 AM) which are actual times, show Excel sheet)

#---------------------------------------------------------------------------------------------------------------------

##### Question 7. What day of the week is most popular for orders? Which day of the week is least popular?

SELECT DAYNAME(date) AS weekday, COUNT(order_id) AS total_orders
	FROM orders
    GROUP BY weekday
    ORDER BY total_orders DESC;

![Question 7](https://github.com/hcabrera93/Pizza-Shop-Sales/assets/131682605/d32e8b12-6f0b-4bc6-8fb2-cbbef3d6f1d2)

##### The results indicate that Friday is the most popular day while Sunday is the least popular day. 
