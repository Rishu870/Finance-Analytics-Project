# Finance-Analytics-Project
A comprehensive finance analytics project leveraging SQL for data extraction, R for statistical modeling.

Fabric + Dataflow Gen2 + Warehouse + SQL Project
Finance Analysis Project

select *  from finance_transactions
select * FROM customers


-- KPI's


-- Total Transactions
SELECT COUNT(*) AS total_transactions
FROM finance_transactions;


--Total Customers
SELECT COUNT(*) AS total_customers
FROM customers;


--Average Customer Age
SELECT AVG(age) AS avg_customer_age
FROM customers;


-- Total Transaction Amount
SELECT ROUND(SUM(amount), 2) AS total_transaction_amount
FROM finance_transactions;


-- Total Fees Collected
SELECT ROUND(SUM(fee_amount), 2) AS total_fees
FROM finance_transactions;


-- Total Tax Amount
SELECT ROUND(SUM(tax_amount),2) AS total_tax
FROM finance_transactions;


-- Average Transaction Amount
SELECT ROUND(AVG(amount),2) AS avg_transaction_amount
FROM finance_transactions;


-- Average Annual Income
SELECT ROUND(AVG(annual_income),2) AS avg_annual_income
FROM customers;


-- Successful Transactions
SELECT COUNT(*) AS successful_transactions
FROM finance_transactions
WHERE transaction_status = 'Success';


-- Fraud Amount
SELECT SUM(amount) AS fraud_amount
FROM finance_transactions
WHERE is_fraud = 'Yes';


-- Success Rate %
SELECT 
    CAST(
        COUNT(CASE WHEN transaction_status = 'Success' THEN 1 END) * 100.0 
        / COUNT(*) AS DECIMAL(10,2)
    ) AS success_rate_percentage
FROM finance_transactions;


-- Fraud Rate %
SELECT 
    CAST(
        COUNT(CASE WHEN is_fraud = 'Yes' THEN 1 END) * 100.0 
        / COUNT(*) AS DECIMAL(10,2)
    ) AS fraud_rate_percentage
FROM finance_transactions;




-- Granular Insights


-- Total Transaction Amount by Month
SELECT 
    YEAR(transaction_date) AS year_,
    MONTH(transaction_date) AS month_no,
    DATENAME(MONTH, transaction_date) AS month_,
    ROUND(SUM(amount),2) AS total_amount
FROM finance_transactions
GROUP BY 
    YEAR(transaction_date),
    MONTH(transaction_date),
    DATENAME(MONTH, transaction_date)
ORDER BY year_, month_no;


-- Transaction Status Distribution
SELECT 
    transaction_status,
    COUNT(*) AS total_transactions
FROM finance_transactions
GROUP BY transaction_status;


-- Transaction Type Analysis
SELECT 
    transaction_type,
    COUNT(*) AS total_transactions,
    ROUND(SUM(amount),2) AS total_amount
FROM finance_transactions
GROUP BY transaction_type
ORDER BY total_amount DESC;


-- Customers by State
SELECT 
    state,
    COUNT(*) AS total_customers
FROM customers
GROUP BY state
ORDER BY total_customers DESC;


-- Customers by Occupation
SELECT 
    occupation,
    COUNT(*) AS total_customers
FROM customers
GROUP BY occupation
ORDER BY total_customers DESC;


-- Customer Segment Distribution
SELECT 
    customer_segment,
    COUNT(*) AS total_customers
FROM customers
GROUP BY customer_segment;


-- Age Group Analysis
SELECT 
    CASE 
        WHEN age < 25 THEN 'Below 25'
        WHEN age BETWEEN 25 AND 40 THEN '25-40'
        WHEN age BETWEEN 41 AND 60 THEN '41-60'
        ELSE '60+'
    END AS age_group,
    COUNT(*) AS total_customers
FROM customers
GROUP BY 
    CASE 
        WHEN age < 25 THEN 'Below 25'
        WHEN age BETWEEN 25 AND 40 THEN '25-40'
        WHEN age BETWEEN 41 AND 60 THEN '41-60'
        ELSE '60+'
    END
ORDER BY total_customers DESC;



-- Top 10 Highest Transactions
SELECT TOP 10
    transaction_id,
    customer_id,
    amount,
    transaction_date
FROM finance_transactions
ORDER BY amount DESC;


-- Top 10 Customers by Transaction Amount
SELECT TOP 10
    c.customer_name,
    ROUND(SUM(f.amount),2) AS total_transaction_amount
FROM finance_transactions f
INNER JOIN customers c
    ON f.customer_id = c.customer_id
GROUP BY c.customer_name
ORDER BY total_transaction_amount DESC;


-- Monthly Revenue by Customer Segment
SELECT 
    YEAR(f.transaction_date) AS year_,
    MONTH(f.transaction_date) AS month_,
    c.customer_segment,
    ROUND(SUM(f.fee_amount + f.tax_amount),2) AS revenue
FROM finance_transactions f
INNER JOIN customers c
    ON f.customer_id = c.customer_id
GROUP BY 
    YEAR(f.transaction_date),
    MONTH(f.transaction_date),
    c.customer_segment
ORDER BY year_, month_;



-- Total Transaction Amount by Gender
SELECT 
    c.gender,
    ROUND(SUM(f.amount),2) AS total_transaction_amount
FROM finance_transactions f
INNER JOIN customers c
    ON f.customer_id = c.customer_id
GROUP BY c.gender
ORDER BY total_transaction_amount DESC;


-- Revenue by Customer Segment
SELECT 
    c.customer_segment,
    ROUND(SUM(f.fee_amount + f.tax_amount),2) AS revenue
FROM finance_transactions f
INNER JOIN customers c
    ON f.customer_id = c.customer_id
GROUP BY c.customer_segment
ORDER BY revenue DESC;



-- Top States by Transaction Amount
SELECT 
    c.state,
    ROUND(SUM(f.amount),2) AS total_transaction_amount
FROM finance_transactions f
INNER JOIN customers c
    ON f.customer_id = c.customer_id
GROUP BY c.state
ORDER BY total_transaction_amount DESC;


-- Top 10 Cities by Revenue
SELECT TOP 10
    c.city,
    ROUND(SUM(f.fee_amount + f.tax_amount),2) AS revenue
FROM finance_transactions f
INNER JOIN customers c
    ON f.customer_id = c.customer_id
GROUP BY c.city
ORDER BY revenue DESC;


-- Age Group Wise Revenue
SELECT 


    CASE 
        WHEN c.age < 25 THEN 'Below 25'
        WHEN c.age BETWEEN 25 AND 40 THEN '25-40'
        WHEN c.age BETWEEN 41 AND 60 THEN '41-60'
        ELSE '60+'
    END AS age_group,


    ROUND(SUM(f.fee_amount + f.tax_amount),2) AS revenue


FROM finance_transactions f
INNER JOIN customers c
    ON f.customer_id = c.customer_id


GROUP BY 
    CASE 
        WHEN c.age < 25 THEN 'Below 25'
        WHEN c.age BETWEEN 25 AND 40 THEN '25-40'
        WHEN c.age BETWEEN 41 AND 60 THEN '41-60'
        ELSE '60+'
    END


ORDER BY revenue DESC;



