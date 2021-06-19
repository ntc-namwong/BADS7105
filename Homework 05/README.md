# CLV Dashboard

This study aims to create CLV dashboard for supermarket dataset. Customer Lifetime Value (CLV) can be calculated from 4 parameters as equation below:

![](http://latex.codecogs.com/svg.latex?CLV%3DT%5Ctimes%20AOV%5Ctimes%20AGM%5Ctimes%20ALT)

- Average number of transaction per month (T)
- Average order value (AOV)
- Average gross margin (AGM)
- Average customer lifespan in month (ALT)

## Assumptions

- Initially, gross margin is assumed as 10% because there is no cost of good sold in the dataset. However, it can be updated by using filter under CLV number in the dashboard.
- Due to customer lifespan depends on churn rate in each month, so [Homewok 10](https://github.com/ntc-namwong/BADS7105/tree/main/Homework%2010) is integrated in this study. The actual number of churned customers is used to calculate ALT.
- Actually, suppermarket cannot treat customer personally. In case that CLV has decreased, it makes sense that supermarket will provide some promotion for a GROUP of customers (not provide promotion person by person). So, [Homework 06](https://github.com/ntc-namwong/BADS7105/tree/main/Homework%2006) is also integrated in this study to provide customer segmentation data.

## Dashboard

This dashboard is designed to answer 3 questions:

- What is the problem now?
- What is the reason of that problem?
- What is the action to solve that problem?

Pictures below are examples of proposed CLV dashboard. It is conspicuous that total sales and spending decrease from last year.

- **Problem:**
  - Customer Lifetime Value (CLV) of this year (2008) decreases compared with last year (2007).
- **Reason:**
  - Totol sales decreases related to decreases in spending per visit and spending per customer.
  - Even DEP00076 do great job to increase the sales, but other departments are still below the target line.
  - Premium customers trend to decrease buying product from the supermarket.
- **Action:**
  - To bring premium customers back to the supermarket, it is neccessary to reinvestigate what is promotion provided to the premium customers for product class CL00045, CL00058, CL00067, CL00150 and CL00229 and study fessibility if repromotion in this year again. This is becuase product class CL00045, CL00058, CL00067, CL00150 and CL00229 are top5 which the spending decreases from last year.
  - *Note that promotion analysis cannot be done because promotion data is not present in the dataset.*

***Picture 5-1 CLV Dashboard***
![Picture 5-1](https://github.com/ntc-namwong/BADS7105/blob/main/Homework%2005/Picture%205-1%20CLV%20Dashboard.jpg)

***Picture 5-2 CLV Dashboard Filtered by Active User***
![Picture 5-2](https://github.com/ntc-namwong/BADS7105/blob/main/Homework%2005/Picture%205-2%20CLV%20Dashboard%20Filtered%20Active%20User.jpg)

***Picture 5-3 CLV Dashboard Filtered by Premium User***
![Picture 5-3](https://github.com/ntc-namwong/BADS7105/blob/main/Homework%2005/Picture%205-3%20CLV%20Dashboard%20Filtered%20Premium%20User.jpg)
