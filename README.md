# Preppin Data 2023 Week 1
This is a challenge from [Summer of SQL](https://github.com/wjsutton/the_summer_of_sql) to help gain skills and confidence in SQL.

Skills tested in challenge 3.1:
- String manipulation with SPLIT_PART


## Challenge: Preppin Data 2023 Week 1

Input

![Input](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgsUOKkk8GcZYYwhHsBtfsNqZc6v2zc0zSnbhAbgfrUF98cdN7ai6ShPZmtJZuRZzio8v2Sovo6QtrzA45eM0Jne3o33sXcE6gmD9qI_j37ABO6eOD7T3cIXQtszMD31hWSNq7AH43Jm5VQdpTOTnfBangz2d_69itGY6ya1qXEPx7d7IWgdleiM_WZSw/w640-h228/Screenshot%202023-01-02%20at%2019.58.57.png).


The Data Source Bank (DSB). This week we have had a report with a number of transactions that have not just our transactions but other banks' too. Can you help clean up the data?
Requirements:
- Split the Transaction Code to extract the letters at the start of the transaction code. These identify the bank who processes the transaction
- Rename the new field with the Bank code 'Bank'.
- Rename the values in the Online or In-person field, Online of the 1 values and In-Person for the 2 values.
- Change the date to be the day of the week

Different levels of detail are required in the outputs. You will need to sum up the values of the transactions in three ways:
1. Total Values of Transactions by each bank
2. Total Values by Bank, Day of the Week and Type of Transaction (Online or In-Person)
3. Total Values by Bank and Customer Code


## 1. Total Values of Transactions by each bank
````sql
SELECT 
SPLIT_PART(transaction_code,'-',1) as bank,
SUM(value) as total_value
FROM pd2023_wk01
GROUP BY SPLIT_PART(transaction_code,'-',1);
````

| BANK | TOTAL_VALUE |
|------|-------------|
| DS   | 653940      |
| DSB  | 530489      |
| DTB  | 618238      |


## 2. Total Values by Bank, Day of the Week and Type of Transaction (Online or In-Person)
````sql
SELECT
SPLIT_PART(transaction_code,'-',1) as bank,
CASE 
WHEN online_or_in_person=1 THEN 'Online'
WHEN online_or_in_person=2 THEN 'In-Person'
END as online_in_person,
DAYNAME(DATE(transaction_date,'dd/MM/yyyy hh24:mi:ss')) as day_of_week,
SUM(value) as total_value
FROM pd2023_wk01
GROUP BY 1,2,3;
````
Sample

| BANK | ONLINE_IN_PERSON | DAY_OF_WEEK | TOTAL_VALUE |
|------|------------------|-------------|--------------|
| DTB  | Online           | Mon         | 38688        |
| DTB  | In-Person        | Fri         | 41861        |
| DTB  | In-Person        | Thu         | 57605        |
| DSB  | Online           | Mon         | 31692        |
| DSB  | Online           | Fri         | 45647        |
| DS   | In-Person        | Wed         | 63686        |
| DTB  | Online           | Tue         | 68959        |
| DTB  | Online           | Sat         | 21392        |
| DS   | In-Person        | Sun         | 51301        |
| DSB  | Online           | Sat         | 61350        |
| DSB  | In-Person        | Wed         | 36069        |
| DSB  | In-Person        | Sat         | 72679        |
| DTB  | Online           | Wed         | 35313        |
| DSB  | In-Person        | Tue         | 28604        |
| DTB  | Online           | Fri         | 27366        |

## 3. Total Values by Bank and Customer Code
````sql
SELECT
SPLIT_PART(transaction_code,'-',1) as bank,
customer_code,
SUM(value) as total_value| BANK | ONLINE_IN_PERSON | DAY_OF_WEEK | TOTAL_VALUE |
|------|------------------|-------------|--------------|
| DTB  | Online           | Mon         | 38688        |
| DTB  | In-Person        | Fri         | 41861        |
| DTB  | In-Person        | Thu         | 57605        |
| DSB  | Online           | Mon         | 31692        |
| DSB  | Online           | Fri         | 45647        |
| DS   | In-Person        | Wed         | 63686        |
| DTB  | Online           | Tue         | 68959        |
| DTB  | Online           | Sat         | 21392        |
| DS   | In-Person        | Sun         | 51301        |
| DSB  | Online           | Sat         | 61350        |
| DSB  | In-Person        | Wed         | 36069        |
| DSB  | In-Person        | Sat         | 72679        |
| DTB  | Online           | Wed         | 35313        |
| DSB  | In-Person        | Tue         | 28604        |
| DTB  | Online           | Fri         | 27366        |
FROM pd2023_wk01
GROUP BY SPLIT_PART(transaction_code,'-',1),
customer_code;
````
Sample
| BANK | CUSTOMER_CODE | TOTAL_VALUE |
|------|----------------|-------------|
| DS   | 100000         | 57909       |
| DSB  | 100001         | 67856       |
| DS   | 100008         | 56400       |
| DSB  | 100010         | 92654       |
| DS   | 100007         | 76190       |
| DSB  | 100000         | 27585       |
| DTB  | 100003         | 84574       |
| DS   | 100006         | 77636       |
| DTB  | 100009         | 52926       |
| DSB  | 100007         | 29702       |
| DTB  | 100000         | 77252       |
| DSB  | 100002         | 27936       |
| DTB  | 100010         | 71396       |
| DTB  | 100006         | 41909       |
