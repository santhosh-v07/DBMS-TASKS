# Task IV - Order Management System

## Objective

Design and manage an Order Management System using `orders` and `order_details` tables.

This task covers:

- Creating Orders and Order Details tables
- Managing customer product orders
- Storing order date, quantity, price, and total amount
- Performing order insertion and modification operations
- Generating customer order history reports

> **Prerequisite:** This task assumes that the `customer` table contains `customer_id` and `customer_name`, and the `product` table from Task II contains `product_id`, `product_name`, and `price`.

---

## 1. Create Orders Table

```sql
create table orders(
    order_id int primary key auto_increment,
    customer_id int not null,
    order_date datetime not null default current_timestamp,
    total_amount decimal(12,2) not null default 0.00,
    order_status varchar(30) not null default 'Placed',
    foreign key(customer_id)
    references customer(customer_id)
);
```

The `orders` table stores the main details of each customer order.

---

## 2. Create Order Details Table

```sql
create table order_details(
    order_detail_id int primary key auto_increment,
    order_id int not null,
    product_id int not null,
    quantity int not null,
    unit_price decimal(10,2) not null,
    line_total decimal(12,2) not null,
    foreign key(order_id)
    references orders(order_id)
    on delete cascade,
    foreign key(product_id)
    references product(product_id),
    check(quantity > 0)
);
```

The `order_details` table stores the products included in each order along with quantity and price details.

---

## 3. Insert a Customer Order

The following example creates an order for customer `1` and adds products to it.

```sql
insert into orders(customer_id, order_date, total_amount)
values(1, current_timestamp, 0.00);

set @order_id = last_insert_id();
```

### Add Products to the Order

```sql
insert into order_details(order_id, product_id, quantity, unit_price, line_total)
select @order_id, product_id, 1, price, price * 1
from product
where product_id = 1;

insert into order_details(order_id, product_id, quantity, unit_price, line_total)
select @order_id, product_id, 2, price, price * 2
from product
where product_id = 3;
```

### Calculate and Store the Order Total

```sql
update orders
set total_amount = (
    select sum(line_total)
    from order_details
    where order_id = @order_id
)
where order_id = @order_id;
```

---

## 4. View Orders

```sql
select * from orders;
```

```sql
select * from order_details;
```

---

## 5. Modify an Existing Order

Example: Change the quantity of product `3` to `3` units.

```sql
update order_details od
join product p
on od.product_id = p.product_id
set od.quantity = 3,
    od.unit_price = p.price,
    od.line_total = p.price * 3
where od.order_id = @order_id
and od.product_id = 3;
```

Recalculate the total amount after modification.

```sql
update orders
set total_amount = (
    select sum(line_total)
    from order_details
    where order_id = @order_id
)
where order_id = @order_id;
```

---

## 6. Change Order Status

```sql
update orders
set order_status = 'Shipped'
where order_id = @order_id;
```

Possible order statuses can include:

- Placed
- Confirmed
- Shipped
- Delivered
- Cancelled

---

## 7. Customer Order History Report

This query displays complete order history including customer, order, product, quantity, and amount details.

```sql
select
    c.customer_id,
    c.customer_name,
    o.order_id,
    o.order_date,
    o.order_status,
    p.product_name,
    od.quantity,
    od.unit_price,
    od.line_total,
    o.total_amount
from customer c
join orders o
    on c.customer_id = o.customer_id
join order_details od
    on o.order_id = od.order_id
join product p
    on od.product_id = p.product_id
order by o.order_date desc, o.order_id desc;
```

---

## 8. Order History of a Specific Customer

Example: Display all orders placed by customer `1`.

```sql
select
    o.order_id,
    o.order_date,
    o.order_status,
    p.product_name,
    od.quantity,
    od.unit_price,
    od.line_total,
    o.total_amount
from orders o
join order_details od
    on o.order_id = od.order_id
join product p
    on od.product_id = p.product_id
where o.customer_id = 1
order by o.order_date desc;
```

---

## 9. Customer Order Summary Report

This query shows the total number of orders and total amount spent by each customer.

```sql
select
    c.customer_id,
    c.customer_name,
    count(o.order_id) as total_orders,
    coalesce(sum(o.total_amount), 0) as total_amount_spent
from customer c
left join orders o
    on c.customer_id = o.customer_id
group by c.customer_id, c.customer_name
order by total_amount_spent desc;
```

---

## 10. Order Summary

This query shows each order with the total number of items ordered.

```sql
select
    o.order_id,
    o.customer_id,
    o.order_date,
    sum(od.quantity) as total_items,
    o.total_amount,
    o.order_status
from orders o
join order_details od
    on o.order_id = od.order_id
group by
    o.order_id,
    o.customer_id,
    o.order_date,
    o.total_amount,
    o.order_status
order by o.order_date desc;
```

---

## Result

The Order Management System successfully supports:

- Customer order creation
- Multiple products in a single order
- Quantity and price management
- Automatic calculation of line totals and order totals
- Order modification and status updates
- Customer-wise order history
- Customer purchase summary reports
