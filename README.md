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

🧱 1. Table Creation
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

🧹 Data Cleaning & Preparation

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
