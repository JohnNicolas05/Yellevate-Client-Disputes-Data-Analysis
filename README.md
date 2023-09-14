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

-	**Average Days to Settle Invoices**
  
![Alt text](https://github.com/JohnNicolas05/Yellevate-Client-Disputes-Data-Analysis/blob/main/Yellevate-Disputes-Data-Analysis/Assets/2b1.PNG)

![Alt text](https://github.com/JohnNicolas05/Yellevate-Client-Disputes-Data-Analysis/blob/main/Yellevate-Disputes-Data-Analysis/Assets/2b2.png)

- **Average Days to Settle Disputes**

![Alt text](https://github.com/JohnNicolas05/Yellevate-Client-Disputes-Data-Analysis/blob/main/Yellevate-Disputes-Data-Analysis/Assets/avg-of-daystosettle.PNG)

![Alt text](https://github.com/JohnNicolas05/Yellevate-Client-Disputes-Data-Analysis/blob/main/Yellevate-Disputes-Data-Analysis/Assets/avg-of-daystosettle%20chart.PNG)

- **Percentage Dispute Won/Lost**

![Alt text](https://github.com/JohnNicolas05/Yellevate-Client-Disputes-Data-Analysis/blob/main/Yellevate-Disputes-Data-Analysis/Assets/percentage-dispute-won-lost.PNG)

![Alt text](https://github.com/JohnNicolas05/Yellevate-Client-Disputes-Data-Analysis/blob/main/Yellevate-Disputes-Data-Analysis/Assets/percentage-dispute-won-lost-chart.PNG)

- **Percentage Revenue Won/Lost**

![Alt text](https://github.com/JohnNicolas05/Yellevate-Client-Disputes-Data-Analysis/blob/main/Yellevate-Disputes-Data-Analysis/Assets/percentage-revenue-won-lost.png)

![Alt text](https://github.com/JohnNicolas05/Yellevate-Client-Disputes-Data-Analysis/blob/main/Yellevate-Disputes-Data-Analysis/Assets/percentage-revenue-won-lost-chart.png)

- **Revenue Loss Per Country**

![Alt text](https://github.com/JohnNicolas05/Yellevate-Client-Disputes-Data-Analysis/blob/main/Yellevate-Disputes-Data-Analysis/Assets/revenue-lost-per-country.png)

![Alt text](https://github.com/JohnNicolas05/Yellevate-Client-Disputes-Data-Analysis/blob/main/Yellevate-Disputes-Data-Analysis/Assets/revenue-lost-per-country-chart.png)

**Findings**
1.	The average processing time in which invoices are settled (average # of days rounded to a whole number) is **26 days.**
2.	The average processing time for the company to settle disputes (average # of days rounded to a whole number) is **37 days** assuming that these are all the invoices that were disputed that were won not lost.
3.	The percentage of disputes received by the company that were lost is **17.69%**.
4.	The percentage of revenue lost from disputes is **4.67%**.
5.	The country that has the most Revenue Losses is **France**.
6.	The company discloses that all services hired to do were fully completed.
7.	The quality of services provided is not the main driving reason for these invoice disputes
8.	The management believes most disputes are the result of contract technicalities or clients thinking they can get away with not paying for the services.

**Recommendations**

**1.) Identify the problems facing the business and come up with objectives that can solve them with data analysis.**

**Problems:**
1.	High number of disputes due to contract technicalities
2.	Long dispute settlement times
3.	High percentage of lost disputes

**Objectives:**
1.	Identify the most common contract technicalities leading to disputes and propose changes to the contract language to reduce the number of disputes.
2.	Analyze the current dispute settlement process and identify bottlenecks to reduce the settlement time.
3.	Investigate the reasons for lost disputes and propose solutions to increase the dispute win rate.

**2.) Data analysis goals according to business objectives.**
**Objective 1:** Identify the most common contract technicalities leading to disputes and propose changes to the contract language to reduce the number of disputes.

Data analysis goals:
-	Analyze the data to identify the most common contract technicalities leading to disputes.
-	Identify the patterns and trends in the data that could explain the reasons for the disputes.
-	Use data visualization techniques to communicate the findings to the management.
-	Propose changes to the contract language that could reduce the number of disputes.

**Objective 2:** Analyze the current dispute settlement process and identify bottlenecks (the cause of delays or issues in the dispute settlement time) to reduce the settlement time.

Data analysis goals:
-	Collect and analyze data on the current dispute settlement process.
-	Identify the bottlenecks and inefficiencies in the settlement process.
-	Propose changes to the process that could reduce the settlement time.
-	Use data visualization techniques to communicate the findings to the management.

**Objective 3:** Investigate the reasons for lost disputes and propose solutions to increase the dispute win rate.

Data analysis goals:
-	Analyze the data to identify the reasons for lost disputes.
-	Identify the patterns and trends in the data that could explain the reasons for the lost disputes.
-	Use data visualization techniques to communicate the findings to the management.
-	Propose solutions to increase the dispute win rate based on the data analysis.

**3.) Generate insights from your analysis to provide recommendations on probable strategies to deal with disputes effectively.**

1.	**Review and improve contract drafting:** As most disputes are the result of contract technicalities, the company should consider reviewing and improvising the contract drafting process. To achieve this, the company should collaborate with legal professionals to identify and detect any uncertainties, inconsistencies or loopholes in the contract terms that can lead to disputes. Clarity and transparency of the contract terms will help in reducing the number of disputes and increasing the client satisfaction.

2.	**Streamline dispute resolution process:** By streamlining the dispute resolution process this will make the process more efficient yet effective with faster and simpler methods. Long dispute settlement times can be a major source of frustration for clients and can lead to a conflict in communication between the company and to its clients. In order to streamline the process, the company must be able to identify the cause of the delays and addressing them. This could involve improving communication networks, providing up-to-date updates to clients on the status of their dispute. and implementing efficient and transparent dispute resolution procedures.

3.	**Improve client communication:** Considering quality of service may not be the main reason for disputes, effective communication with clients can help in reducing disputes. Improving on how the company communicate with its clients should be put in to consideration. This can be achieved by providing clear and timely information on the services offered, contract terms, payment procedures, and other relevant information. This could involve investing in client relationship management and communication skills training.

4.	**Implement data analytics tools:** With proper data analytics tools, it can be used in identifying patterns in client disputes. This could also help in improving the dispute resolution process and prevent disputes to happen. Investing in data analytics tools can provide insights with the causes of disputes, kinds of disputes that are common, and the effectiveness of the company's dispute resolution procedures. This could involve hiring data analysts or collaboration with a data analytics firm.

**4.) Sales Analysis and Areas of Improvement (Additional Analysis)**
1.	**Total sales:** We can calculate the total sales by summing up the ‘InvoiceAmount’ column excluding ‘DisputeLost’.
   
![Alt text](https://github.com/JohnNicolas05/Yellevate-Client-Disputes-Data-Analysis/blob/main/Yellevate-Disputes-Data-Analysis/Assets/total-sales-and-loss.png)

![Alt text](https://github.com/JohnNicolas05/Yellevate-Client-Disputes-Data-Analysis/blob/main/Yellevate-Disputes-Data-Analysis/Assets/total-sales-and-loss-chart.png)

2.	**Sales by country:** We can group the data by country and calculate the total sales for each country.

![Alt text](https://github.com/JohnNicolas05/Yellevate-Client-Disputes-Data-Analysis/blob/main/Yellevate-Disputes-Data-Analysis/Assets/total-sales-per-country.PNG)

![Alt text](https://github.com/JohnNicolas05/Yellevate-Client-Disputes-Data-Analysis/blob/main/Yellevate-Disputes-Data-Analysis/Assets/total-sales-per-country-chart.png)

3.	**Sales by customer:** We can group the data by ‘customerID’ and calculate the total sales for each customer.

![Alt text](https://github.com/JohnNicolas05/Yellevate-Client-Disputes-Data-Analysis/blob/main/Yellevate-Disputes-Data-Analysis/Assets/total-sales-by-customer.PNG)

![Alt text](https://github.com/JohnNicolas05/Yellevate-Client-Disputes-Data-Analysis/blob/main/Yellevate-Disputes-Data-Analysis/Assets/total-sales-by-customer-chart.png)

4.	**Sales trends over time:** We can plot the sales over time to identify any trends or seasonal patterns.

![Alt text](https://github.com/JohnNicolas05/Yellevate-Client-Disputes-Data-Analysis/blob/main/Yellevate-Disputes-Data-Analysis/Assets/sales-trend-over-time.PNG)

![Alt text](https://github.com/JohnNicolas05/Yellevate-Client-Disputes-Data-Analysis/blob/main/Yellevate-Disputes-Data-Analysis/Assets/sales-trend-over-time-chart.png)

5.	**Dispute rate:** We can calculate the dispute rate by dividing the number of ‘disputed’ by the total number of invoices.

![Alt text](https://github.com/JohnNicolas05/Yellevate-Client-Disputes-Data-Analysis/blob/main/Yellevate-Disputes-Data-Analysis/Assets/dispute-rate.PNG)

![Alt text](https://github.com/JohnNicolas05/Yellevate-Client-Disputes-Data-Analysis/blob/main/Yellevate-Disputes-Data-Analysis/Assets/dispute-rate-chart.png)

**Based from the information provided, we can identify the areas for improvement. These are some potential recommendations:**

1.	**High dispute rate:** We need to identify the dispute and its causes and develops strategic methods to address them. Possible causes of disputes could include unclear contract terms and poor communication with clients. To address these issues, we may need to review and modify contracts and improve how we communicate with our clients.

2.	**Low sales in certain countries and customers:** If sales are low in certain countries and customers, we may need to investigate the reasons on having this issue and to implement effective marketing strategies to generate more sales. This may involve conducting market research to identify the demand and needs or offering customized services for specific customers

3.	**Seasonal patterns in sales:** If there are seasonal patterns in sales, we may need to adjust staffing levels or inventory to meet demand during peak seasons. This could involve hiring temporary workers or increasing inventory levels during high-demand periods.

4.	**Slow dispute resolution:** If disputes take too long to get resolved, we may need to implement faster, easier, yet effective dispute resolution process. This could involve hiring additional staff or investing in technology to automate the dispute resolution process.
