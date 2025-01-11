# Website Traffic Analysis

<br>

## A. Session and User Analysis

#### `To retrieve all unique combinations of traffic sources, campaigns, and content`
```sql
select
    distinct source,campaign,content
from sessions
order by 1;
```
| source   | campaign | content      |
|----------|----------|--------------|
| Bing     | brand    | Bing_Ad_2    |
| Bing     | nonbrand | Bing_Ad_1    |
| Google   | brand    | Google_Ad_2  |
| Google   | nonbrand | Google_Ad_1  |
| Yahoo    | brand    | Yahoo_Ad     |

#### `How many sessions do we have across sources?`
```sql
select
    source,
    count(session_id) as Number_of_sessions
from sessions
group by 1
order by 2 desc;
```
|  source  | Number_of_sessions  |  
|----------|---------------------|
| Google   | 59639               |   
| Bing     | 11323               |   
| Yahoo    | 8522                |   

#### `How many sessions do we have across campaigns?`
```sql
select
    campaign,
    count(session_id) as Number_of_sessions
from sessions
group by 1
order by 2 desc;
```
 campaign | Number_of_sessions  
----------|---------------------
 nonbrand | 73282               
 brand    | 6202                

#### `How many sessions do we have across devices?`
```sql
select
    device,
    count(session_id) as Number_of_sessions
from sessions
group by 1
order by 2 desc;
```
 device  | Number_of_sessions  
---------|---------------------
 desktop | 58823               
 mobile  | 20117               
 tablet  | 544                 
