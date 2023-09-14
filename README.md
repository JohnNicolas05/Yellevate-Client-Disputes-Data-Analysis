### The Analytics Team:

**John Nicolas** - *the team lead* 

**Pat Aquino** - *the data analyst* 

**Charmaine Tago** - *the data analyst* 

# Yellevate: Client Disputes Data Analysis
**Problem**
Yellevate, a company which specializes in providing marketing services to other companies, has an annual revenue loss of approximately 5% due to loss disputes from clients who are unsatisfied with their services. All the services the company was hired to do were fully completed and the quality of the services provided is not the main driving reason for these invoice disputes. The management believes most disputes are the result of contract technicalities or clients thinking they can get away with not paying for the services. Determining the reason for these disputes and how the company can minimize them will in turn reduce the company’s annual revenue loss.

**Methodology**

&nbsp; **1. Data Importation and Cleaning in Postgre SQL**

**1.A. Create a table in SQL using the query below. Import the file yellevate_invoices.csv to the created table.**

SQL Syntax:
```sh
CREATE TABLE IF NOT EXISTS yellevate_invoices(
country varchar,
customerID varchar,
InvoiceNumber numeric,
InvoiceDate date,
DueDate date,
InvoiceAmount numeric,
disputed numeric,
DisputeLost numeric,
SettledDate date,
DaysToSettle integer,
DaysLate integer
);
```
**1.B. Import the file yellevate_invoices.csv to the created table.**

**1.C. Finding the average processing time in which invoices are settled (in days):**

SQL syntax:
```sh
SELECT ROUND(AVG(DaysToSettle)) AS avg_days_settled 
FROM yellevate_invoices;
```
![Alt text](https://github.com/JohnNicolas05/Yellevate-Client-Disputes-Data-Analysis/blob/main/Yellevate-Disputes-Data-Analysis/Assets/1c.png)

**1.D. Finding the processing time for the company to settle disputes (in days):**

SQL syntax:
```sh
SELECT ROUND(AVG(daystosettle)) as avg_processing_time
FROM yellevate_invoices
WHERE disputed = 1 AND disputelost = 0;
```
![Alt text](https://github.com/JohnNicolas05/Yellevate-Client-Disputes-Data-Analysis/blob/main/Yellevate-Disputes-Data-Analysis/Assets/1d.png)

**1.E. Finding the percentage of disputes received by the company that were lost:**

SQL syntax:
```sh
SELECT ROUND((SUM(disputelost)/COUNT(disputed))*100, 2) AS percent_dispute_lost
FROM yellevate_invoices
WHERE disputed = 1;
```
![Alt text](https://github.com/JohnNicolas05/Yellevate-Client-Disputes-Data-Analysis/blob/main/Yellevate-Disputes-Data-Analysis/Assets/1e.png)

**1.F. Finding the percentage of revenue lost from disputes:**

SQL syntax:
```sh
SELECT ROUND((SUM(CASE WHEN disputelost = 1 THEN invoiceamount ELSE 0 END)/SUM(invoiceamount))*100, 2) as percent_revenue_lost
FROM yellevate_invoices;
```
![Alt text](https://github.com/JohnNicolas05/Yellevate-Client-Disputes-Data-Analysis/blob/main/Yellevate-Disputes-Data-Analysis/Assets/1f.png)

**1.G. Finding the country where the company reached the highest losses from lost disputes (in USD):**

SQL syntax:
```sh
SELECT country, SUM(CASE WHEN disputelost = 1 THEN invoiceamount else 0 END) as total_loss
FROM yellevate_invoices
WHERE disputed = 1
GROUP BY country
ORDER BY total_loss DESC;
```
![Alt text](https://github.com/JohnNicolas05/Yellevate-Client-Disputes-Data-Analysis/blob/main/Yellevate-Disputes-Data-Analysis/Assets/1g.png)

&nbsp; **2. Data Analysis and Visualization Using Excel**

**2.A. Copy the cleaned data from PostgreSQL to excel.**

**2.B. Convert Data from PostgreSQL to Pivot table and insert charts**

-	Average Days to Settle Invoices
  
![Alt text](https://github.com/JohnNicolas05/Yellevate-Client-Disputes-Data-Analysis/blob/main/Yellevate-Disputes-Data-Analysis/Assets/2b1.PNG)

![Alt text](https://github.com/JohnNicolas05/Yellevate-Client-Disputes-Data-Analysis/blob/main/Yellevate-Disputes-Data-Analysis/Assets/2b2.png)



