# CLV Dashboard

This study aims to create CLV dashboard for supermarket dataset. Customer Lifetime Value (CLV) can be calculated from 4 parameters as equation below:

![formula](http://latex.codecogs.com/svg.latex?CLV%3DT%5Ctimes%20AOV%5Ctimes%20AGM%5Ctimes%20ALT)

- Average number of transaction per month (T)
- Average order value (AOV)
- Average gross margin (AGM)
- Average customer lifespan in month (ALT)

## Assumptions

- Initially, gross margin is assumed as 10% because there is no cost of good sold in the dataset. However, it can be updated by using filter under CLV number in the dashboard.
- Due to customer lifespan depends on churn rate in each month, so Homewok 10 is integrated in this study. The actual number of churned customers is used to calculate ALT.
- Actually, suppermarket cannot treat customer personally. In case that CLV has decreased, it makes sense that supermarket will provide some promotion for a GROUP of customers (not provide promotion person by person). So, Homework 06 is also integrated in this study to provide customer segmentation data.

## Dashboard

*Picture 5-1 CLV Dashboard*
![](https://github.com/ntc-namwong/BADS7105/blob/main/Homework%2005/Picture%205-1%20CLV%20Dashboard.jpg)

*Picture 5-2 CLV Dashboard Filtered by Active User*
![](https://github.com/ntc-namwong/BADS7105/blob/main/Homework%2005/Picture%205-2%20CLV%20Dashboard%20Filtered%20Active%20User.jpg)

*Picture 5-3 CLV Dashboard Filtered by Premium User*
![](https://github.com/ntc-namwong/BADS7105/blob/main/Homework%2005/Picture%205-3%20CLV%20Dashboard%20Filtered%20Premium%20User.jpg)
