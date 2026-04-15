# zomato_sql_project
Zomato Data Analysis (SQL Project)

📌 Project Overview

This project explores restaurant data from Zomato to uncover insights about:

Customer preferences

Cost vs rating trends

Online vs offline ordering behavior

Voting patterns and popularity

The analysis is performed using SQL, focusing on data cleaning, transformation, and exploratory data analysis (EDA).

🗂️ Dataset Description

The dataset contains the following columns:

1. namee	: Restaurant name

2. online_order : Online ordering availability

3. book_table :	Table booking availability

4. rate :	Rating of the restaurant

5. votes :	Number of votes

6. approx_cost :	Approximate cost for two people

7. listed_in : Category/type of restaurant

1. Table Creation
```sql
drop table if exists zomato;

create table zomato(
    namee varchar(500),	
    online_order varchar(40),
    book_table varchar(40),
    rate numeric,
    votes int,
    approx_cost int,
    listed_in varchar(100)
);
```

2.Data Cleaning & Preparation

~ Key cleaning steps performed:

Standardized online_order values:

'Yes' → 'online'

'No' → 'offline'

~Renamed column:

 online_order → medium_of_order

~Checked for null values across all columns

```sql
-- Handle inconsistent values
update zomato
set online_order = 'offline'
where online_order = 'No';

update zomato
set online_order = 'online'
where online_order = 'Yes';

-- Rename column
alter table zomato
rename column online_order to medium_of_order;

-- Check nulls
select *
from zomato
where namee is null
   or medium_of_order is null
   or book_table is null
   or rate is null
   or votes is null
   or approx_cost is null
   or listed_in is null;
```
3. Basic Exploration
```sql
select count(*) as total_rows from zomato;

select distinct listed_in from zomato;

select distinct namee from zomato;
```
4. Restaurant Frequency
```sql
-- Restaurants appearing multiple times
select namee, count(*) as frequency
from zomato
group by namee
having count(*) > 1
order by frequency desc;
```
5. Booking Analysis
```sql
-- Total booking availability
select book_table, count(*) as total
from zomato
group by book_table;

-- Booking under online vs offline
select medium_of_order, count(*) as bookings
from zomato
where book_table = 'Yes'
group by medium_of_order;

-- Booking by category
select listed_in, count(*) as bookings
from zomato
where book_table = 'Yes'
group by listed_in
order by bookings desc;
```
6. Cost Analysis
```sql
-- Highest cost per category
select listed_in, max(approx_cost) as highest_cost
from zomato
group by listed_in;

-- Most expensive restaurant
select namee, approx_cost, listed_in, rate, votes
from zomato
order by approx_cost desc
limit 1;
```
7. Cost vs Rating
```sql
select 
case 
    when approx_cost < 300 then 'low'
    when approx_cost between 300 and 600 then 'medium'
    else 'high'
end as cost_category,
round(avg(rate),2) as avg_rating
from zomato
group by cost_category;
```
Insight: Higher cost ≠ higher rating

8. Highest Rated per Category
```sql
select *
from (
    select listed_in, namee, rate,
    row_number() over(partition by listed_in order by rate desc) as rn
    from zomato
) as zomato_table
where rn = 1;
```
9. Votes Analysis
```sql
-- Most voted restaurants
select namee, votes
from zomato
order by votes desc;

-- Votes vs rating
select 
case 
    when votes < 1000 then 'low'
    when votes between 1000 and 2500 then 'medium'
    else 'high'
end as vote_category,
round(avg(rate),2) as avg_rating
from zomato
group by vote_category
order by avg_rating desc;
```
10. Online vs Offline Analysis
```sql
-- Average rating comparison
select
listed_in,
round(avg(case when medium_of_order='offline' then rate end),2) as offline_rating,
round(avg(case when medium_of_order='online' then rate end),2) as online_rating
from zomato
group by listed_in;

-- Count of orders
select medium_of_order, count(*) as total
from zomato
group by medium_of_order;

-- Total votes by medium
select medium_of_order, sum(votes) as total_votes
from zomato
group by medium_of_order;
```
11. Cost Distribution
```sql
select 
listed_in,
min(approx_cost) as min_cost,
avg(approx_cost) as avg_cost,
max(approx_cost) as max_cost
from zomato
group by listed_in;
```
📊 Insights & Business Recommendations
Key Insights

1. Cost does NOT guarantee better ratings
   
High-cost restaurants do not consistently receive higher ratings.

Mid-range restaurants often perform equally well or better.

Insight: Customers value quality and experience over pricing.

2. Votes influence perceived quality

Restaurants with medium to high votes tend to have slightly better average ratings.
Low-vote restaurants show inconsistent ratings.

Insight: Higher votes build trust and credibility.

3. Online vs Offline Ratings Are Similar

Average ratings between online and offline orders are nearly identical.

Insight: Service quality is consistent across ordering modes.

4. Booking Availability Varies by Category

Some restaurant categories heavily support table booking, while others do not.

Insight: Booking features depend on restaurant type and dining experience.

Business Recommendations

1. Focus on Value for Money

Restaurants should balance pricing and quality to attract more customers.

Overpricing without quality improvements may hurt ratings.

2. Increase Customer Reviews

Encourage users to leave ratings and reviews.

Final Conclusion

This analysis shows that:

.Customer perception is driven more by experience, value, and popularity than just price.

.Restaurants can significantly improve performance by focusing on customer engagement, pricing strategy, and visibility.

More votes = higher credibility and visibility.
