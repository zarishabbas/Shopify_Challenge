## Question 1: Given some sample data, write a program to answer the following: click here to access the required data set

On Shopify, we have exactly 100 sneaker shops, and each of these shops sells only one model of shoe. We want to do some analysis of the average order value (AOV). When we look at orders data over a 30 day window, we naively calculate an AOV of $3145.13. Given that we know these shops are selling sneakers, a relatively affordable item, something seems wrong with our analysis. 

### Question 1.1 Think about what could be going wrong with our calculation. Think about a better way to evaluate this data. 

    #### Answer:
    #### 1.1: The average order value can often be a useful metric to evaluate the data. However, the presence of outliers can skew the AOV and greatly reduce the usefulness of this metric.This is what is happening in the data here. Two main issues stand out:
    a) Less than 1% (0.33% to be specific) of total orders come from a single user that buys 2000 sneakers every few days, compared to other users that average around 2 pairs of sneakers per order.The orders are placed at exactly 4 o’clock so it seems that it is an automated purchase. 
    b) Shop number 78 sells a single sneaker for $154,350. This amount per sneaker is about 1000 times greater than the average cost of a sneaker in the other shops. 

    In order to address these issues, the first step is to evaluate the legitimacy of these outliers and understand the reason for obtaining an AOV. Let's assume that these outliers are legitimate purchases, and an AOV is being asked for to gain an understanding of the current market for the sneakers. In that case, the AOV of the total data gives no useful information on the majority of orders. Thus, the most straightforward way to evaluate the data is to separate the dataset into these three different groups - the majority or orders, the shop with an extraordinarily high AOV, and the user with an extraordinarily high order amount. 

    Another way to address these issues is to look at the median of the order value, instead of the average. This is because the median is robust to outliers. However, in choosing to use the median, we lose information that may be relevant about the multiple groups that make up the customers in the data set.   

   

### Question 1.2: What metric would you report for this dataset?

    #### Answer:
    #### 1.2: I would report the median order value, and the interquartile range. However, I would also give the option to look at the average order value of the three different groups detected in the above exploration. 

### Question 1.3: What is its value?

    #### Depending on the situtaion and the audience, I would report either a short answer, or a long answe as follows:

    Short answer: The median order value is $284, with the middle 50% of the orders being between $163 and $390. However, extreme outliers with very high order amounts or order values are present in the data.

    Long answer: The median order value is $284, with the middle 50% of the orders being between $163 and $390. However, extreme outliers are present in the data. These outliers come from two sources: 1) A shop that sells a single sneaker for $154,350 (AOV = $49,213, SD = 26,472). 2) A user that buys 2000 sneakers every few days (AOV: $704,000, SD = 0).
    The AOV excluding this 1.26% of orders that clearly define a different population is $302.58 (SD = 160.80). The median is similar to the median when outliers are included (median = 302.58)

## Question 2: For this question you’ll need to use SQL. Follow this link to access the data set required for the challenge. Please use queries to answer the following questions. Paste your queries along with your final numerical answers below.

### 2.1: How many orders were shipped by Speedy Express in total?

    #### Answer:
    #### 2.1: There were 54 orders shipped by Speedy Express.

    Code is as follows:
```
SELECT COUNT (*)
	FROM Orders JOIN Shippers 
    ON Shippers.ShipperID = Orders.ShipperID
    WHERE Shippers.ShipperName = 'Speedy Express';
```

### 2.2 What is the last name of the employee with the most orders?

    #### Answer:
    #### 2.2: The last name of the employee with the most orders is 'Peacock' - They had 40 orders.

    Code is as follows:
```
SELECT Employees.LastName, COUNT (Orders.EmployeeID) AS most_common
	FROM Orders
    JOIN Employees ON Orders.EmployeeID = Employees.EmployeeID
    GROUP BY Orders.EmployeeID
    ORDER BY most_common DESC
    LIMIT 1;
```

### 2.3: What product was ordered the most by customers in Germany?

    #### Answer:
    #### 2.3: The product that was ordered the most in terms of the highest quantity by customers in Germany was 'Boston Crab Meat'- the quantity was 160. 
              The product that was ordered the most in terms of frequency was Gorgonzola Telino, which was ordered 5 times

    Code is as follows for most orders by quantity:
```
SELECT Products.ProductName, COUNT(Orders.OrderID), SUM(OrderDetails.quantity) AS most_orders
    FROM Orders
    JOIN OrderDetails ON Orders.OrderID = OrderDetails.OrderID
    JOIN Products ON OrderDetails.ProductID = Products.ProductID
    JOIN Customers ON Orders.CustomerID = Customers.CustomerID
    WHERE Customers.Country = 'Germany'
    GROUP BY Products.ProductName
    ORDER BY most_orders DESC
    LIMIT 1;
```
    Code is as follows for most orders by frequency:

```
SELECT Products.ProductName, COUNT(Orders.OrderID) AS most_frequent
    FROM Orders
    JOIN OrderDetails ON Orders.OrderID = OrderDetails.OrderID
    JOIN Products ON OrderDetails.ProductID = Products.ProductID
    JOIN Customers ON Orders.CustomerID = Customers.CustomerID
    WHERE Customers.Country = 'Germany'
    GROUP BY Products.ProductName
    ORDER BY most_frequent DESC
    LIMIT 1;
```
