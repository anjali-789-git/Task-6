

1. Subquery in SELECT (Scalar Subquery)

Find each customer’s total spending using a subquery inside SELECT.

SELECT 
    Name,
    (SELECT SUM(TotalAmount) 
     FROM Orders 
     WHERE Orders.CustomerID = Customers.CustomerID) AS TotalSpent
FROM Customers;

Explanation:

Inner query calculates total spending for one customer.

Outer query lists all customers with that calculated value.



---

2. Subquery in WHERE with IN

Get the names of customers who bought products in the "Fashion" category.

SELECT Name 
FROM Customers
WHERE CustomerID IN (
    SELECT DISTINCT o.CustomerID
    FROM Orders o
    JOIN OrderDetails od ON o.OrderID = od.OrderID
    WHERE od.ProductID IN (
        SELECT ProductID
        FROM ProductCategory
        WHERE CategoryID = (
            SELECT CategoryID 
            FROM Categories 
            WHERE CategoryName = 'Fashion'
        )
    )
);

Explanation:

Nested subqueries filter by category name → category ID → product ID → orders → customers.



---

3. Subquery in FROM (Derived Table)

Get products with their total quantity sold using a subquery in FROM.

SELECT ProductName, TotalSold
FROM (
    SELECT 
        p.ProductID, 
        p.ProductName, 
        SUM(od.Quantity) AS TotalSold
    FROM Products p
    JOIN OrderDetails od ON p.ProductID = od.ProductID
    GROUP BY p.ProductID, p.ProductName
) AS ProductSales
WHERE TotalSold > 0;

Explanation:

Inner query aggregates sales per product.

Outer query filters results.



---

4. Correlated Subquery with EXISTS

Find customers who have placed at least one order above ₹10,000.

SELECT Name
FROM Customers c
WHERE EXISTS (
    SELECT 1
    FROM Orders o
    WHERE o.CustomerID = c.CustomerID
    AND o.TotalAmount > 10000
);

Explanation:

EXISTS checks if a matching order exists for each customer.



---

5. Subquery with =

Get the most expensive product(s).

SELECT ProductName, Price
FROM Products
WHERE Price = (
    SELECT MAX(Price) FROM Products
);

Explanation:

Inner query finds highest price.

Outer query returns products with that price.
