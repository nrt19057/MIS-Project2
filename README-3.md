
# Team Name

21479 Group 5




## Team Name
Sydney James -  [@srj44873](https://github.com/srj44873)

Mary Earhart -  [@marykearhart](https://github.com/marykearhart)

Derek Quinlan - [@derekq111](https://github.com/derekq111)

Regan Taylor - [@nrt19057](https://github.com/nrt19057)
## Problem Description
Our data model is for a fictional company called "Terry Teas" the company is a tea shop selling nutrition teas. The database is for the day to day operation of the company. The database includes tables such as Customers, Orders, OrderDetails, LoyaltyPrograms, etc. Our database would help a manager visualise simple things such as listing all customer names, or a more complex query such as total quantity sold for each product. 

## Data Model

Explanation of Data Model:
Our data model represents the structure of a retail and inventory management system that tracks customers, sales, products, suppliers, employees, and store operations.

At the core of the model is the Customers entity, which stores personal information such as name, contact details, and address. Each customer can make multiple purchases, which is why there is a one-to-many relationship between the Customers and Sales entities.

The Sales entity records each transaction, including the sale price, date, and payment method. Because a single sale can involve multiple products, we created an associative entity called Product Line, which connects Sales and Products. This resolves the many-to-many relationship between Sales and Products, allowing each sale to include multiple products and each product to appear in multiple sales.

The Products entity stores details about each item, including product name and type. Products are supplied by vendors, which is represented through the Suppliers entity. Since suppliers can provide multiple products and products can come from multiple suppliers, we use the Supplier Orders associative entity to connect Suppliers and Products. This table also links to Store Location, indicating where products are being delivered.

The Store Location entity represents different store branches, including attributes such as city and state. Supplier Orders are tied to specific locations, establishing a one-to-many relationship between Store Location and Supplier Orders.

Customer purchasing activity is further detailed through the Customer Orders and Customer Order Details entities. Customer Orders represent overall orders placed by customers, while Customer Order Details break down each order into specific products, quantities, shipping dates, and statuses. This creates a structured relationship where:
- One customer can have many orders
- One order can have many order details (products)

To manage promotions, we included a Discount Programs entity. This entity links customers, sales, and specific discount programs, allowing discounts to be applied to particular transactions. This creates relationships between Customers, Sales, and Discount Programs to track promotional usage.

On the organizational side, the Employees entity stores employee information such as name, email, and department. Each employee belongs to a Department, forming a many-to-one relationship (many employees per department). Additionally, the Employees table includes a self-referencing relationship (via PreviousChair), which models succession or reporting structure within the organization.

Finally, the Department entity defines different functional areas within the company, such as sales, management, or operations.

Overall, this data model efficiently captures the relationships between customers, transactions, products, suppliers, and employees, while resolving complex many-to-many relationships through associative entities like Product Line, Supplier Orders, and Customer Order Details.
## Data Dictionary
## Queries

Query 1 - Simple Question: What customers are currently in the database? Why it matters: A manager may want a full customer list for outreach, customer service, or record review.

SELECT customerID, firstName, lastName, emailAddress, phoneNumber FROM Customers ORDER BY lastName, firstName;

Query 2 - Simple Question: What products does the business carry? Why it matters: A manager may want a current list of products for catalog review, sales planning, or inventory coordination.

SELECT productID, productName, productType FROM Products ORDER BY productType, productName;

Query 3 - Simple Question: What suppliers are in the system? Why it matters: A manager may need a supplier directory when reviewing vendor relationships or sourcing issues.

SELECT SupplierID, supName FROM Suppliers ORDER BY supName;

Query 4 - Simple Question: What store locations are recorded in the database? Why it matters: A manager may want to review all operating locations for staffing or supplier coordination.

SELECT LocationID, city, state, areaCode FROM Store Location ORDER BY state, city;

Query 5 - Complex Question: Which customers have generated the most total order revenue? Why it matters: A manager can use this to identify the highest-value customers and focus retention efforts on them.

SELECT c.customerID, c.firstName, c.lastName, SUM(o.amount) AS total_spent FROM Customers c JOIN Orders o ON c.customerID = o.customerID GROUP BY c.customerID, c.firstName, c.lastName ORDER BY total_spent DESC;

Query 6 - Complex Question: Which products have sold the highest total quantity? Why it matters: A manager can use this to identify top-selling products and make pricing, stocking, and promotion decisions.

SELECT p.productID, p.productName, p.productType, SUM(od.orderqty) AS total_quantity_sold FROM Products p JOIN Order Details od ON p.productID = od.productID GROUP BY p.productID, p.productName, p.productType ORDER BY total_quantity_sold DESC;

Query 7 - Complex Question: Which products have generated the most order revenue? Why it matters: A manager can use this to distinguish products that drive revenue, not just volume.

SELECT p.productID, p.productName, SUM(od.orderPrice * od.orderqty) AS total_product_revenue FROM Products p JOIN Order Details od ON p.productID = od.productID GROUP BY p.productID, p.productName ORDER BY total_product_revenue DESC;

Query 8 - Complex Question: Which products have never been included in any order? Why it matters: A manager can use this to identify underperforming or unused products that may need promotion or discontinuation.

SELECT p.productID, p.productName, p.productType FROM Products p WHERE NOT EXISTS ( SELECT 1 FROM Order Details od WHERE od.productID = p.productID ) ORDER BY p.productName;

Query 9 - Complex Question: Which customers have spent more than the average order amount in total? Why it matters: A manager can use this to identify above-average customers for loyalty offers or targeted marketing.

SELECT c.customerID, c.firstName, c.lastName, SUM(o.amount) AS total_customer_spent FROM Customers c JOIN Orders o ON c.customerID = o.customerID GROUP BY c.customerID, c.firstName, c.lastName HAVING SUM(o.amount) > ( SELECT AVG(amount) FROM Orders ) ORDER BY total_customer_spent DESC;

Query 10 - Complex Question: How many products does each supplier provide to each store location? Why it matters: A manager can use this to evaluate supplier coverage by location and spot sourcing concentration or gaps.

SELECT s.supName, sl.city, sl.state, COUNT(so.productID) AS products_supplied FROM Suppliers s JOIN Supplier Orders so ON s.SupplierID = so.SupplierID JOIN Store Location sl ON so.LocationID = sl.LocationID GROUP BY s.supName, sl.city, sl.state ORDER BY s.supName, sl.state, sl.city;
## Database Information