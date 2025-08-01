# SQL-RetailAnalysis
Welcome to my SQL project focused on Retail Exploratory Data Analysis (EDA) — a deep dive into customer behavior, sales trends, and product performance using structured query techniques.
-- Data Analysis & Business Key Problems & Answers

-- My Analysis & Findings
-- Q.1 Write a SQL query to retrieve all columns for sales made on '2022-11-05

select count(*) as orders_made_on_2022Nov5th
from retail_sales 
where sale_date = '2022-11-05';

select *
from retail_sales 
where sale_date = '2022-11-05';

-- Q.2 Write a SQL query to retrieve all transactions where the category is 'Clothing' and the quantity sold is more than 10 in the month of Nov-2022

select concat(monthname(sale_date) ,"-",year(sale_date)) as month_year, count(*) as orders
from retail_sales
where category = 'Clothing' And quantity > 3
Group by concat(monthname(sale_date) ,"-",year(sale_date));

-- Q.3 Write a SQL query to calculate the total sales (total_sale) for each category.

select category , sum(total_sale) as Total_Sales_By_Category
from retail_sales
group by category;

-- Q.4 Write a SQL query to find the average age of customers who purchased items from the 'Beauty' category.

select category, round(avg(age),2) as Avg_age_By_Category
from retail_sales
group by category
order by round(avg(age),2) desc;

-- Q.5 Write a SQL query to find all transactions where the total_sale is greater than 1000.

select count(*) as cnt_Sale_greater1000 
from retail_sales
where total_sale > 1000;

select category, count(*) as Sale_greater1000
from retail_sales
where total_sale > 1000
group by category;

-- Q.6 Write a SQL query to find the total number of transactions (transaction_id) made by each gender in each category.

select gender, count(transactions) as No_Of_Transaction
from retail_sales
group by gender;

-- Q.7 Write a SQL query to calculate the average sale for each month. Find out best selling month in each year

select monthname(sale_date) as Month,
year(sale_date) as Year, 
round(avg(Total_sale),2) as Avg_Sale_By_Month
from retail_sales
group by monthname(sale_date), year(sale_date)
order by round(avg(Total_sale),2) desc;

-- Q.8 Write a SQL query to find the top 5 customers based on the highest total sales 

select customer_id, sum(total_sale) as Total_sale
from retail_sales
group by customer_id
order by sum(total_sale)
limit 5;

 with cte as (select *, dense_rank() over (order by Total_Sale) as Sale_Rnk
				from (select customer_id, sum(total_sale) as Total_sale
				from retail_sales
				group by customer_id) ad )
select * from cte
where Sale_Rnk <= 5;

-- Q.9 Write a SQL query to find the number of unique customers who purchased items from each category.

Select category, count(Distinct Customer_id) as No_of_Unique_Cus
from Retail_Sales
Group by Category;

-- Q.10 Write a SQL query to create each shift and number of orders (Example Morning <=12, Afternoon Between 12 & 17, Evening >17)

With cte as (Select * ,
				Case 
				when hour(sale_time) < 12 then "Morning"
				when hour(sale_time) Between 12 and 17 then "Afternoon"
				else "Evening"
				End as Time_Shift
			from retail_sales)
select Time_shift, count(Transactions) as No_of_Orders
from cte
Group by Time_Shift
order by count(Transactions) desc ;