#### `Which source and campaign are driving more sessions?`
```sql
select
    source,
    campaign,
    count(session_id) as Number_of_sessions
from sessions
group by 1,2
order by 3 desc;
```
| source | campaign | Number_of_sessions  |
|--------|----------|---------------------|
| Google | nonbrand | 56262               |
| Bing   | nonbrand | 10511               |
| Yahoo  | nonbrand | 6509                |
| Google | brand    | 3377                |
| Yahoo  | brand    | 2013                |
| Bing   | brand    | 812                 |
#### `Which source and device are driving more sessions?`
```sql
select
    source,
    device,
    count(session_id) as Number_of_sessions
from sessions
group by 1,2
order by 3 desc;
```
| source | device  | Number_of_sessions  |
|--------|---------|---------------------|
| Google | desktop | 43391               |
| Google | mobile  | 15884               |
| Bing   | desktop | 10176               |
| Yahoo  | desktop | 5256                |
| Yahoo  | mobile  | 3176                |
| Bing   | mobile  | 1057                |
| Google | tablet  | 364                 |
| Yahoo  | tablet  | 90                  |
| Bing   | tablet  | 90                  |
#### `Which campaign and device are driving more sessions?`
```sql
select
    campaign,
    device,
    count(session_id) as Number_of_sessions
from sessions
group by 1,2
order by 3 desc;
```
| campaign | device  | Number_of_sessions  |
|----------|---------|---------------------|
| nonbrand | desktop | 54976               |
| nonbrand | mobile  | 18001               |
| brand    | desktop | 3847                |
| brand    | mobile  | 2116                |
| nonbrand | tablet  | 305                 |
| brand    | tablet  | 239                 |
#### `What are the contributions of source and campaign across quarters?`
```sql
select
    year(date) as yr,
    quarter(date) as mn,
    count(session_id) as Number_of_sessions,
    count(distinct case when source = 'Google' and campaign = 'nonbrand' then session_id else null end) as Google_Nonbrand,
	count(distinct case when source = 'Google' and campaign = 'brand' then session_id else null end) as Google_Brand,
    count(distinct case when source = 'Bing' and campaign = 'nonbrand' then session_id else null end) as Bing_Nonbrand,
    count(distinct case when source = 'Bing' and campaign = 'brand' then session_id else null end) as Bing_Brand,
    count(distinct case when source = 'Yahoo' and campaign = 'nonbrand' then session_id else null end) as Yahoo_Nonbrand,
    count(distinct case when source = 'Yahoo' and campaign = 'brand' then session_id else null end) as Yahoo_Brand
from sessions
group by 1,2;
```
| yr   | mn | Number_of_sessions | Google_Nonbrand | Google_Brand | Bing_Nonbrand | Bing_Brand | Yahoo_Nonbrand | Yahoo_Brand  |
|------|----|--------------------|-----------------|--------------|---------------|------------|----------------|--------------|
| 2012 | 1  | 1879               | 1852            | 8            | 0             | 2          | 0              | 17           |
| 2012 | 2  | 11433              | 10243           | 319          | 0             | 61         | 0              | 810          |
| 2012 | 3  | 16892              | 12560           | 619          | 2009          | 179        | 339            | 1186         |
| 2012 | 4  | 32266              | 20949           | 1338         | 6260          | 318        | 3401           | 0            |
| 2013 | 1  | 17014              | 10658           | 1093         | 2242          | 252        | 2769           | 0            |
#### `What are the contribution of sessions over time?`
```sql
select
    year(date) as Yr,
    month(date) as mn,
    count(session_id) as Number_of_sessions
from sessions
group by 1,2;
```
| Yr   | mn | Number_of_sessions  |
|------|----|---------------------|
| 2012 | 3  | 1879                |
| 2012 | 4  | 3734                |
| 2012 | 5  | 3736                |
| 2012 | 6  | 3963                |
| 2012 | 7  | 4249                |
| 2012 | 8  | 6097                |
| 2012 | 9  | 6546                |
| 2012 | 10 | 8183                |
| 2012 | 11 | 14011               |
| 2012 | 12 | 10072               |
| 2013 | 1  | 6401                |
| 2013 | 2  | 7168                |
| 2013 | 3  | 3445                |
#### `How many sessions across devices by quarter?`
```sql
select
    year(date) as yr,
    quarter(date) as mn,
    count(session_id) as Number_of_sessions,
    count(distinct case when device = 'Desktop' then session_id else null end) as Desktop_sessions,
    count(distinct case when device = 'Mobile' then session_id else null end) as Mobile_sessions,
    count(distinct case when device = 'Tablet' then session_id else null end) as Tablet_sessions
from sessions
group by 1,2;
```
| yr   | mn | Number_of_sessions | Desktop_sessions | Mobile_sessions | Tablet_sessions  |
|------|----|--------------------|------------------|-----------------|------------------|
| 2012 | 1  | 1879               | 1104             | 713             | 62               |
| 2012 | 2  | 11433              | 7795             | 3556            | 82               |
| 2012 | 3  | 16892              | 12718            | 4131            | 43               |
| 2012 | 4  | 32266              | 24626            | 7370            | 270              |
| 2013 | 1  | 17014              | 12580            | 4347            | 87               |
#### `Which are the pageviews that a customer can visit?`
```sql
select
    distinct url
from pageviews;
```
| url                  |
|----------------------|
| /home                |
| /products            |
| /Ironbark Ale        |
| /cart                |
| /order_placed        |
| /billing             |
| /order_confirmed     |
| /home-1              |
| /billing-1           |
| /Emberstone Whiskey  |
| /home-2              |
#### `When were the additional pages launched for A/B test?`
```sql
select
    url,
    min(date) as Launch_date,
    min(pageview_id) as first_pageview
from pageviews
where url in ('/home-1','/billing-1','/home-2')
group by url;
```
| url        | Launch_date | first_pageview  |
|------------|-------------|-----------------|
| /home-1    | 2012-06-19  | 23504           |
| /billing-1 | 2012-09-10  | 53550           |
| /home-2    | 2013-01-14  | 142442          |
#### `How many sessions did we have before launching a new /home-1 page?`
```sql
select
    url,
    count(session_id__) as Number_of_sessions
from pageviews
where date < '2012-06-19'
group by 1
order by 2 desc;
```
| url              | Number_of_sessions  |
|------------------|---------------------|
| /home            | 11682               |
| /products        | 4789                |
| /Ironbark Ale    | 3418                |
| /cart            | 1476                |
| /order_placed    | 979                 |
| /billing         | 807                 |
| /order_confirmed | 349                 |
#### `How many sessions did we have across landing pages before launching a new /home-1 page?`
```sql
With cte as (
select
    session_id__,
    min(pageview_id) as first_pageview 
from pageviews
where date < '2012-06-19'
group by session_id__
)

select 
    pageviews.url as landing_page,
	count(cte.session_id__) as sessions_on_landing_page
from cte
left join pageviews on cte.first_pageview = pageviews.pageview_id
group by pageviews.url
order by 2 desc;
```
| landing_page | sessions_on_landing_page  |
|--------------|---------------------------|
| /home        | 11663                     |
| /products    | 19                        |
#### `How many new and returning users do we have?`
```sql
select 
    year(date) as yr,
    month(date) as mn,
    count(case when repeat_user = 1 then user_id end) as Returning_users,
    count(case when repeat_user = 0 then user_id end) as New_users
from sessions 
group by 1,2
order by 1,2;
```
| yr   | mn | Returning_users | New_users  |
|------|----|-----------------|------------|
| 2012 | 3  | 8               | 1871       |
| 2012 | 4  | 150             | 3584       |
| 2012 | 5  | 350             | 3386       |
| 2012 | 6  | 405             | 3558       |
| 2012 | 7  | 442             | 3807       |
| 2012 | 8  | 535             | 5562       |
| 2012 | 9  | 666             | 5880       |
| 2012 | 10 | 890             | 7293       |
| 2012 | 11 | 1003            | 13008      |
| 2012 | 12 | 1478            | 8594       |
| 2013 | 1  | 1601            | 4800       |
| 2013 | 2  | 1024            | 6144       |
| 2013 | 3  | 539             | 2906       |
#### `What is the Bounce rate for customers on the landing page?`
```sql
with cte as (
    select
        p.session_id__,
        p.pageview_id,
        p.url,
        row_number() over (partition by p.session_id__ order by p.pageview_id) as row_num,
        count(p.pageview_id) over (partition by p.session_id__) as total_views
    from pageviews p
    left join sessions s on p.session_id__ = s.session_id
),
cte_1 as (
    select
        session_id__,
        url as Landing_page,
        total_views
    from cte
    where row_num = 1
),
final as (
    select
        Landing_page,
        count(distinct session_id__) as Total_sessions,
        count(distinct case when total_views != 1 then session_id__ end) as Next_page_sessions,
        round(count(distinct case when total_views != 1 then session_id__ end)
			/count(distinct session_id__)*100,2) as Next_page_rate,
        count(distinct case when total_views = 1 then session_id__ end) as Bounced_sessions,
        round(count(distinct case when total_views = 1 then session_id__ end)
			/count(distinct session_id__)*100,2) as Bounce_rate
    from cte_1
    group by 1
)

select 
    * 
from final
order by Bounce_rate desc;
```
| Landing_page  | Total_sessions | Next_page_sessions | Next_page_rate | Bounced_sessions | Bounce_rate  |
|---------------|----------------|--------------------|----------------|------------------|--------------|
| /home-1       | 47422          | 22092              | 46.59          | 25330            | 53.41        |
| /home         | 25665          | 12806              | 49.90          | 12859            | 50.10        |
| /home-2       | 6169           | 3200               | 51.87          | 2969             | 48.13        |
| /Ironbark Ale | 2              | 2                  | 100.00         | 0                | 0.00         |
| /products     | 226            | 226                | 100.00         | 0                | 0.00         |

