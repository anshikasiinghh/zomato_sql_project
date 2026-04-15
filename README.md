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

🧹 Data Cleaning & Preparation

~ Key cleaning steps performed:

Standardized online_order values:

'Yes' → 'online'

'No' → 'offline'

~Renamed column:

online_order → medium_of_order

~ Checked for null values across all columns
