==question 1: 130==
```select count(distinct user_id) from users;```

==question 2: 15==
```
with orders_by_hour as (
  select date_part('hour',created_at) as order_hour, count(distinct order_id) as order_count
  from orders
  group by 1)
select round(sum(order_count)/count(distinct order_hour)) from orders_by_hour oh
; 
```
==question 3: 3 days 21:24:11.803279==
```
with 
  orders_placed as (select order_id, date_trunc('day',created_at) from events where event_type = 'checkout')
  , orders_delivered as (select order_id, date_trunc('day',delivered_at) from orders where status = 'delivered')
select *
from orders_delivered od 
left join orders_placed op on od.order_id = op.order_id
```
==question 4: {1:25,2:28,3:71}==
```
with orders_by_user as (
  select  user_id, 
          count(distinct order_id) order_count
  from  orders o 
  group by 1)
select 
  case 
    when obu.order_count<3 then cast(order_count as varchar) 
    else cast(3 as varchar)
  end as order_count_ntile, 
  count(obu.user_id)
from orders_by_user obu
group by 1;
```
==qusetion 5: 39==
```with sess_by_hour as (
  select date_part('hour',created_at) as session_hour, 
  count(distinct session_id) as session_count
  from events
  group by 1)
select round(sum(session_count)/count(distinct session_hour)) 
from sess_by_hour sh
;
```