# E-Commerce-Webstore-Performance-Dashboard

you can explore the project [here](https://app.powerbi.com/view?r=eyJrIjoiNDVlMzIyOTMtNjhkNy00Nzc1LThlMzYtNDQwYmE4Yzk5Y2IzIiwidCI6IjU2NDM4N2U5LWNmMTEtNDI3Yy1hNmY3LTcxZGU2MWFlZTQxNyJ9)

<img width="1249" height="804" alt="Screenshot (114)" src="https://github.com/user-attachments/assets/ffc99778-a016-4729-876d-956b0a508174" />


## Project Overview

This project is designed to answer a fundamental business question:

How effectively is the webstore driving revenue growth, customer engagement, and repeat purchasing over time?

The dashboard provides a consolidated view of webstore performance by combining sales, customer activity, and behavioral metrics into a single decision-support tool. It enables stakeholders to evaluate whether the digital channel is not only generating revenue, but also building sustained customer engagement and long term value.

The dashboard helps answer key operational and strategic questions, including:

- Is online revenue growing consistently over time?

- How efficiently are customers converting from engagement to purchase?

- Are customers returning to place repeat orders?

- How quickly do new customers place their first order after account creation?

- What share of overall revenue is driven by the online channel?

By presenting these metrics in an interactive format with time, country, and customer type filters, the dashboard supports data driven decision making around customer experience, engagement strategies, and revenue optimization.

---

## Insights & Observations

1️⃣ Strong Digital Adoption

With 51% of total revenue coming from online channels and over 96% of customers active online, the webstore plays a critical role in the business’s revenue engine.

2️⃣ High Conversion Efficiency

A conversion rate of 96.88% indicates that logged in customers are highly intent driven, suggesting:

Effective UX

Strong account-based purchasing behavior

Well optimized checkout flows

3️⃣ Loyal Customer Base

A repeat online order rate of 76.71% highlights strong customer retention and satisfaction, which is essential because lifetime value matters more than one time purchases.

4️⃣ Fast Customer Activation

New customers place their first online order within 13 days on average, suggesting efficient onboarding, account setup, and early value realization.

5️⃣ Consistent Revenue Growth

Weekly revenue trends show steady growth with periods of acceleration, allowing stakeholders to:

Spot seasonality

Identify high performing weeks

Compare online vs offline performance

---

## Insights & Recommendations

| Insight                                                         | Evidence                                         | Recommendation                                                             |
| --------------------------------------------------------------- | ------------------------------------------------ | -------------------------------------------------------------------------- |
| Online revenue shows sustained growth over time                 | Weekly revenue trends increase across the period | Continue monitoring growth patterns and prepare for demand peaks           |
| Online channel contributes a significant share of total revenue | Online Share of Revenue exceeds 50%              | Prioritize optimization of the online channel as a key revenue driver      |
| Customer engagement is very high                                | Customers Active Online above 90%                | Use high engagement to introduce personalized offers and upsell strategies |
| Strong repeat purchasing behavior                               | Repeat Online Order Rate above 70%               | Strengthen retention through loyalty incentives and repeat-order campaigns |
| Conversion rate is extremely high                               | Conversion Rate close to 97%                     | Focus efforts on increasing order value rather than acquisition            |
| Customers convert relatively quickly after onboarding           | Avg Time to First Online Order ~13 days          | Improve onboarding journeys to reduce time-to-first-purchase further       |
| Revenue fluctuates week to week                                 | Noticeable weekly revenue volatility             | Analyze campaign timing and operational factors impacting demand           |

---

## Data Model Overview

<img width="1058" height="755" alt="Screenshot (115)" src="https://github.com/user-attachments/assets/c117fad9-778e-409a-b4d4-a612fa1a116f" />


- FactLogings : (LogingID, CustomerID, LogingDate)

- FactOrders: (Order ID, Order Date, Revenue, Channel, Customer ID)

- DimCustomer: (Customer ID, Account Creation Date, Country, Customer Type)

- Date Dimension: (week, month, year)

Star schema modeling was used to ensure:

 - High performance

 - Accurate filtering

 - Scalable analytics

---

## Key Metrics

| KPI                                | Definition                                                             | Calculation                                        |
| ---------------------------------- | ---------------------------------------------------------------------- | -------------------------------------------------- |
| **Online Revenue**                 | Total revenue generated from online orders during the selected period  | CALCULATE( SUM(FactOrders[Revenue]), FactOrders[Channel] = "Online" )       |
| **Online Orders**                  | Total number of online orders placed                                   | CALCULATE( DISTINCTCOUNT(FactOrders[OrderID]), FactOrders[Channel] = "Online" )         |
| **Average Online Order Value**     | Average amount spent per online order                                  | DIVIDE( [Online Revenue], [Online Orders] )                     |
| **Conversion Rate**                | Percentage of engaged customers who placed at least one online order   | DIVIDE( [Online Ordering Customers], [Logged In Customers] )    |
| **Online Share of Revenue**        | Contribution of online sales to total revenue                          | DIVIDE( [Online Revenue], SUM(FactOrders[Revenue]) )                     |
| **Customers Active Online**        | Percentage of customers who logged into the webstore during the period | DIVIDE( [Logged In Customers], DISTINCTCOUNT(DimCustomer[CustomerID]) )              |
| **Repeat Online Order Rate**       | Percentage of online customers who placed more than one order          | DIVIDE( [Repeat Online Customers], [Online Customers] )         |
| **Avg Time to First Online Order** | Average number of days from account creation to first online purchase  | AVERAGEX( VALUES(DimCustomer[CustomerID]), DATEDIFF( MIN(DimCustomer[AccountCreationDate]), CALCULATE( MIN(FactOrders[OrderDate]), FactOrders[Channel] = "Online" ), DAY ) ) |
