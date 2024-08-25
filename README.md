# Pizza Store Analysis using SQL
![enter image description here](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEj-_soEWWcJP5WwXooHIj3wHsVty6eTZu6AxeyMzf91mC_bO4uT2vwyI2nD_ni0xUN3MjVsu2VFs1yJcp6t5VWA3faXA6S0OgIVFqIaP-o166E3nMTdOfQrk8K94okJkHIf87qTT35BrbtYtEhorPo81CVcFMhCH2FnxAPc0xQtlwHZ9k10rdeVkrk2Bnw/w548-h309/PIZZA%20SQL%20cover%20page.png)

In this project, I performed a comprehensive data analysis and exploration of pizza store sales using MySQL. With a dataset containing over 20,000 rows, I addressed several crucial business questions, including identifying top-selling pizzas, peak sales periods, and customer ordering patterns. By leveraging SQL queries, I extracted actionable insights that helped optimize inventory management, enhance customer satisfaction, and improve overall operational performance. This project highlights my ability to work with large datasets and derive meaningful conclusions to drive business success.

### About the Dataset
- Size: 20000+ rows
- Format: CSV
- Duration: Sales Data of 1 Year Period - From Jan 1 2015 to Dec 31 2015

### Tool(s) Used
- MySQL

## Data Model

