## Lita-Capstone-Project-2--Customer-Segmentation-for-a-Subscription-Service

### Project Overview
This repository demonstrates a Power BI solution that integrates data from both an Excel Pivot Table and an SQL Database to visualize and analyze customer subscription trends, segmentation, and cancellations. The dashboard uses slicers for interactive analysis, and the data sources are cleaned and transformed for deeper insights. This project involves analyzing customer data for a subscription service to identify segments and trends. The goal is to understand customer behavior, track subscription types, and identify key trends in cancellations and renewals. The final deliverable is a Power BI dashboard that presents the analysis.

### Dataset Structure
The dataset used in this dashboard contains the following columns:

- CustomerID: Unique identifier for the customer.
- CustomerName: The name of the customer.
- Region: The geographic region the customer belongs to.
- SubscriptionType: Type of subscription the customer has (e.g., Basic, Premium, Standard).
- SubscriptionStart: The start date of the customer’s subscription.
- SubscriptionEnd: The end date of the customer’s subscription.
- Canceled: Boolean flag indicating whether the subscription was canceled (True/False).
- Revenue: The revenue generated from the subscription.

### Technologies

- *SQL*: For querying and analyzing the data.
- *Pivot Table (Excel)*: To summarize data and perform basic analysis.
- *Power BI*: To create an interactive dashboard with slicers and visualizations.

