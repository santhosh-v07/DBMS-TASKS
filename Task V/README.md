# Task V - Payment Transaction Management System

## Payment Table

The payment table is used to store payment details for customer orders.

```sql
create table payment(
    payment_id int primary key auto_increment,
    order_id int not null,
    payment_mode varchar(30) not null,
    payment_date datetime default current_timestamp,
    amount decimal(10,2) not null,
    payment_status varchar(20) not null,
    foreign key(order_id) references orders(order_id)
);
```

## Insert Payment Details

Below are some sample payment records.

> The order ids used below are assumed to already exist in the `orders` table.

```sql
insert into payment(order_id,payment_mode,payment_date,amount,payment_status)
values
(1,'UPI','2026-09-15 10:30:00',69999.00,'Success'),
(2,'Credit Card','2026-09-15 11:15:00',64999.00,'Success'),
(3,'UPI','2026-09-15 12:20:00',2999.00,'Failed'),
(4,'Debit Card','2026-09-16 09:45:00',32999.00,'Success'),
(5,'Net Banking','2026-09-16 10:10:00',399.00,'Failed');
```

## View All Payments

```sql
select * from payment;
```

## Successful Transactions

```sql
select * from payment
where payment_status = 'Success';
```

## Failed Transactions

```sql
select * from payment
where payment_status = 'Failed';
```

## Update Payment Status

If a failed payment is completed later, its status can be updated.

```sql
update payment
set payment_status = 'Success'
where payment_id = 3;
```

## Payment Methods Used by Customers

This query shows how many times each payment method was used.

```sql
select payment_mode,count(*) as total_payments
from payment
group by payment_mode;
```

## Successful Amount by Payment Mode

```sql
select payment_mode,sum(amount) as total_amount
from payment
where payment_status = 'Success'
group by payment_mode;
```

## Payment Transaction Report

This report shows payment details along with order and customer information.

```sql
select
    p.payment_id,
    o.order_id,
    c.customer_name,
    p.payment_mode,
    p.payment_date,
    p.amount,
    p.payment_status
from payment p
join orders o
on p.order_id = o.order_id
join customer c
on o.customer_id = c.customer_id
order by p.payment_date desc;
```

## Payment Status Report

This query gives the number of successful and failed payments.

```sql
select payment_status,count(*) as total_transactions
from payment
group by payment_status;
```

## Total Successful Payment Amount

```sql
select sum(amount) as total_successful_amount
from payment
where payment_status = 'Success';
```

## Result

The Payment Transaction Management System can be used to:

- Store payment mode, date, amount and status
- Identify successful and failed transactions
- Update payment status
- Find commonly used payment methods
- Generate customer payment transaction reports