![enter image description here](https://blogger.googleusercontent.com/img/a/AVvXsEjItggqHsQo75UtX7oNFBbV3hHy_1Im6Iw0WDK00cwsk17tlTcX4AS_CetajoDx5VSH4Tj_KU4cAMP9XrXglLpWzBfreSb6GKlDFQOvA9_emfFEtzi_jiKqspjg3YRM6Z70f7u0v2J-ddHcWH1ULisafEdL3FSucT0QDcI4mNSv43Hz3-54l_WLnkpfHj8=w555-h366)


# Analysis

### 1. Retrieve the total number of orders placed
#### Query

```sql
SELECT COUNT(order_id) AS total_orders FROM orders;
```

#### Result

![enter image description here](https://blogger.googleusercontent.com/img/a/AVvXsEiRp_JjJd34nnVSvyiCw6U6NGDSzISE8z31BBqYUD092X2ZnOvHWLFh5GvdfIfPoVLQ0Ez37WRdxkpU7CQ--WcpEk1X2kSEPZzb5u29i5hY0RI1Z82iaGgRs2hBpevRPVPvQJNeBTJrSlTITUCTG_aoqV3QUFG-s6C9VSn0psR_YzapnLYWSBAWUqPnn-o=w412-h128)
---
### 2. Calculate the total revenue generated from pizza sales
#### Query

```sql
SELECT ROUND(SUM(quantity * price), 2) AS Total_Revenue
FROM
    order_details
        JOIN
    pizzas ON order_details.pizza_id = pizzas.pizza_id;
 ```

#### Result
![enter image description here](https://blogger.googleusercontent.com/img/a/AVvXsEj5ASZGiIoRMuL-GoSzlG2Mp-tZGiHkLwJ6ao8c95zk_8V1KndMYXC7Y4W3UbUU8q38aip0ZBnXl-jfEyHj4EUBqSuAOa6u6v13Yb3NlAM5KnWhp2Ynpmh_VN04RIZU602rBPAFJWGMc5ZntHjc7_tI4l_Ic35-DdbBjuzeHX5i7hdXln2eEuFzv2SFlao=w402-h123)
---
### 3. Identify the highest-priced pizza
#### Query

```sql
SELECT 
    pizza_types.name, pizzas.price
FROM
    pizza_types
        JOIN
    pizzas ON pizza_types.pizza_type_id = pizzas.pizza_type_id
ORDER BY pizzas.price DESC
LIMIT 1;
```

#### Result
![enter image description here](https://blogger.googleusercontent.com/img/a/AVvXsEg1tvUwNY16Y1rQ7fnV-dVHYy6JBBHcFf9QzVuvF0eICNaoLa5Mfr9X6eRMULB7JbY3RFXvU_sh0G4Ewoul7WAnNcZrzYoUVq79uARrAAVQmCLXUMRjomR7YTKHJjtJh4u674YoA2OK37MMwqi7nhKf8VxNXhD64iRTjtZ0ENOXzBOlaaYljfPeEUTnwLc=w479-h114)
---
### 4. Identify the most common pizza size ordered
#### Query

```sql
SELECT 
    pizzas.size,
    COUNT(order_details.order_details_id) AS total_orders
FROM
    pizzas
        JOIN
    order_details ON pizzas.pizza_id = order_details.pizza_id
GROUP BY pizzas.size
ORDER BY total_orders DESC
LIMIT 1;
```

#### Result
![enter image description here](https://blogger.googleusercontent.com/img/a/AVvXsEhFC7Uciwi7Aw3tJIGD9b3Nzil_k7cNwsO2hxNyrRTJ9h4UPaaJ9_YpWVs-DIu8ce3Xgf9BzXv8dKNBW4XZutXw3g2e5nr7rOSDL6e2qJfhsNzlcGQ7t-R2bu89Wn1bjtvTUYmbM66VUzw5Yqc67g-9RgI1ZPDddsUhZQKoHrsycWHSYR7lA6-8uFpRIMg=w459-h152)
---
### 5. List the top 5 most ordered pizza types along with their quantities
#### Query

```sql
SELECT 
    pizza_types.name,
    SUM(order_details.quantity) AS total_orders
FROM
    pizza_types
        JOIN
    pizzas ON pizza_types.pizza_type_id = pizzas.pizza_type_id
        JOIN
    order_details ON pizzas.pizza_id = order_details.pizza_id
GROUP BY pizza_types.name
ORDER BY total_orders DESC
LIMIT 5; 
```
#### Result
![enter image description here](https://blogger.googleusercontent.com/img/a/AVvXsEgte8ONr76PfccoTqVFptjgKV-E4_bEZbjoKLJGDIr-ZNO_JlyfYmSKhswYx_0xy5asTv0rAzv4wA8nyV1RhspAZz_XLNWT8H-t3EFRlnc2QXzv-TVduBqEf9Wu6DXim03Il3kIjxwsUFWE5FOJQzybXYYmwcn3zolRDj5DFeGHBd2zBKbjDmLmbfny_Ns=w456-h208)
---
### 6. Find the total quantity of each pizza category ordered
#### Query
```sql
SELECT 
    pizza_types.category,
    SUM(order_details.quantity) AS quantity
FROM
    pizza_types
        JOIN
    pizzas ON pizza_types.pizza_type_id = pizzas.pizza_type_id
        JOIN
    order_details ON pizzas.pizza_id = order_details.pizza_id
GROUP BY pizza_types.category
order by quantity desc;
```
#### Result
![enter image description here](https://blogger.googleusercontent.com/img/a/AVvXsEhpmjh4q8QNA7HXt0x6FjlIbUxEVR2ywzMewdiA2JyO-t8Xr_C8C--80OJ1_rd8hiRx1Rx4jtJ0HGxB1Ou5CTQmCg3tWC1wa6-0GvYIGY4o-WXZI9Kn0mle84gSy2t__Fk8aQEmnui-RPz_ff1qmMZ_5wNOwL0fnZBnUtgwJ9wh0hJTuUlUpaPNQBehQmw=w370-h301)
---
### 7. Determine the distribution of orders by hour of the day
#### Query

```sql
SELECT 
    HOUR(order_time) AS hour_of_the_day,
    COUNT(order_id) AS orders
FROM
    orders
GROUP BY hour_of_the_day
ORDER BY hour_of_the_day ASC; 
```
#### Result
![enter image description here](https://blogger.googleusercontent.com/img/a/AVvXsEgVT_Of9x_srXhp4mfwgQg742JCKeVSencm2Hw-xSeJ1b9lfXb6vgp_haKBRiaNV7HnxFG0MquVx6AeV49Vr-gza62VyJXYe-COcQ4ynk6nr5k3NGiLSAIfPVxT4z7e5TD8NVXVw9bNBgU1K4kZnsQxisdUV304PtrG28mnyX5tJF2U5vfR6QfD8vooJiU=w334-h622)
---
### 8. Find the category-wise distribution of pizzas
#### Query

```sql
SELECT 
    category, COUNT(pizza_types.name) AS total_pizzas
FROM
    pizza_types
GROUP BY category;
```
#### Result
![enter image description here](https://blogger.googleusercontent.com/img/a/AVvXsEhHi1a9UL_nZRhXAWZ0syRrqhvCHP_mRniqVUZNGFmRvyUYHRiL1XaKJmxSkQABaJxicTT1zzL-1-0Ge8n0G_4psUZKJDsLUNM2R-qq0T-edsB0ITgWW34LL6Qk4SPwyzUaMulFoY8p5nUOtwu4oODcE1o91iEHN8BiE1UoeO5MuBF7mHt9-BGGz6yqCOg=w362-h258)
---
### 9. Calculate the average number of pizzas ordered per day
#### Query
```sql
SELECT 
    ROUND(AVG(total_orders), 0) AS avg_orders_per_day
FROM
    (SELECT 
        orders.order_date,
            SUM(order_details.quantity) AS total_orders
    FROM
        orders
    JOIN order_details ON orders.order_id = order_details.order_id
    GROUP BY orders.order_date) AS order_quantity; 
```
#### Result
![enter image description here](https://blogger.googleusercontent.com/img/a/AVvXsEhTLy_TWK90Tau38hC8zJyDd1cm2TRNCRkzKLYP5hAC8nK4Ty12OlV1Us8THTgTcIct7P4DixtlZ-59btrMvAEQCbcyiX-7foj9xY23IykJ3w3zkS_P0dH1DrD911y9bbLFKbiR5eMthMuuOgmDMVTVSkEG0ToflMg7WbTYyV4BNxiXg9tV9OY41O-bsHc=w402-h127)
---
### 10. Determine the top 3 most ordered pizzas based on revenue
#### Query
```sql
SELECT 
    pizza_types.name,
    ROUND(SUM((order_details.quantity * pizzas.price)),
            2) AS revenue
FROM
    pizzas
        JOIN
    order_details ON pizzas.pizza_id = order_details.pizza_id
        JOIN
    pizza_types ON pizza_types.pizza_type_id = pizzas.pizza_type_id
GROUP BY pizza_types.name
ORDER BY revenue DESC
LIMIT 3;
```
#### Result
![enter image description here](https://blogger.googleusercontent.com/img/a/AVvXsEi0WVL5t4OznlEiYOqJHGwRbiSDEQIvmQPQX4QxuQTo56IDkk8jzuyDvCl-VEvIitKx2sfmAM41wNXJiccmwZrYQ75n3Mq5O_TiveMPDui83jHf0PXZv3P4KMcm0x98we6Likx7TofgfbietiKRQzNann-TFguG8bjZFCQFHNb9wyW4IrVJu15rxJ4uH14=w414-h137)
---
### 11. Calculate the percentage contribution of each pizza type to total revenue
#### Query
```sql
SELECT 
    pizza_types.category,
    round((SUM((order_details.quantity * pizzas.price)) /
    (SELECT 
    ROUND(SUM(quantity * price), 2) AS Total_Revenue
FROM
    order_details
        JOIN
    pizzas ON order_details.pizza_id = pizzas.pizza_id))*100, 2) as revenue_pct
FROM
    pizza_types
        JOIN
	pizzas ON pizza_types.pizza_type_id = pizzas.pizza_type_id
    join
    order_details ON pizzas.pizza_id = order_details.pizza_id
   
GROUP BY pizza_types.category
ORDER BY revenue_pct DESC;
```
#### Result
![enter image description here](https://blogger.googleusercontent.com/img/a/AVvXsEh3vvrasiawKC6FTRZ6hYe2-rcitWPe3E5qmXYhbLJ-JwGrUsjHPwgHt55JmIpgdT30WI9CYuVUR-adJeqmbB6TyDZqA2pnNJHWeVVfSbQGDb0ZeWLAWy7eKBHD0TuWzDn-2BommC6eAnv_-qUglJMPuFSGKVTc8uI9mHGAjAdOfPkAdJbik3HRuN7iYyQ=w398-h256)
---
### 12. Determine the top 3 most ordered pizzas based on revenue for each category
#### Query
```sql
SELECT
	name, category, revenue
FROM
(SELECT category, name, revenue,
	RANK() OVER(PARTITION BY category ORDER BY revenue DESC) AS rn
FROM
(SELECT 
    pizza_types.category,
	pizza_types.name,
    ROUND(SUM((order_details.quantity * pizzas.price)),0)
    AS revenue
FROM
    pizzas
        JOIN
    order_details ON pizzas.pizza_id = order_details.pizza_id
        JOIN
    pizza_types ON pizza_types.pizza_type_id = pizzas.pizza_type_id
GROUP BY pizza_types.category, pizza_types.name) as a) as b
where rn<=3;
```
#### Result
![enter image description here](https://blogger.googleusercontent.com/img/a/AVvXsEg6z2nP9mcl0fjTui4RdB6qEKEzAv626gudwva0YSWAwRIE8Z6CfpW8QcZhl5Dqn7uZgCwPkwcQ3Cgcc4Xf9F333UXdN2bEPgN_e4fRBF1Q1QalnuwmQd2no9GyLDfr_CPsszW1AmgXCiPTlNToHFf1ByastAmQTIBHa5o38iPgdAUr11HrFQg08soz6UA=w430-h356)
---

## Recommendations based on Insights
-   Identified peak rush hours as 12 PM to 2 PM and 5 PM to 8 PM, recommending optimal stock availability and full staff engagement during these times to meet high demand.
-   Discovered that while Classic pizzas are the most popular, Chicken pizzas generate the highest revenue, suggesting the implementation of discount or combo offers to drive further growth.
---
[Follow me on Linkedin](https://www.linkedin.com/in/mandipdevnath/)
