# Task VII - SQL Query Implementation for E-Commerce Database

This task contains some basic SQL queries used in an e-commerce database.

## 1. Select All Products

```sql
select * from product;
```

## 2. Select Product Name and Price

```sql
select product_name,price
from product;
```

## 3. Products Above a Particular Price

Example: Products with price greater than 5000.

```sql
select * from product
where price > 5000;
```

## 4. Products Below a Particular Price

```sql
select * from product
where price < 1000;
```

## 5. Order Products by Price

```sql
select product_name,price
from product
order by price asc;
```

To display costly products first:

```sql
select product_name,price
from product
order by price desc;
```

## 6. Display Different Brands

```sql
select distinct brand
from product;
```

## 7. Search Product by Category

Example: Display products from category id 2.

```sql
select * from product
where category_id = 2;
```

## 8. Search Product Between Price Range

```sql
select * from product
where price between 1000 and 50000;
```

## 9. Available Products

Products with stock greater than zero are available.

```sql
select product_name,price,stock
from product
where stock > 0;
```

## 10. Products with Low Stock

```sql
select product_name,stock
from product
where stock < 20;
```

## 11. Search Using More Than One Condition

Example: Products from category 1 with price below 40000.

```sql
select * from product
where category_id = 1
and price < 40000;
```

## 12. Retrieve Customer Information

```sql
select * from customer;
```

## 13. Retrieve Customer Name and Contact Details

```sql
select customer_name,email,phone
from customer;
```

## 14. Product and Category Information

```sql
select
    p.product_id,
    p.product_name,
    c.category_name,
    p.price,
    p.stock
from product p
join category c
on p.category_id = c.category_id;
```

## 15. Products Starting with a Letter

Example: Product names starting with S.

```sql
select * from product
where product_name like 'S%';
```

## 16. Total Number of Products

```sql
select count(*) as total_products
from product;
```

## 17. Total Products in Each Category

```sql
select
    c.category_name,
    count(p.product_id) as total_products
from category c
left join product p
on c.category_id = p.category_id
group by c.category_id,c.category_name;
```

## 18. Average Product Price

```sql
select avg(price) as average_price
from product;
```

## 19. Most Expensive Product Price

```sql
select max(price) as highest_price
from product;
```

## 20. Cheapest Product Price

```sql
select min(price) as lowest_price
from product;
```

## 21. Total Stock Available

```sql
select sum(stock) as total_stock
from product;
```

## 22. Customer Order Report

```sql
select
    c.customer_name,
    o.order_id,
    o.order_date,
    o.total_amount,
    o.order_status
from customer c
join orders o
on c.customer_id = o.customer_id
order by o.order_date desc;
```

## 23. Total Orders Placed by Each Customer

```sql
select
    c.customer_name,
    count(o.order_id) as total_orders
from customer c
left join orders o
on c.customer_id = o.customer_id
group by c.customer_id,c.customer_name;
```

## 24. Total Sales Amount

```sql
select sum(total_amount) as total_sales
from orders;
```

## Result

Using these SQL queries we can:

- Display required records using SELECT
- Filter data using WHERE
- Sort records using ORDER BY
- Find unique values using DISTINCT
- Search products by price, category and availability
- Retrieve customer and product details
- Generate simple reports using aggregate functions