### Dataset File
- Excel File: 
([CustomersDataCSV.csv](https://github.com/user-attachments/files/17703675/CustomersDataCSV.csv))

### Key Objectives:
- Clean and preprocess sales data for analysis
- Analyze sales trends and patterns using SQL Server queries
- Visualize critical sales performance metrics using Power BI dashboards
- Identify top products, regions with highest sales, and top customers

### Methodology:
- Data Cleaning: Excel data manipulation and Pivot Tables
- Data Analysis: SQL Server queries for data modeling and insights
- Data Visualization: Power BI dashboards for interactive and dynamic visualization

### Exploratory Data Analysis (EDA)
 - Total revenue by subscription type
 - Total number of active and canceled subscriptions
 - Total number of customers from each region
 - Average subscription duration for all customers
 - Customers with subscriptions longer than 12 months
 - Customers who canceled their subscription within 6 months

### Excel:
 - Analyze customer data using pivot tables to find subscription patterns
 - Calculate the average subscription duration and identify the most popular
subscription types.

![image](https://github.com/user-attachments/assets/7d8f9452-aa8f-42b9-a458-90c8849ad3fd)

![image](https://github.com/user-attachments/assets/990562fa-1f14-4a31-abb3-40da1f31f1f3)

![image](https://github.com/user-attachments/assets/d18e753b-a55c-4039-aa24-94ada82b545b)

![image](https://github.com/user-attachments/assets/6e104845-027c-4aa3-9b83-5a8fc98f2f9c)

![image](https://github.com/user-attachments/assets/ef870d08-adb5-497a-906d-c56b51d777eb)

![image](https://github.com/user-attachments/assets/5ee4770c-42fc-4fa3-8fe0-88ae4f94672e)

### SQL:
I loaded the dataset into my SQL Server environment to write and validate the queries to extract key insights based on the following questions.
- Retrieve the total number of customers from each region.
- Find the most popular subscription type by the number of customers.
- Find customers who canceled their subscription within 6 months.
- Calculate the average subscription duration for all customers.
- Find customers with subscriptions longer than 12 months.
- Calculate total revenue by subscription type.
- Find the top 3 regions by subscription cancellations.
- Find the total number of active and canceled subscriptions.

``` Select * from [dbo].[CustomersDataCSV]

------- (Question 1) Retrieve the total number of customers from each region-----------
Select Region, Count(CustomerID)
as Total_No_of_Customers
From CustomersDataCSV 
group by Region

------- (Question 2) find the most popular subscription type by the number of customers-------------
Select SubscriptionType,
Count(CustomerID) as Number_of_Customers
from CustomersDataCSV group by SubscriptionType

-------(Question 3) find customers who canceled their subscription within 6 months-----
Select CustomerName,Canceled,SubscriptionStart 
from CustomersDataCSV 
where Canceled =0
and Month(SubscriptionStart)
between 1 and 6

-------(Question 4) calculate the average subscription duration for all customers----------
Select Count(CustomerID)
AS All_customers, AVG(DateDiff(Day,SubscriptionStart, SubscriptionEnd)) 
as Average_Subscription_Duration 
from CustomersDataCSV
where SubscriptionEnd is not Null

---------(Question 5) Find customers with subscriptions longer than 12 months.

Select CustomerName, SubscriptionType, SubscriptionStart, SubscriptionEnd
from CustomersDataCSV
where DateDiff(Month, SubscriptionStart, SubscriptionEnd)>=12

---------(Question 6) Calculate total revenue by subscription type
Select SubscriptionType, sum(Revenue)
as Total_Revenue
from CustomersDataCSV
group by SubscriptionType

---------(Question 7) Find the top 3 regions by subscription cancellations
Select Top 3 Region, Canceled from CustomersDataCSV

----------(Question 8) Find the total number of active and canceled subscriptions
Select Canceled,
sum(case when Canceled = 1 then 1 else 0 end) 
as Activesubscriptions,
sum(case when canceled = 0 then 1 else 0 end)
as Canceledsubscriptions
from CustomersDataCSV
group by Canceled
```

### Power BI:
To create a Power BI dashboard that visualizes key customer segments, cancellations, and subscription trends, I followed the following steps to design an interactive and insightful report.
####Step 1: *Load the Data*
1. *Import Data*: Import the dataset into Power BI by selecting "Get Data" and choosing the file format (CSV) 
2. *Verify Data Quality*: Ensured the data types are correctly assigned ( CustomerID as text, SubscriptionStart and SubscriptionEnd as dates, Revenue as a numeric field, Canceled as Boolean)
### Step 2: *Create Calculated Columns/Measures*
Before building the visuals, I created some useful calculated columns or measures to enhance your analysis:
- *Subscription Duration* (calculated column):
  DAX
  SubscriptionDuration = DATEDIFF([SubscriptionStart], [SubscriptionEnd], DAY)
  

- *Active Subscription* (calculated column to flag active subscriptions):
  DAX
  ActiveSubscription = IF([Canceled] = TRUE(), "Canceled", "Active")
  

- *Total Revenue* (measure for total revenue):
  DAX
  TotalRevenue = SUM([Revenue])
  

- *Revenue per Customer* (measure to calculate average revenue per customer):
  DAX
  RevenuePerCustomer = AVERAGE([Revenue])
  

- *Canceled Customers* (measure to count canceled customers):
  DAX
  CanceledCustomers = COUNTROWS(FILTER(YourTable, [Canceled] = TRUE()))
  
- *Active Customers* (measure to count active customers):
  DAX
  ActiveCustomers = COUNTROWS(FILTER(YourTable, [Canceled] = FALSE()))
  

- *Customer Growth* (measure for new customers by subscription start year):
  DAX
  CustomerGrowth = DISTINCTCOUNT([CustomerID])
  
### Step 3: *Create Visuals for the Dashboard*

#### 1. *Customer Segments Visualization* (Customer Demographics and Subscription Type)
- *Bar Chart* or *Stacked Column Chart*: 
  - *Axis*: Region or SubscriptionType
  - *Values*: Count of CustomerID (this gives you the number of customers per region or subscription type)
  - *Legend*: Use ActiveSubscription to show how many customers are active vs. canceled.

#### 2. *Subscription Status Breakdown*
- *Pie Chart* or *Donut Chart*:
  - *Values*: Count of CustomerID
  - *Legend*: Use ActiveSubscription (e.g., Active vs Canceled)

#### 3. *Revenue Over Time*
- *Line Chart*: 
  - *Axis*: SubscriptionStart or use a Date Hierarchy (Year, Quarter, Month)
  - *Values*: TotalRevenue
  - This will show the trend of revenue over time.

#### 4. *Customer Growth by Period*
- *Line Chart* or *Area Chart*:
  - *Axis*: Year (or Month, if granular data is available)
  - *Values*: CustomerGrowth
  - This will show the growth or decline in the number of customers over time.

#### 5. *Revenue by Region/Subscription Type*
- *Stacked Column Chart*:
  - *Axis*: Region or SubscriptionType
  - *Values*: TotalRevenue
  - *Legend*: ActiveSubscription
  - This can help to see which regions or subscription types are generating more revenue and how that correlates with cancellations.

#### 6. *Cancellation Analysis*
- *Bar Chart*:
  - *Axis*: SubscriptionStart (use Date Hierarchy for Year/Month)
  - *Values*: CanceledCustomers
  - This will help track cancellations over time and identify any trends or spikes.

#### 7. *Revenue per Customer by Subscription Type*
- *Clustered Bar Chart*:
  - *Axis*: SubscriptionType
  - *Values*: RevenuePerCustomer
  - This will give insight into which subscription types generate the most revenue per customer.

#### 8. *Top 10 Customers by Revenue*
- *Table or Matrix Visualization*:
  - *Columns*: CustomerID, CustomerName, TotalRevenue
  - This shows the top 10 customers contributing the most revenue. Use sorting to display top revenue-generating customers.

### Step 4: *Interactive Slicers*
Add slicers to make the dashboard interactive and allow users to filter the data:

1. *Slicer for Region*: Add a slicer to filter by region.
   - *Field*: Region

2. *Slicer for Subscription Type*: Add a slicer for users to filter by subscription type.
   - *Field*: SubscriptionType

3. *Slicer for Active vs Canceled*: Add a slicer to toggle between active and canceled customers.
   - *Field*: ActiveSubscription

4. *Slicer for Date*: Add a slicer for the subscription start date or year.
   - *Field*: SubscriptionStart

5. *Slicer for Customer Growth Period*: Add a slicer for period selection (e.g., year, month, quarter).
   - *Field*: SubscriptionStart or use a Date Hierarchy.





### Key Insights the Dashboard Could Reveal:
1. *Customer Segmentation*: Identifies which regions or subscription types have the highest customer count and revenue.
2. *Trends in Cancellations*: Tracks the impact of cancellations over time, helping businesses focus on retention efforts.
3. *Revenue Trends*: Shows how revenue has evolved, revealing seasonality or long-term trends.
4. *Growth and Acquisition*: Visualizes customer growth over time, helping businesses understand their acquisition rate.
5. *Top Customers*: Pinpoints top customers by revenue, which can help in targeted sales or retention strategies.

This dashboard design provides a comprehensive view of the customer and subscription landscape, while the interactivity of slicers gives users the flexibility to analyze different segments or time periods.
