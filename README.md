# Customer Churn Analysis

## Snapshot of Dashboard (Power BI Service)

![Dashboad](https://github.com/Shubham301099/Customer-Churn-Analysis-/assets/154081009/49f5095e-2960-49d2-8d2d-ed34e72bb361)

## Snapshot of Age_group Dashboard (Power BI Service)

![AgeGroup wise data ](https://github.com/Shubham301099/Customer-Churn-Analysis-/assets/154081009/4df9e50b-265c-41d9-b0bb-7a903e12516b)

## Snapshot of Service_wise Dashboard (Power BI Service)

![service wise data](https://github.com/Shubham301099/Customer-Churn-Analysis-/assets/154081009/bf2443ba-f8fa-411d-855c-e1f4fbc80b69)

## Snapshot of Client_wise Dashboard (Power BI Service)

![Client wise details](https://github.com/Shubham301099/Customer-Churn-Analysis-/assets/154081009/a9528242-28f5-4886-841d-563e7978cd1d)

## Problem Statement

The telecommunications company is facing a high rate of customer churn, leading to significant revenue loss. The management is keen on identifying the key factors contributing to customer churn and developing strategies to reduce it. This project utilizes data analytics techniques to analyze a dataset containing customer demographics, service usage, contract details, and churn status. The goal is to uncover insights and answer specific questions that will help the company understand and mitigate customer churn.

## Project Overview

The customer churn analysis project aims to:

1. Identify primary factors influencing customer churn.
2. Differentiate churned customers from active ones based on demographic and behavioral characteristics.
3. Analyze services and contract terms contributing significantly to customer churn.
4. Develop data-driven strategies to improve customer retention.

## Tech Stack Used

- MySQL Workbench 8.0 CE for SQL environment and PowerBI dataset visualization.

## Project Approach

1. **Data Collection and Understanding:**
   - Explore the dataset to understand its structure, variables, and relationships.
   - Identify key features related to demographics, services, and churn status.

2. **Data Analysis:**
   - Conduct data analysis to uncover patterns, correlations, and distributions.
   - Explore relationships between variables and identify potential churn indicators.

3. **Model Building and Evaluation:**
   - Develop predictive models and create a PowerBI dashboard for forecasting customer churn.
   - Evaluate models using appropriate metrics and perform hyperparameter tuning.

4. **Churn Prediction and Insights:**
   - Apply the trained model to predict churn for unseen data.
   - Derive actionable insights on factors influencing churn rates and their impact on customer retention.

   
### Questions to find the solution of churn analysis.

- Identify the total number of customers and the churn rate.

``` sql
SELECT
   	COUNT(*) AS TotalCustomers,
   	SUM(CASE WHEN Churn_Status = 'Yes' THEN 1 ELSE 0 END) AS ChurnedCustomers,
   	(SUM(CASE WHEN Churn_Status = 'Yes' THEN 1 ELSE 0 END) / COUNT(*)) AS ChurnRate
FROM Customers;

```
Output - 

![Screenshot 2024-01-28 000915](https://github.com/Shubham301099/Customer-Churn-Analysis-/assets/154081009/24bd1fcb-e3d3-4e07-999d-05144fe71724)


- 	Find the average age of churned customers.

``` SELECT
AVG(Age) AS AverageAgeOfChurnedCustomers
FROM Customers
WHERE Churn_Status = 'Yes';
```
Output - 

![image](https://github.com/Shubham301099/Customer-Churn-Analysis-/assets/154081009/021f4bba-04db-40f2-a37c-d61d27c46ed0)


- Discover the most common contract types among churned customers.

``` 
SELECT
Contract_Type,
COUNT(*) AS Count
FROM Customers
WHERE Churn_Status = 'Yes'
GROUP BY Contract_Type
ORDER BY Count DESC;
```
Output - 

![image](https://github.com/Shubham301099/Customer-Churn-Analysis-/assets/154081009/8aabc984-fb2d-4612-99a6-6dec2532fb4f)


- 	Analyze the distribution of monthly charges among churned customers.

``` 
SELECT
Churn_Status,
AVG(Monthly_Charges) AS AverageMonthlyCharges,
MIN(Monthly_Charges) AS MinimumMonthlyCharge,
MAX(Monthly_Charges) AS MaximumMonthlyCharge
FROM Customers
GROUP BY Churn_Status;
```
Output - 

![image](https://github.com/Shubham301099/Customer-Churn-Analysis-/assets/154081009/b9a93fc2-ae29-48f4-9745-066e4489b9ae)


- Create a query to identify the contract types that are most prone to churn.
```
SELECT
Contract_Type,
COUNT(*) AS ChurnedCustomers
FROM Customers
WHERE Churn_Status = 'Yes'
GROUP BY Contract_Type
ORDER BY ChurnedCustomers DESC;
```
Output - 

![image](https://github.com/Shubham301099/Customer-Churn-Analysis-/assets/154081009/7e8bcf63-30e6-4623-a256-cf10e78aaf8c)

- Identify customers with high total charges who have churned.
```
SELECT
Customer_ID,
Total_Charges
FROM Customers
WHERE Churn_Status = 'Yes' AND Total_Charges > (SELECT AVG(Total_Charges) FROM Customers);
```
Output - 

![image](https://github.com/Shubham301099/Customer-Churn-Analysis-/assets/154081009/b0365efb-e407-4fd9-83c2-8d031c142fbd)

- 	Calculate the total charges distribution for churned and non-churned customers.
```
-  SELECT
    Churn_Status,
    AVG(Total_Charges) AS AverageTotalCharges,
    MIN(Total_Charges) AS MinimumTotalCharges,
    MAX(Total_Charges) AS MaximumTotalCharges
    FROM Customers
    GROUP BY Churn_Status;
```
Output - 

![image](https://github.com/Shubham301099/Customer-Churn-Analysis-/assets/154081009/8f0f03ae-c4f0-444d-b19f-13869c81f972)

- 	Calculate the average monthly charges for different contract types among churned customers.
```
SELECT
    Contract_Type,
    AVG(Monthly_Charges) AS AverageMonthlyCharges
    FROM Customers
    WHERE Churn_Status = 'Yes'
    GROUP BY Contract_Type;
```
Output - 

![image](https://github.com/Shubham301099/Customer-Churn-Analysis-/assets/154081009/f7733e20-2261-49e6-aa57-b2055ed4084d)

- Identify customers who have both online security and online backup services and have not churned.

```
SELECT
    Customer_ID,
    Gender,
    Age,
    Monthly_Charges
    FROM Customers
    WHERE Churn_Status = 'No' AND Online_Security = 'Yes' AND Online_Backup = 'Yes';
``` 

Output - 

![image](https://github.com/Shubham301099/Customer-Churn-Analysis-/assets/154081009/3931399d-e198-4586-999c-26fc3d30a31c)

- Determine the most common combinations of services among churned customers. 

``` 
SELECT
    CONCAT_WS(', ', Online_Security, Online_Backup, Device_Protection,             Tech_Support) AS ServiceCombination,
    COUNT(*) AS ChurnedCustomers
    FROM Customers
    WHERE Churn_Status = 'Yes'
    GROUP BY ServiceCombination
    ORDER BY ChurnedCustomers DESC. 
```
Output - 

![image](https://github.com/Shubham301099/Customer-Churn-Analysis-/assets/154081009/65680754-f83f-4c13-af46-2a0a87b71fbd)

- Identify the average total charges for customers grouped by gender and marital status.

```
SELECT
    Gender,
    Marital_Status,
    AVG(Total_Charges) AS AverageTotalCharges
    FROM Customers
    GROUP BY Gender, Marital_Status;

```
Output - 

![image](https://github.com/Shubham301099/Customer-Churn-Analysis-/assets/154081009/607c8880-e299-456b-b79c-b5500f77f410)

- 	Calculate the average monthly charges for different age groups among churned customers. 
```

SELECT
CASE
WHEN Age < 19 THEN 'Teen age'
WHEN Age >= 20 AND Age < 39 THEN 'adult age'
WHEN Age >= 40 AND Age < 59 THEN 'Middle age adult'
ELSE '60 and over'
END AS AgeGroup,
AVG(Monthly_Charges) AS AverageMonthlyCharges
FROM Customers
WHERE Churn_Status = 'Yes'
GROUP BY AgeGroup;
```
Output - 

![image](https://github.com/Shubham301099/Customer-Churn-Analysis-/assets/154081009/43b4ec9f-3816-479d-967a-2371a0daa6d5)

-  	Determine the average age and total charges for customers with multiple lines and online backup.
``` 
SELECT
    Multiple_Lines,
    Online_Backup,
    AVG(Age) AS AverageAge,
    AVG(Total_Charges) AS AverageTotalCharges
FROM Customers
GROUP BY Multiple_Lines, Online_Backup;
```
Output - 

  ![image](https://github.com/Shubham301099/Customer-Churn-Analysis-/assets/154081009/c1efce83-43de-45fb-81c5-e987c5785339)

- Identify the contract types with the highest churn rate among senior citizens (age 65 and over)
```
SELECT
    Contract_Type,
    COUNT(*) AS ChurnedCustomers,
    COUNT(*) / (SELECT COUNT(*) FROM Customers WHERE Age >= 60) AS ChurnRate
FROM Customers
WHERE Churn_Status = 'Yes' AND Age >= 60
GROUP BY Contract_Type
ORDER BY ChurnRate DESC;
```

Output - 

![image](https://github.com/Shubham301099/Customer-Churn-Analysis-/assets/154081009/deb4e68d-4d10-428d-97ae-112521f141ef)

- Calculate the average monthly charges for customers who have multiple lines and streaming TV.
```
SELECT
    Multiple_Lines,
    Streaming_TV,
    AVG(Monthly_Charges) AS AverageMonthlyCharges
FROM Customers
GROUP BY Multiple_Lines, Streaming_TV;
```
Output - 

![image](https://github.com/Shubham301099/Customer-Churn-Analysis-/assets/154081009/ec1ac1ba-21d2-4894-9afd-4fa2469648f7)

-	Identify the customers who have churned and used the most online services.
```
SELECT
    Customer_ID,
    Gender,
    Age,
    (CASE
        WHEN Online_Security = 'Yes' THEN 1 ELSE 0 END +
        CASE WHEN Online_Backup = 'Yes' THEN 1 ELSE 0 END +
        CASE WHEN Device_Protection = 'Yes' THEN 1 ELSE 0 END +
        CASE WHEN Tech_Support = 'Yes' THEN 1 ELSE 0 END) AS TotalOnlineServices
FROM Customers
WHERE Churn_Status = 'Yes'
ORDER BY TotalOnlineServices DESC
LIMIT 5;
```

Output - 

![image](https://github.com/Shubham301099/Customer-Churn-Analysis-/assets/154081009/41072fe6-a9d9-4ef2-838c-8629f2d92e55)

- Calculate the average age and total charges for customers with different combinations of streaming services.
```
SELECT CONCAT_WS(', ', 
                CASE WHEN Streaming_TV = 'Yes' THEN 'Streaming TV' ELSE NULL END,
                CASE WHEN Streaming_Movies = 'Yes' THEN 'Streaming Movies' ELSE NULL END
              ) AS Streaming_Services_Combination,
       AVG(Age) AS Avg_Age,
       SUM(Total_Charges) AS Total_Charges
FROM customers
GROUP BY Streaming_Services_Combination
ORDER BY Streaming_Services_Combination;
```
Output - 

![image](https://github.com/Shubham301099/Customer-Churn-Analysis-/assets/154081009/a8e195dc-db42-4d3c-8578-3d9de1ed6fca)

- Identify the gender distribution among customers who have churned and are on yearly contracts.

``` 
SELECT
    Gender,
    COUNT(*) AS ChurnedCustomers
FROM Customers
WHERE Churn_Status = 'Yes' AND Contract_Type = 'Yearly' 
GROUP BY Gender; 
```

Output - 

![image](https://github.com/Shubham301099/Customer-Churn-Analysis-/assets/154081009/d67b0373-32fd-4419-a291-f8a575aa56dc)

- Calculate the average monthly charges and total charges for customers who have churned, grouped by contract type and internet service type.
```
SELECT
    Contract_Type,
    Internet_Service,
    AVG(Monthly_Charges) AS AvgMonthlyCharges,
    AVG(Total_Charges) AS AvgTotalCharges
FROM Customers
WHERE Churn_Status = 'Yes'
GROUP BY Contract_Type, Internet_Service;
```
Output -

![image](https://github.com/Shubham301099/Customer-Churn-Analysis-/assets/154081009/ebe8d0cd-a5ed-4eb6-a6a3-1c1ffb5e93c5)

- Find the customers who have churned and are not using online services, and their average total charges. 
```
SELECT Customer_ID, 
       AVG(Total_Charges) AS Avg_Total_Charges_No_Online_Services
FROM customers 
WHERE Churn_Status = 'Yes'
      AND Online_Security = 'No'
      AND Online_Backup = 'No'
      AND Device_Protection = 'No'
      AND Tech_Support = 'No'
GROUP BY Customer_ID;
```
Output - 

![image](https://github.com/Shubham301099/Customer-Churn-Analysis-/assets/154081009/9ec364aa-300d-4477-bc20-ae08ed05173c)

- Calculate the average monthly charges and total charges for customers who have churned, grouped by the number of dependents.
```
SELECT
    Dependents,
    AVG(Monthly_Charges) AS AvgMonthlyCharges,
    AVG(Total_Charges) AS AvgTotalCharges
FROM Customers
WHERE Churn_Status = 'Yes'
GROUP BY Dependents;
```
Output - 

![image](https://github.com/Shubham301099/Customer-Churn-Analysis-/assets/154081009/68a705c6-5b34-4ae9-9980-95fdeb60ab61)

- 	Identify the customers who have churned, and their contract duration in months (for monthly contracts) 
```
SELECT
    Customer_ID,
    Gender,
    Age,
    Contract_Type,
    TIMESTAMPDIFF(MONTH, CURDATE(), DATE_ADD(CURDATE(), INTERVAL 1 MONTH)) AS ContractDurationMonths
FROM Customers
WHERE Churn_Status = 'Yes'
AND Contract_Type = 'Monthly';
```
Output - 

![image](https://github.com/Shubham301099/Customer-Churn-Analysis-/assets/154081009/2ced9f3f-4cd2-4f9d-9af1-20abd8208113)

- Determine the average age and total charges for customers who have churned, grouped by internet service and phone service. 
```
SELECT
    Internet_Service,
    Phone_Service,
    AVG(Age) AS AvgAge,
    AVG(Total_Charges) AS AvgTotalCharges
FROM Customers
WHERE Churn_Status = 'Yes'
GROUP BY Internet_Service, Phone_Service; 
```
Output -

![image](https://github.com/Shubham301099/Customer-Churn-Analysis-/assets/154081009/b536f610-3914-479e-a3cb-27c3a122584b)

-	Create a view to find the customers with the highest monthly charges in each contract type.  
```
CREATE VIEW Highest_MonthlyCharges_By_ContractType AS
SELECT Contract_Type, Customer_ID, Monthly_Charges
FROM (
    SELECT *,
           ROW_NUMBER() OVER (PARTITION BY Contract_Type ORDER BY Monthly_Charges DESC) AS rn
    FROM customers
) AS Ranked_Customers
WHERE rn = 1; 
```
Output -

![image](https://github.com/Shubham301099/Customer-Churn-Analysis-/assets/154081009/778b8be1-e96a-4211-984a-f987e6576f6a)

- Create a view to identify customers who have churned, and the average monthly charges compared to the overall average
```

CREATE VIEW ChurnedCustomerMonthlyCharges AS
SELECT
    Customer_ID,
    Gender,
    Age,
    Monthly_Charges,
    AVG(Monthly_Charges) OVER () AS OverallAvgMonthlyCharges
FROM Customers
where churn_status = 'Yes';
```
Output -
 
 ![image](https://github.com/Shubham301099/Customer-Churn-Analysis-/assets/154081009/ab0e16ba-d447-4ee4-9c2e-d4dc46dea855)

 - 	Create a view to find the customers who have churned and their cumulative total charges over time.
```
CREATE VIEW ChurnedCumulativeTotalCharges AS
SELECT
    Customer_ID,
    Gender,
    Age,
    Churn_Status,
    Total_Charges,
    SUM(Total_Charges) OVER (PARTITION BY Customer_ID ) AS CumulativeTotalCharges
FROM Customers
WHERE Churn_Status = 'Yes';
```
Output - 

![image](https://github.com/Shubham301099/Customer-Churn-Analysis-/assets/154081009/e683c107-4ef5-4a5b-b336-b9c8281df09e)



- Stored Procedure to Calculate Churn Rate 
```
DELIMITER //
CREATE PROCEDURE CalculateChurnRate()
BEGIN
    DECLARE total_customers INT;
    DECLARE churned_customers INT;
    DECLARE churn_rate DECIMAL(10,2);

    -- Total number of customers
    SELECT COUNT(*) INTO total_customers FROM Your_Table_Name;

    -- Number of churned customers
    SELECT COUNT(*) INTO churned_customers FROM Your_Table_Name WHERE Churn_Status = 'Yes';

    -- Calculating churn rate
    IF total_customers > 0 THEN
        SET churn_rate = (churned_customers / total_customers) * 100;
    ELSE
        SET churn_rate = 0;
    END IF;

    -- Displaying the churn rate
    SELECT churn_rate AS Churn_Rate;
END //
DELIMITER ; 
CALL CalculateChurnRate();
```
Output - 

![image](https://github.com/Shubham301099/Customer-Churn-Analysis-/assets/154081009/9b8f9b3a-3eee-4a8b-b4d5-ebc4122db0d6)

- Stored Procedure to Identify High-Value Customers at Risk of Churning. 
```
DELIMITER //
CREATE PROCEDURE IdentifyHighValueChurners(IN MonthlyThreshold DECIMAL(8, 2))
BEGIN
    SELECT Customer_ID, Gender, Age, Total_Charges, Monthly_Charges
    FROM Customers
    WHERE Churn_Status = 'Yes' AND Total_Charges > MonthlyThreshold * 6;
END //
DELIMITER ;


CALL IdentifyHighValueChurners(75.00);


DELIMITER //
```
Output – 

![image](https://github.com/Shubham301099/Customer-Churn-Analysis-/assets/154081009/6a42b058-2113-40a0-a14e-0816f54e64ba)




