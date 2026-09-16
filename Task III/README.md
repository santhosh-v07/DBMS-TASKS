# Task III - Seller and Inventory Management System

This task is used to manage sellers, their products and available stock.

## Seller Table

```sql
create table seller(
    seller_id int primary key auto_increment,
    seller_name varchar(100) not null,
    phone varchar(15),
    email varchar(100),
    city varchar(50)
);
```

## Inventory Table

The inventory table connects sellers with products and stores stock information.

```sql
create table inventory(
    inventory_id int primary key auto_increment,
    seller_id int not null,
    product_id int not null,
    seller_price decimal(10,2) not null,
    stock_quantity int not null,
    stock_status varchar(20) not null,
    foreign key(seller_id) references seller(seller_id),
    foreign key(product_id) references product(product_id)
);
```

## Insert Seller Details

```sql
insert into seller(seller_name,phone,email,city)
values
('Mobile World','9876543210','mobileworld@gmail.com','Chennai'),
('Tech Store','9876501234','techstore@gmail.com','Coimbatore'),
('Fashion Hub','9876512345','fashionhub@gmail.com','Madurai');
```

## Insert Inventory Details

> Product ids are assumed to already exist in the `product` table created in Task II.

```sql
insert into inventory(seller_id,product_id,seller_price,stock_quantity,stock_status)
values
(1,1,69999.00,10,'Available'),
(1,2,64999.00,8,'Available'),
(2,3,2999.00,20,'Available'),
(2,4,32999.00,0,'Unavailable'),
(3,5,399.00,30,'Available');
```

## View Sellers

```sql
select * from seller;
```

## View Inventory

```sql
select * from inventory;
```

## Seller Product Information

```sql
select
    s.seller_name,
    p.product_name,
    i.seller_price,
    i.stock_quantity,
    i.stock_status
from inventory i
join seller s
on i.seller_id = s.seller_id
join product p
on i.product_id = p.product_id;
```

## Available Products

```sql
select
    p.product_name,
    s.seller_name,
    i.stock_quantity
from inventory i
join product p
on i.product_id = p.product_id
join seller s
on i.seller_id = s.seller_id
where i.stock_quantity > 0;
```

## Unavailable Products

```sql
select
    p.product_name,
    s.seller_name
from inventory i
join product p
on i.product_id = p.product_id
join seller s
on i.seller_id = s.seller_id
where i.stock_quantity = 0;
```

## Update Stock

Example: update the stock of inventory id 4.

```sql
update inventory
set stock_quantity = 5,
    stock_status = 'Available'
where inventory_id = 4;
```

## Mark Product as Unavailable

```sql
update inventory
set stock_quantity = 0,
    stock_status = 'Unavailable'
where inventory_id = 2;
```

## Inventory Status Report

```sql
select
    s.seller_name,
    p.product_name,
    i.seller_price,
    i.stock_quantity,
    i.stock_status
from inventory i
join seller s
on i.seller_id = s.seller_id
join product p
on i.product_id = p.product_id
order by s.seller_name;
```

## Total Stock for Each Seller

```sql
select
    s.seller_name,
    sum(i.stock_quantity) as total_stock
from seller s
join inventory i
on s.seller_id = i.seller_id
group by s.seller_id,s.seller_name;
```

## Low Stock Products

```sql
select
    p.product_name,
    s.seller_name,
    i.stock_quantity
from inventory i
join product p
on i.product_id = p.product_id
join seller s
on i.seller_id = s.seller_id
where i.stock_quantity < 10
and i.stock_quantity > 0;
```

## Result

The Seller and Inventory Management System can be used to:

- Store seller details
- Connect sellers with products
- Maintain seller product price and stock
- Find available and unavailable products
- Update inventory stock
- Generate inventory status reports
