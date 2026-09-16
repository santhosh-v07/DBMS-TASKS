# Task VI - Product Review and Rating Management System

## Review Table

The review table stores feedback given by customers for products.

```sql
create table review(
    review_id int primary key auto_increment,
    customer_id int not null,
    product_id int not null,
    review_text varchar(500),
    review_date date,
    foreign key(customer_id) references customer(customer_id),
    foreign key(product_id) references product(product_id)
);
```

## Rating Table

The rating table stores the rating given by a customer for a product.

```sql
create table rating(
    rating_id int primary key auto_increment,
    customer_id int not null,
    product_id int not null,
    rating_value int not null,
    rating_date date,
    foreign key(customer_id) references customer(customer_id),
    foreign key(product_id) references product(product_id),
    check(rating_value between 1 and 5)
);
```

## Insert Review Details

```sql
insert into review(customer_id,product_id,review_text,review_date)
values
(1,1,'Good product and camera quality is nice','2026-09-15'),
(2,2,'Performance is good','2026-09-15'),
(3,3,'Sound quality is good for this price','2026-09-16'),
(1,3,'Battery backup is average','2026-09-16');
```

## Insert Ratings

```sql
insert into rating(customer_id,product_id,rating_value,rating_date)
values
(1,1,5,'2026-09-15'),
(2,2,4,'2026-09-15'),
(3,3,4,'2026-09-16'),
(1,3,3,'2026-09-16'),
(2,1,5,'2026-09-16');
```

## View Reviews

```sql
select * from review;
```

## View Ratings

```sql
select * from rating;
```

## Product Review Details

This query shows the product name, customer name and review given by the customer.

```sql
select
    p.product_name,
    c.customer_name,
    r.review_text,
    r.review_date
from review r
join product p
on r.product_id = p.product_id
join customer c
on r.customer_id = c.customer_id
order by r.review_date desc;
```

## Reviews for a Particular Product

Example for product id 1.

```sql
select review_text,review_date
from review
where product_id = 1;
```

## Average Rating of Products

```sql
select
    p.product_id,
    p.product_name,
    avg(r.rating_value) as average_rating
from product p
join rating r
on p.product_id = r.product_id
group by p.product_id,p.product_name;
```

## Highly Rated Products

Products with an average rating of 4 or more are displayed here.

```sql
select
    p.product_name,
    avg(r.rating_value) as average_rating
from product p
join rating r
on p.product_id = r.product_id
group by p.product_id,p.product_name
having avg(r.rating_value) >= 4
order by average_rating desc;
```

## Number of Ratings for Each Product

```sql
select
    p.product_name,
    count(r.rating_id) as total_ratings
from product p
left join rating r
on p.product_id = r.product_id
group by p.product_id,p.product_name;
```

## Result

The Review and Rating Management System is used to:

- Store customer reviews
- Store ratings from 1 to 5
- View feedback given for products
- Find the average rating of each product
- Identify highly rated products