<br>

## B. Product Orders and Refund Analysis

#### `When were our products launched?`
```sql
select
    *
from products;
```
| product_id | date       | brand               |
|------------|------------|---------------------|
| 1          | 2012-03-19 | Ironbark Ale        |
| 2          | 2013-01-06 | Emberstone Whiskey  |
#### `How many orders did we receive over time?`
```sql
select
    year(date) as yr,
    month(date) as mn,
    count(order_id) as Number_of_orders
from orders
group by 1,2;
```
| yr   | mn | Number_of_orders  |
|------|----|-------------------|
| 2012 | 3  | 60                |
| 2012 | 4  | 99                |
| 2012 | 5  | 108               |
| 2012 | 6  | 140               |
| 2012 | 7  | 169               |
| 2012 | 8  | 228               |
| 2012 | 9  | 287               |
| 2012 | 10 | 371               |
| 2012 | 11 | 618               |
| 2012 | 12 | 506               |
| 2013 | 1  | 390               |
| 2013 | 2  | 498               |
| 2013 | 3  | 202               |
#### `How many orders did we receive and what is our sales statistics?`
```sql
select
    count(order_id) as Number_of_orders,
	round(sum(price_in_usd),2) as Revenue,
    round(sum(price_in_usd-cogs_in_usd),2) as Margin,
    round(avg(price_in_usd),2) as Average_usd_per_order
from orders;
```
| Number_of_orders | Revenue  | Margin  | Average_usd_per_order  |
|------------------|----------|---------|------------------------|
| 3676             | 62481.67 | 29966.6 | 17                     |
#### `What is the order to sales split over time?`
```sql
select
    year(date) as yr,
    month(date) as mn,
    count(order_id) as Number_of_orders,
    round(sum(price_in_usd),2) as Revenue,
    round(sum(price_in_usd-cogs_in_usd),2) as Margin
from orders
where date < '2013-01-06'
group by 1,2;
```
| yr   | mn | Number_of_orders | Revenue | Margin  |
|------|----|------------------|---------|---------|
| 2012 | 3  | 60               | 899.4   | 420     |
| 2012 | 4  | 99               | 1484.01 | 693     |
| 2012 | 5  | 108              | 1618.92 | 756     |
| 2012 | 6  | 140              | 2098.6  | 980     |
| 2012 | 7  | 169              | 2533.31 | 1183    |
| 2012 | 8  | 228              | 3417.72 | 1596    |
| 2012 | 9  | 287              | 4302.13 | 2009    |
| 2012 | 10 | 371              | 5561.29 | 2597    |
| 2012 | 11 | 618              | 9263.82 | 4326    |
| 2012 | 12 | 506              | 7584.94 | 3542    |
| 2013 | 1  | 63               | 944.37  | 441     |
#### `How many customers bought products split over quantity and also sales statistics?`
```sql
select
	products_purchased,
    count(order_id) as Number_of_orders,
    round(sum(price_in_usd),2) as Revenue,
    round(avg(price_in_usd),2) as Average_usd_per_order
from orders
where date > '2013-01-06'
group by 1;
```
| products_purchased | Number_of_orders | Revenue  | Average_usd_per_order  |
|--------------------|------------------|----------|------------------------|
| 1                  | 859              | 15938.91 | 18.56                  |
| 2                  | 157              | 6669.36  | 42.48                  |
#### `How much is the session to order conversion rate?`
```sql
select
    count(sessions.session_id) as Number_of_session,
    count(orders.order_id) as Number_of_orders,
    round(count(orders.order_id)/count(sessions.session_id)*100,2) Conversion_rate
from sessions
left join orders on sessions.session_id = orders.session_id_;
```
| Number_of_session | Number_of_orders | Conversion_rate  |
|-------------------|------------------|------------------|
| 79484             | 3676             | 4.62             |
#### `How much is the session to order conversion rate by source?`
```sql
select
    sessions.source,
	count(sessions.session_id) as Number_of_sessions,
    count(orders.order_id) as Number_of_orders,
    round(count(orders.order_id)/count(sessions.session_id)*100,2) as Conversion_rate
from sessions
left join orders on sessions.session_id = orders.session_id_
group by 1
order by 4 desc;
```
| source | Number_of_sessions | Number_of_orders | Conversion_rate  |
|--------|--------------------|------------------|------------------|
| Yahoo  | 8522               | 483              | 5.67             |
| Bing   | 11323              | 601              | 5.31             |
| Google | 59639              | 2592             | 4.35             |
#### `How much is the session to order conversion rate by campaign?`
```sql
select
    sessions.campaign,
	count(sessions.session_id) as Number_of_sessions,
    count(orders.order_id) as Number_of_orders,
    round(count(orders.order_id)/count(sessions.session_id)*100,2) as Conversion_rate
from sessions
left join orders on sessions.session_id = orders.session_id_
group by 1
order by 4 desc;
```
| campaign | Number_of_sessions | Number_of_orders | Conversion_rate  |
|----------|--------------------|------------------|------------------|
| brand    | 6202               | 354              | 5.71             |
| nonbrand | 73282              | 3322             | 4.53             |
#### `How much is the session to order conversion rate by device?`
```sql
select
    sessions.device,
	count(sessions.session_id) as Number_of_sessions,
    count(orders.order_id) as Number_of_orders,
    round(count(orders.order_id)/count(sessions.session_id)*100,2) as Conversion_rate
from sessions
left join orders on sessions.session_id = orders.session_id_
group by 1
order by 4 desc;
```
| device  | Number_of_sessions | Number_of_orders | Conversion_rate  |
|---------|--------------------|------------------|------------------|
| desktop | 58823              | 3297             | 5.60             |
| tablet  | 544                | 22               | 4.04             |
| mobile  | 20117              | 357              | 1.77             |
#### `How much is the sessions to order conversion rate by source and campaign?`
```sql
select
    sessions.source,
    sessions.campaign,
    count(sessions.session_id) as Number_of_sessions,
    count(orders.order_id) as Number_of_orders,
    count(orders.order_id)/count(sessions.session_id)*100 as Conversion_rate
from sessions
left join orders on sessions.session_id = orders.session_id_
group by 1,2
order by 5 desc;
```
| source | campaign | Number_of_sessions | Number_of_orders | Conversion_rate  |
|--------|----------|--------------------|------------------|------------------|
| Bing   | brand    | 812                | 51               | 6.2808           |
| Google | brand    | 3377               | 208              | 6.1593           |
| Yahoo  | nonbrand | 6509               | 388              | 5.9610           |
| Bing   | nonbrand | 10511              | 550              | 5.2326           |
| Yahoo  | brand    | 2013               | 95               | 4.7193           |
| Google | nonbrand | 56262              | 2384             | 4.2373           |
#### `Which product has been sold the most?`
```sql
select
    items.product_id,
    products.brand as Product_name,
    count(items.item_id) as Total_items,
    round(sum(price_in_usd),2) as Revenue,
    round(sum(price_in_usd-cogs_in_usd),2) as Margin
from items
left join products on items.product_id = products.product_id
group by 1
order by 4 desc;
```
| product_id | Product_name       | Total_items | Revenue  | Margin  |
|------------|--------------------|-------------|----------|---------|
| 1          | Ironbark Ale       | 3431        | 51430.69 | 24017   |
| 2          | Emberstone Whiskey | 402         | 11050.98 | 5949.6  |
#### `Which product has been returned the most?`
```sql
select 
    items.product_id, 
    products.brand as Product_Name,
	count(items.item_id) as total_sold,
	count(item_refund.item_refund_id) as total_refunds,
	round(count(item_refund.item_refund_id)/count(items.item_id),4)*100 as refund_rate
from items
left join item_refund on items.item_id = item_refund.item_id
left join products on items.product_id = products.product_id
group by items.product_id;
```
| product_id | Product_Name       | total_sold | total_refunds | refund_rate  |
|------------|--------------------|------------|---------------|--------------|
| 1          | Ironbark Ale       | 3431       | 227           | 6.6200       |
| 2          | Emberstone Whiskey | 402        | 6             | 1.4900       |
#### `What is the session to order conversion rate split by product over time?`
```sql
select
    year(sessions.date) as yr,
    month(sessions.date) as mn,
    count(sessions.session_id) as Number_of_sessions,
    count(case when items.product_id = 1 then items.item_id else null end) as Ale_ordered,
	count(case when items.product_id = 2 then items.item_id else null end) as Whiskey_ordered,
    count(orders.order_id) as Number_of_orders,
    count(orders.order_id)/count(sessions.session_id)*100 as Conversion_rate
from sessions
left join orders on sessions.session_id = orders.session_id_
left join items on items.order_id = orders.order_id
group by 1,2;
```
| yr   | mn | Number_of_sessions | Ale_ordered | Whiskey_ordered | Number_of_orders | Conversion_rate  |
|------|----|--------------------|-------------|-----------------|------------------|------------------|
| 2012 | 3  | 1879               | 60          | 0               | 60               | 3.1932           |
| 2012 | 4  | 3734               | 99          | 0               | 99               | 2.6513           |
| 2012 | 5  | 3736               | 108         | 0               | 108              | 2.8908           |
| 2012 | 6  | 3963               | 140         | 0               | 140              | 3.5327           |
| 2012 | 7  | 4249               | 169         | 0               | 169              | 3.9774           |
| 2012 | 8  | 6097               | 228         | 0               | 228              | 3.7395           |
| 2012 | 9  | 6546               | 287         | 0               | 287              | 4.3844           |
| 2012 | 10 | 8183               | 371         | 0               | 371              | 4.5338           |
| 2012 | 11 | 14011              | 618         | 0               | 618              | 4.4108           |
| 2012 | 12 | 10072              | 506         | 0               | 506              | 5.0238           |
| 2013 | 1  | 6438               | 345         | 83              | 428              | 6.6480           |
| 2013 | 2  | 7240               | 336         | 233             | 569              | 7.8591           |
| 2013 | 3  | 3493               | 164         | 86              | 250              | 7.1572           |
#### `How many refund orders did we initiate along with amount over time?`
```sql
select
    year(date) as yr,
	month(date) as mn,
    count(item_refund.item_refund_id) as total_refunds,
    round(sum(refund_in_usd),2) as Total_refund_amount
from item_refund
group by 1,2;
```
| yr   | mn | total_refunds | Total_refund_amount  |
|------|----|---------------|----------------------|
| 2012 | 4  | 5             | 74.95                |
| 2012 | 5  | 5             | 74.95                |
| 2012 | 6  | 5             | 74.95                |
| 2012 | 7  | 13            | 194.87               |
| 2012 | 8  | 18            | 269.82               |
| 2012 | 9  | 21            | 314.79               |
| 2012 | 10 | 24            | 359.76               |
| 2012 | 11 | 40            | 599.6                |
| 2012 | 12 | 38            | 569.62               |
| 2013 | 1  | 21            | 327.29               |
| 2013 | 2  | 23            | 369.77               |
| 2013 | 3  | 20            | 324.8                |
#### `Categorize our customers based on spending limit and user behaviour`
```sql
select
    count(orders.order_id) as Number_of_orders,
	round(sum(orders.price_in_usd),2) as Revenue,
    case
		when orders.price_in_usd > 35 then 'High Spenders'
        when orders.price_in_usd between 25 and 35 then 'Medium Spenders'
        else 'Low Spenders'
	end as Customer_category,
    case
		when sessions.repeat_user = 1 then 'Repeat Customer'
        else 'New Customer'
	end as Customer_type
from orders
left join sessions on orders.user_id = sessions.user_id
group by 3,4;
```
| Number_of_orders | Revenue  | Customer_category | Customer_type    |
|------------------|----------|-------------------|------------------|
| 3274             | 49077.26 | Low Spenders      | New Customer     |
| 1299             | 19472.01 | Low Spenders      | Repeat Customer  |
| 245              | 6735.05  | Medium Spenders   | New Customer     |
| 157              | 6669.36  | High Spenders     | New Customer     |
| 94               | 2584.06  | Medium Spenders   | Repeat Customer  |
| 56               | 2378.88  | High Spenders     | Repeat Customer  |
#### `How many orders, revenue and average did we receive from customers by type?`
```sql
select 
    case
        when repeat_user = 1 then 'Repeat customer'
        else 'New customer'
    end as customer_type,
    count(distinct o.order_id) as Total_orders,
    round(sum(o.price_in_usd),2) as Total_revenue,
    round(avg(o.price_in_usd), 2) as Avg_order_value
from sessions s
left join orders o
on s.session_id = o.session_id_
group by customer_type
order by total_revenue desc;
```
| customer_type   | Total_orders | Total_revenue | Avg_order_value  |
|-----------------|--------------|---------------|------------------|
| New customer    | 3122         | 52987.47      | 16.97            |
| Repeat customer | 554          | 9494.2        | 17.14            |
#### `How many customers have their orders in the cart and placed an order?`
```sql
select
    count(distinct case when pageviews.url = '/cart' and orders.session_id_ is null then sessions.session_id end) as Orders_in_cart,
    count(distinct orders.order_id) as Orders_placed,
    count(distinct orders.order_id)
		/count(distinct case when pageviews.url = '/cart' and orders.session_id_ is null then sessions.session_id end)*100 as Conv_rate
from pageviews
left join sessions on pageviews.session_id__ = sessions.session_id
left join orders on pageviews.session_id__ = orders.session_id_;
```
| Orders_in_cart | Orders_placed | Conv_rate  |
|----------------|---------------|------------|
| 8646           | 3676          | 42.5168    |
#### `Build a conversion funnel from sessions to orders with conversion rate`
```sql
with pageview_level as (
    select
        sessions.session_id,
        pageviews.url,
        case when url = '/products' then 1 else 0 end as products_page,
        case when url = '/Ironbark Ale' then 1 else 0 end as ironbrak_ale_page,
        case when url = '/cart' then 1 else 0 end as cart_page,
        case when url = '/order_placed' then 1 else 0 end as order_placed_page,
        case when url = '/billing' then 1 else 0 end as billing_page,
        case when url = '/order_confirmed' then 1 else 0 end as order_confirmed_page
    from sessions
    left join pageviews on sessions.session_id = pageviews.session_id__
    where sessions.date < '2012-09-10'   #/billing-1 was launched on 2012-09-10
),
session_level_made_it_flag as (
    select
        session_id,
        max(products_page) as product_made_it,
        max(ironbrak_ale_page) as ironbrak_ale_made_it,
        max(cart_page) as cart_made_it,
        max(order_placed_page) as order_placed_made_it,
        max(billing_page) as billing_made_it,
        max(order_confirmed_page) as order_confirmed_made_it
    from pageview_level
    group by session_id
)

select 
    count(distinct session_id) as sessions,
    count(distinct case when product_made_it = 1 then session_id else null end) as to_products_with_rate,
    count(distinct case when ironbrak_ale_made_it = 1 then session_id else null end) as to_ale_with_rate,
    count(distinct case when cart_made_it = 1 then session_id else null end) as to_cart_with_rate,
    count(distinct case when order_placed_made_it = 1 then session_id else null end) as to_placed_with_rate,
    count(distinct case when billing_made_it = 1 then session_id else null end) as to_billing_with_rate,
    count(distinct case when order_confirmed_made_it = 1 then session_id else null end) as to_confirmed_with_rate
from session_level_made_it_flag
union all
select
    count(distinct case when product_made_it = 2 then '' else null end) as to_products,
    round(count(distinct case when product_made_it = 1 then session_id else null end)
        /count(distinct session_id) * 100, 2) as lander_click_rate,
    round(count(distinct case when ironbrak_ale_made_it = 1 then session_id else null end)
        /count(distinct case when product_made_it = 1 then session_id else null end) * 100, 2) as products_click_rate,
    round(count(distinct case when cart_made_it = 1 then session_id else null end)
        /count(distinct case when ironbrak_ale_made_it = 1 then session_id else null end) * 100, 2) as fuzzy_click_rate,
    round(count(distinct case when order_placed_made_it = 1 then session_id else null end)
        /count(distinct case when cart_made_it = 1 then session_id else null end) * 100, 2) as cart_click_rate,
    round(count(distinct case when billing_made_it = 1 then session_id else null end)
        /count(distinct case when order_placed_made_it = 1 then session_id else null end) * 100, 2) as shipping_click_rate,
    round(count(distinct case when order_confirmed_made_it = 1 then session_id else null end)
        /count(distinct case when billing_made_it = 1 then session_id else null end) * 100, 2) as billing_click_rate
from session_level_made_it_flag;
```
| sessions | to_products_with_rate | to_ale_with_rate | to_cart_with_rate | to_placed_with_rate | to_billing_with_rate | to_confirmed_with_rate  |
|----------|-----------------------|------------------|-------------------|---------------------|----------------------|-------------------------|
| 25324    | 11335.00              | 8137.00          | 3544.00           | 2379.00             | 1954.00              | 870.00                  |
| 0        | 44.76                 | 71.79            | 43.55             | 67.13               | 82.14                | 44.52                   |
#### `Which of our /billing page perform better in terms of conversion rate for initial three months?`
```sql
select	
    pageviews.url,
    count(pageviews.session_id__) as Number_of_sessions,
    count(orders.order_id) as Number_of_orders,
    round(count(orders.order_id)/count(pageviews.session_id__)*100,2) as Conv_rate
from pageviews
left join orders on pageviews.session_id__ = orders.session_id_
where 
	pageviews.date > '2012-09-10' and   #/billing-1 was launched on 2012-09-10
    pageviews.date < '2012-12-10' and
	pageviews.url in ('/billing','/billing-1')
group by 1;
```
| url        | Number_of_sessions | Number_of_orders | Conv_rate  |
|------------|--------------------|------------------|------------|
| /billing-1 | 1266               | 784              | 61.93      |
| /billing   | 1282               | 577              | 45.01      |
#### `What is the running cost for our products over time?`
```sql
select 
    year(date) as yr,
    month(date) as mn,
    round(sum(price_in_usd),2) as Monthly_sales,
    round(sum(sum(price_in_usd)) over (partition by year(date) order by month(date)),2) as running_total
from orders
group by 1,2
order by 1,2;
```
| yr   | mn | Monthly_sales | running_total  |
|------|----|---------------|----------------|
| 2012 | 3  | 899.4         | 899.4          |
| 2012 | 4  | 1484.01       | 2383.41        |
| 2012 | 5  | 1618.92       | 4002.33        |
| 2012 | 6  | 2098.6        | 6100.93        |
| 2012 | 7  | 2533.31       | 8634.24        |
| 2012 | 8  | 3417.72       | 12051.96       |
| 2012 | 9  | 4302.13       | 16354.09       |
| 2012 | 10 | 5561.29       | 21915.38       |
| 2012 | 11 | 9263.82       | 31179.2        |
| 2012 | 12 | 7584.94       | 38764.14       |
| 2013 | 1  | 7438.23       | 7438.23        |
| 2013 | 2  | 11456.8       | 18895.03       |
| 2013 | 3  | 4822.5        | 23717.53       |
#### `Which product has the highest sale over the months?`
```sql
with monthly_sales as (
    select 
        product_id,
        year(date) as yr,
        month(date) as mn,
        round(sum(price_in_usd),2) as Total_sales,
        rank() over (partition by year(date), month(date) order by sum(price_in_usd) desc) as sales_rank
    from items i
    group by 1,2,3
)
select 
    ms.yr,
    ms.mn,
    ms.product_id,
    p.brand,
    ms.total_sales,
    ms.sales_rank
from monthly_sales ms
join products p on ms.product_id = p.product_id
where ms.sales_rank <= 1
order by 1,2,6;
```
| yr   | mn | product_id | brand              | total_sales | sales_rank  |
|------|----|------------|--------------------|-------------|-------------|
| 2012 | 3  | 1          | Ironbark Ale       | 899.4       | 1           |
| 2012 | 4  | 1          | Ironbark Ale       | 1484.01     | 1           |
| 2012 | 5  | 1          | Ironbark Ale       | 1618.92     | 1           |
| 2012 | 6  | 1          | Ironbark Ale       | 2098.6      | 1           |
| 2012 | 7  | 1          | Ironbark Ale       | 2533.31     | 1           |
| 2012 | 8  | 1          | Ironbark Ale       | 3417.72     | 1           |
| 2012 | 9  | 1          | Ironbark Ale       | 4302.13     | 1           |
| 2012 | 10 | 1          | Ironbark Ale       | 5561.29     | 1           |
| 2012 | 11 | 1          | Ironbark Ale       | 9263.82     | 1           |
| 2012 | 12 | 1          | Ironbark Ale       | 7584.94     | 1           |
| 2013 | 1  | 1          | Ironbark Ale       | 5156.56     | 1           |
| 2013 | 2  | 2          | Emberstone Whiskey | 6405.17     | 1           |
| 2013 | 3  | 1          | Ironbark Ale       | 2458.36     | 1           |
#### `When did our products reach the highest sale over months?`
```sql
with product_sales as (
    select 
        product_id,
        year(date) as yr,
        month(date) as mn,
        round(sum(price_in_usd),2) as Total_sales,
        rank() over (partition by product_id order by sum(price_in_usd) desc) as sales_rank
    from items 
    group by 1,2,3
)
select 
    product_sales.product_id,
    products.brand,
    product_sales.yr,
    product_sales.mn,
    product_sales.Total_sales
from product_sales 
join products on product_sales.product_id = products.product_id
where product_sales.sales_rank = 1
order by product_sales.total_sales desc;
```
| product_id | brand              | yr   | mn | Total_sales  |
|------------|--------------------|------|----|--------------|
| 1          | Ironbark Ale       | 2012 | 11 | 9263.82      |
| 2          | Emberstone Whiskey | 2013 | 2  | 6405.17      |
#### `How much is the session to order breakdown by products after launching Emberstone Whiskey?`
```sql
select
	pageviews.url,
    count(distinct pageviews.session_id__) as Number_of_sessions,
    count(distinct orders.order_id) as Number_of_orders,
    round(count(distinct orders.order_id)/count(distinct pageviews.session_id__)*100,2) as conv_rate
from pageviews
left join orders on pageviews.session_id__ = orders.session_id_
where 
	pageviews.date > '2013-02-01' and
	url in ('/Ironbark Ale','/Emberstone Whiskey')
group by 1
order by 4 desc;
```
| url                 | Number_of_sessions | Number_of_orders | conv_rate  |
|---------------------|--------------------|------------------|------------|
| /Emberstone Whiskey | 999                | 198              | 19.82      |
| /Ironbark Ale       | 3119               | 485              | 15.55      |
#### `What is the pre/post session count of one month after launching Emberstone Whiskey?`
```sql
with cte as (
select
	session_id__,
    pageview_id,
    url,
    case 
		when date < '2013-01-06' then 'Before Emberstone Whiskey'
        when date >= '2013-01-06' then 'After Emberstone Whiskey'
        else '.'
	end Launch_cycle
from pageviews
where
	date > '2012-12-06' and
    date < '2013-02-06' and
	url = '/products'
),
cte_1 as (
select	
	cte.Launch_cycle,
	cte.session_id__,
    #cte.pageview_id as products_pageview_id,
    min(pageviews.pageview_id) as Next_pageview_id
from cte
left join pageviews on 
	cte.session_id__ = pageviews.session_id__ and
    pageviews.pageview_id > cte.pageview_id
group by 1,2
),
cte_2 as (
select
	cte_1.Launch_cycle,
	cte_1.session_id__,
    pageviews.url
from cte_1
left join pageviews on cte_1.Next_pageview_id = pageviews.pageview_id
)

select 
	cte_2.launch_cycle,
    count(distinct cte_2.session_id__) as Number_of_products_sessions,
    count(distinct case when cte_2.url = '/Ironbark Ale' then cte_2.session_id__ else null end)
		+count(distinct case when cte_2.url = '/Emberstone Whiskey' then cte_2.session_id__ else null end) as Next_page_Sessions,
	round((count(distinct case when cte_2.url = '/Ironbark Ale' then cte_2.session_id__ else null end)
		+count(distinct case when cte_2.url = '/Emberstone Whiskey' then cte_2.session_id__ else null end))
			/count(distinct cte_2.session_id__)*100,2) as Next_page_rate,
    count(distinct case when cte_2.url = '/Ironbark Ale' then cte_2.session_id__ else null end) as Ale_sessions,
	count(distinct case when cte_2.url = '/Emberstone Whiskey' then cte_2.session_id__ else null end) as Whiskey_sessions
from cte_2
group by 1
order by 1 desc;
```
| Launch_cycle              | Number_of_products_sessions | Next_page_Sessions | Next_page_rate | Ale_sessions | Whiskey_sessions  |
|---------------------------|-----------------------------|--------------------|----------------|--------------|-------------------|
| Before Emberstone Whiskey | 4452                        | 3115               | 69.97          | 3115         | 0                 |
| After Emberstone Whiskey  | 3265                        | 2476               | 75.83          | 2159         | 317               |
#### `Build a conversion funnel from /products to /order_confirmed by brands`
```sql
with cte as (
select
	pageviews.session_id__,
    pageviews.pageview_id,
    pageviews.url as products
from pageviews
where 
	pageviews.url in ('/Ironbark Ale','/Emberstone Whiskey') and
    pageviews.date > '2013-01-06'
),
cte_1 as (
select
	cte.session_id__,
    cte.pageview_id,
    cte.products,
    pageviews.url,
    case when pageviews.url = '/cart' then 1 else 0 end as cart,
    case when pageviews.url = '/order_placed' then 1 else 0 end as order_placed,
    case when pageviews.url = '/billing-1' then 1 else 0 end as billing,
    case when pageviews.url = '/order_confirmed' then 1 else 0 end as order_confirmed
from cte
left join pageviews on 
	cte.session_id__ = pageviews.session_id__ and
    pageviews.pageview_id > cte.pageview_id
),
cte_2 as (
select  
	cte_1.session_id__,
    products,
    max(cart) as cart,
    max(order_placed) as order_placed,
    max(billing) as billing,
    max(order_confirmed) as order_confirmed
from cte_1
group by 1,2
)

select
	cte_2.products,
    count(distinct session_id__) as Total_sessions,
    count(distinct case when cart = 1 then session_id__ else 0 end) as cart_visited,
    round(count(distinct case when cart = 1 then session_id__ else 0 end)
		/count(distinct session_id__)*100,2) as cart_rate,
	count(distinct case when order_placed = 1 then session_id__ else 0 end) as placed_visited,
    round(count(distinct case when order_placed = 1 then session_id__ else 0 end)	
		/count(distinct case when cart = 1 then session_id__ else 0 end)*100,2) as placed_rate,
    count(distinct case when billing = 1 then session_id__ else 0 end) as billing_visited,
    round(count(distinct case when billing = 1 then session_id__ else 0 end)
		/count(distinct case when order_placed = 1 then session_id__ else 0 end)*100,2) as billing_rate,
    count(distinct case when order_confirmed = 1 then session_id__ else 0 end) as confirmed_visited,
    round(count(distinct case when order_confirmed = 1 then session_id__ else 0 end)
		/count(distinct case when billing = 1 then session_id__ else 0 end)*100,2) as confirmed_rate
from cte_2
group by 1
order by 1 desc;
```
| products            | Total_sessions | cart_visited | cart_rate | placed_visited | placed_rate | billing_visited | billing_rate | confirmed_visited | confirmed_rate  |
|---------------------|----------------|--------------|-----------|----------------|-------------|-----------------|--------------|-------------------|-----------------|
| /Ironbark Ale       | 5029           | 2160         | 42.95     | 1492           | 69.07       | 1218            | 81.64        | 770               | 63.22           |
| /Emberstone Whiskey | 1289           | 699          | 54.23     | 493            | 70.53       | 392             | 79.51        | 248               | 63.27           |
#### `What is the pre/post session to order conversion rate for two months after launching Emberstone Whiskey along with sales statistics?`
```sql
select
	case
		when sessions.date < '2013-01-06' then 'Before Emberstone Whiskey'
        when sessions.date >= '2013-01-06' then 'After Emberstone Whiskey'
        else '.'
	end as Launch_cycle,
    count(distinct sessions.session_id) as sessions,
    count(distinct orders.order_id) as orders,
    round(count(distinct orders.order_id)/count(distinct sessions.session_id)*100,2) as conv_rate,
    round(sum(price_in_usd),2) as revenue,
    sum(orders.products_purchased) as total_products_sold,
    round(sum(price_in_usd)/count(distinct orders.order_id),2) as average_order_value,
    round(sum(price_in_usd)/count(distinct sessions.session_id),2) as revenue_per_session
from sessions
	left join orders
		on orders.session_id_= sessions.session_id
where 
	sessions.date between '2012-11-06' and '2013-03-06'
group by 1
order by 1 desc;
```
| Launch_cycle              | sessions | orders | conv_rate | revenue  | total_products_sold | average_order_value | revenue_per_session  |
|---------------------------|----------|--------|-----------|----------|---------------------|---------------------|----------------------|
| Before Emberstone Whiskey | 23825    | 1135   | 4.76      | 17013.65 | 1135                | 14.99               | 0.71                 |
| After Emberstone Whiskey  | 13715    | 899    | 6.55      | 19729.74 | 1026                | 21.95               | 1.44                 |
#### `What is the order count for cross selling products?`
```sql
select
	orders.first_product_id_in_cart,
    count(distinct orders.order_id) as orders,
    count(distinct case when items.product_id = 1 then orders.order_id else null end) as Items_sold_with_product_1,
    count(distinct case when items.product_id = 2 then orders.order_id else null end) as Items_sold_with_product_2
from orders
left join 
	items on items.order_id = orders.order_id and 
	items.is_product_first_in_cart = 0
group by 1;
```
| first_product_id_in_cart | orders | Items_sold_with_product_1 | Items_sold_with_product_2  |
|--------------------------|--------|---------------------------|----------------------------|
| 1                        | 3385   | 0                         | 111                        |
| 2                        | 291    | 46                        | 0                          |
#### `Categorize our customers based on the number of days taken to return their products`
```sql
select 
    case 
        when datediff(item_refund.date, items.date) > 0 
             and datediff(item_refund.date, items.date) <= 7 then 'one week'
        when datediff(item_refund.date, items.date) > 7 
             and datediff(item_refund.date, items.date) <= 14 then 'two weeks'
        else 'more than two weeks'
    end as Time_category, 
    count(item_refund.item_refund_id) as Number_of_refunds,
    round(sum(item_refund.refund_in_usd), 2) as Refund_amount,
    round(sum(item_refund.refund_in_usd) / count(item_refund.item_refund_id), 2) as Refund_per_order,
    round(avg(datediff(item_refund.date, items.date)), 2) as Avg_days_to_refund
from item_refund
left join items on items.item_id = item_refund.item_id
group by 1;
```
| Time_category       | Number_of_refunds | Refund_amount | Refund_per_order | Avg_days_to_refund  |
|---------------------|-------------------|---------------|------------------|---------------------|
| one week            | 94                | 1434.06       | 15.26            | 4.53                |
| two weeks           | 114               | 1746.36       | 15.32            | 11.29               |
| more than two weeks | 25                | 374.75        | 14.99            | 15.48               |



























































