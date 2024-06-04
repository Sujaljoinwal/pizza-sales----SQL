 dashbord link - ![Screenshot 2024-06-02 164141](https://github.com/Sujaljoinwal/pizza-sales----SQL/assets/159111450/5c047a08-8009-4eaf-bdc0-855d475869ca)

Basic 
Question 1 - Retrieve the total number of orders placed.

Select count(order_id) as Total_orders
from orders

Question 2 - Calculate the total revenue generated from pizza sales.

Select 
Round(sum(order_details.quantity * pizzas.price),2) as Total_revenue
from order_details
join pizzas on order_details.pizza_id = pizzas.pizza_id;

Question 3 - Identify the highest-priced pizza.

Select 
pizza_types.name , pizzas.price
from 
pizza_types
join pizza_types.pizza_type_id = pizzas.pizza_type_id
order by pizza.price desc
limit 1;

Question 4 - Identify the most common pizza size ordered.

Select 
pizzas.size , count(order_details.order_details_id)
from pizzas
join order_details 
on pizzas.pizza_id = order_details.pizza_id
group by pizzas.size
order by count(order_details.order_details_id) desc
limit1;

Question 5 - List the top 5 most ordered pizza types along with their quantities.

Select 
pizza_types.name , count(order_details.quantity) as count_quantity
from 
order_details
join pizzas
on order_details.pizza_id = pizzas.pizza_id
join pizza_types
on pizzas.pizza_type_id = pizza_types.pizza_type_id
group by pizza_types.name
order by count(order_details.quantity) desc
limit 5;

Intermediate:

Question 6 - Join the necessary tables to find the total quantity of each pizza category ordered.

Select 
pizza_types.category , count(order_details.quantity) as Total_qunatity
from order_details
join pizzas
on order_details.pizza_id = pizzas.pizza_id
join pizza_types
on pizzas.pizza_type_id = pizza_types.pizza_type_id
group by pizza_types.category
order by count(order_details.quantity) desc;

Question 7 - Determine the distribution of orders by hour of the day.

Select 
Hour(orders.order_time) as Hour,
count(orders.order_id) as count_orders
from orders
group by hour(orders.order_time)

Question 8 - Join relevant tables to find the category-wise distribution of pizzas.

select 
pizza_types.category as category,
count(pizza_types.pizza_type_id) as orders_count
from pizza_types
group by pizza_types.category
order by orders_count desc;

Question 9 - Group the orders by date and calculate the average number of pizzas ordered per day.

Select 
Round(Avg(sum_quantity)) as average_pizza_ordered
from 
(select orders.order_date , sum(order_details.quantity) as sum_ quantity;

Question 10  - Determine the top 3 most ordered pizza types based on revenue.

select 
pizza_type.name,
Round(Sum(order_details.quantity * pizzas.price),2) as Total_sales
from order_details
join pizzas
on order_details.pizza_id  = pizzas.pizza_id
join pizza_types
on pizzas.pizza_type_id = pizza_types.pizza_type_id
group by pizza_types.name
order by total_sales desc
limit 3;

Advanced:

Question 11 - Calculate the percentage contribution of each pizza type to total revenue.

Select 
pizza_types.category,
Round(Sum(order_details.quantity * pizzas.price) / 
(Select Round(Sum(order_details.quantity * prizzas.price),2 ) as sum_revenue
from order_details
join pizzas
on order_details.pizza_id = pizzas.pizza_id ) * 100, 2) as revenue
from order_details
join pizzas
on order_details.pizza_id = pizzas.pizza_id
join pizza_types
on pizzas.pizzza_type_id = pizza_types.pizza_type_id
group by pizza_types.category
order by revenue desc;

Question 12 - Analyze the cumulative revenue generated over time.

Select 
order_date,
Sum(total_sales) over ( order by order_date) as cumlative_sum
from 
(select orders.order_date , 
sum(order_details.quantity * pizzas.price) as total_sales
from order_details
join pizzas
on order_details.pizza_id = pizzas.pizza_id
join orders
on order_details.order_id = orders.order_id
group by orders.order_date) as revenue

Question 13 - Determine the top 3 most ordered pizza types based on revenue for each pizza category.

Select 
category , name , revenue
from 
(select category , name , revenue , 
rank() over(partition by category order by revenue desc) as cate_rank
from 
(select pizza_types.category , pizza_types.name, 
sum(order_details.quantity * pizzas.price) as revenue
from pizza_types
join pizzas
on pizza_types.pizza_type_id  = pizzas.pizza_type_id
join order_details
on order_details.pizza_id = pizzas.pizza_id
group by pizza_types.category , pizza_types.name ) as reveneue_table ) as rank_table
where cate_rank < = 3;

