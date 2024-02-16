# Atliq-hardware-store-sales-report
AtliQ Hardware is a company that supplies computer hardware and peripherals to many clients such as Excel stores, and Surge stories across India. AtliQ Hardware's head office is situated in Delhi, India and they have many regional offices throughout India.

Problem statement:
Now Problem was that all these things happening were verbal and there was no proof with facts of how his business was affected which made him frustrated as he could see that overall sales were declining but when he asked the regional manager, he did not get a complete picture of his business. When he asks to give details, they give a lot of Excel files and this AtliQ hardware is big business so to see insight clearly, he needs a dashboard to look at real data. And he will get proper insight and can make data-driven decisions to increase sales of his company.

Data Exploration Using MySQL:
![image](https://github.com/palmahendra/Atliq-hardware-store-sales-report/assets/65278325/fd563b0a-fa41-4f27-877d-5ea2b49dc008)

Let's explore the Data using the query-

# All Customer records
SELECT * FROM sales. customers;
#All Transaction 
SELECT * FROM sales. transactions; /*we can see that the sales amount is negative */ 
![image](https://github.com/palmahendra/Atliq-hardware-store-sales-report/assets/65278325/25223da8-f8fc-4672-bd02-80dd0aa2421a)
Observation- Sales Qty is negative which is invalid and also currency is in USD, so we need to convert it into INR as finding insight will be problematic (total sum of revenue).


# Markets in regions
SELECT * FROM sales. markets;
![image](https://github.com/palmahendra/Atliq-hardware-store-sales-report/assets/65278325/817e3805-4501-450e-91f0-b21c2ad68c9a)
Observation- The company is doing Sales in India and also might have done in New York and Paris 1–2 times, so here we will remove it because right now company is doing business only in India so these data are not useful.

. Data Cleaning and ETL
Now we will pull data into Power BI and also do Data Cleaning known as ETL(Extract Transform and Load).

We need to transform data as it is messy and need to convert it into different formats so that we can perform Data Analysis in Power BI.

Get data →More Options →Databases →MySQL database
![image](https://github.com/palmahendra/Atliq-hardware-store-sales-report/assets/65278325/31ec94b3-a213-4e2c-9f5f-1856e974aa4f)

→Now, Transform Data which will launch into Power Query Editor where we will do Data Cleaning.

In sales. market table, we need to remove New York and Paris, and for that, we will remove the blank from the zone column and that will filter out .
![image](https://github.com/palmahendra/Atliq-hardware-store-sales-report/assets/65278325/eab1f817-066f-485e-9c3e-5fb37aff44b6)

In the transaction table , we will remove 0 and -1 from sales_amount as we can see that in below picture that-

5 sales qty done but sales_amount = 0 which is illogic and there are only 2 product which sales qty is -1
![image](https://github.com/palmahendra/Atliq-hardware-store-sales-report/assets/65278325/b73a90b1-69be-4dab-8959-2c27d95d44eb)

![image](https://github.com/palmahendra/Atliq-hardware-store-sales-report/assets/65278325/09d6c123-be38-4873-84ed-6fb381de6b3a)

Convert USD into INR-

Add new column →Conditional column →normalized currency where sales amount will be in INR

![image](https://github.com/palmahendra/Atliq-hardware-store-sales-report/assets/65278325/9c981f3a-8434-4164-897b-040412b0e93f)
Also in sales. date table, convert date format into mmm-yy for better visualization

![image](https://github.com/palmahendra/Atliq-hardware-store-sales-report/assets/65278325/c987ae6f-0546-4c6b-a578-2c08feb17ea3)

If you explore the data, you will find that there are duplicates of USD and INR

SELECT count(*) from sales. transactions where sales. transactions.currency="INR\r"; 
#150000 - cant removed as it is a large amount

SELECT count(*) from sales. transactions where sales. transactions.currency="INR"; 
#279 -we can remove it as it is a small record and can be considered bad data

SELECT count(*) from sales. transactions where sales. transactions.currency="USD\r"; 
# 2 

SELECT count(*) from sales. transactions where sales. transactions.currency="USD"; 
# 2 

#LET SEE USD AND USD/r RECORDS 

SELECT * from sales.transactions where sales.transactions.currency='USD\r' or sales.transactions.currency='USD';

![image](https://github.com/palmahendra/Atliq-hardware-store-sales-report/assets/65278325/8c61c052-b344-4950-8373-47843e5928ac)


So we can see that it is a duplicate and for analysis, it's better to delete anyone of them so let's delete USD and keep USD/r .

Finally, we will keep data with INR/r and USD/r

![image](https://github.com/palmahendra/Atliq-hardware-store-sales-report/assets/65278325/225d9d29-d0ae-4421-9249-0b88cff767ec)

Build Dashboard
Create Measure table →where we will use DAX Formula

Profit Margin % = DIVIDE([Total Profit Margin],[Revenue],0)

Profit Margin Contribution % = DIVIDE([Total Profit Margin],CALCULATE([Total Profit Margin],ALL('sales products'),ALL('sales customers'),ALL('sales markets')))

Revenue = SUM('Sales transactions'[norm_sales_amount])

Revenue Contribution % = DIVIDE([Revenue],CALCULATE([Revenue],ALL('sales products'),ALL('sales customers'),ALL('sales markets')))

Sales Qty = SUM('sales transactions'[sales_qty])

Total Profit Margin = SUM('Sales transactions'[Profit_Margin])

Conclusion-
If you check the dashboard, you will come to know that Sales are declining and now the sales director will come to know in which region sales are high and sales are low so that they can work on the product and enhance their business in this dynamic growth market.

