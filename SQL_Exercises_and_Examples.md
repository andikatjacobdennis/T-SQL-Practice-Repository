| **Topic** | **Practice Question** | **T-SQL Answer** |
|-----------|------------------------|------------------|
| **Database Creation & Management** | Create a new database called `SalesDB`. | `CREATE DATABASE SalesDB;` |
| | Select and use the `SalesDB` database. | `USE SalesDB;` |
| | List all available databases on the server. | `SHOW DATABASES;` |
| | Rename `OldSalesDB` to `NewSalesDB`. | `ALTER DATABASE OldSalesDB MODIFY NAME = NewSalesDB;` |
| | Backup the `SalesDB` database. | `BACKUP DATABASE SalesDB TO DISK = 'C:\Backup\SalesDB.bak';` |
| | Restore the `SalesDB` database. | `RESTORE DATABASE SalesDB FROM DISK = 'C:\Backup\SalesDB.bak' WITH REPLACE;` |
| | Drop the database `TestDB`. | `DROP DATABASE TestDB;` |
| **Table Creation & Management** | Create a `Products` table with `ProductID` as the primary key, `ProductName` as unique, `Price` not null, and `Category` with a check constraint. | ```CREATE TABLE Products ( ProductID INT PRIMARY KEY, ProductName NVARCHAR(100) UNIQUE, Price DECIMAL(10, 2) NOT NULL, Category NVARCHAR(50) CHECK (Category IN ('Electronics', 'Clothing', 'Food', 'Furniture')) ); ``` |
| | Create a temporary table for storing product sales during a session. | ```CREATE TABLE #TempSales ( SaleID INT, ProductID INT, SaleAmount DECIMAL(10, 2) ); ``` |
| | Rename the table `OldProducts` to `NewProducts`. | ```EXEC sp_rename 'OldProducts', 'NewProducts'; ``` |
| | Add a new column `StockQuantity` to the `Products` table. | ```ALTER TABLE Products ADD StockQuantity INT; ``` |
| | Create a composite key on the `ProductID` and `OrderID` columns in the `OrderDetails` table. | `ALTER TABLE OrderDetails ADD CONSTRAINT PK_ProductOrder PRIMARY KEY (ProductID, OrderID);` |
| | Add a foreign key constraint to the `Sales` table referencing `Products`. | ```ALTER TABLE Sales ADD CONSTRAINT FK_Product FOREIGN KEY (ProductID) REFERENCES Products(ProductID); ``` |
| | Add a unique key constraint on the `ProductName` column in the `Products` table. | ```ALTER TABLE Products ADD CONSTRAINT UQ_ProductName UNIQUE (ProductName); ``` |
| | Drop the unique key constraint on the `ProductName` column. | ```ALTER TABLE Products DROP CONSTRAINT UQ_ProductName; ``` |
| | Truncate all records from the `Sales` table. | ```TRUNCATE TABLE Sales; ``` |
| | Permanently delete the `OldProducts` table. | ```DROP TABLE OldProducts; ``` |
| | Drop the `#TempSales` table. | ```DROP TABLE #TempSales; ``` |
| **Data Manipulation** | Insert a new product into the `Products` table with name 'Smartphone' and price 299. | `INSERT INTO Products (ProductID, ProductName, Price, Category) VALUES (1, 'Smartphone', 299, 'Electronics');` |
| | Update the price of the product 'Smartphone' to 350. | `UPDATE Products SET Price = 350 WHERE ProductName = 'Smartphone';` |
| | Delete all products where the price is below 10. | `DELETE FROM Products WHERE Price < 10;` |
| **Querying & Data Retrieval** | Retrieve all product names and their prices from the `Products` table. | `SELECT ProductName, Price FROM Products;` |
| | Insert all records from the `Products` table into a new table called `BackupProducts`. | `SELECT * INTO BackupProducts FROM Products;` |
| | Retrieve the top 5 most expensive products. | `SELECT TOP 5 * FROM Products ORDER BY Price DESC;` |
| | Retrieve the distinct product categories from the `Products` table. | `SELECT DISTINCT Category FROM Products;` |
| | Use COALESCE to return the first non-null value from a list. | `SELECT ProductName, COALESCE(Price, 0) AS Price FROM Products;` |
| | Retrieve all products and replace any NULL price values with 0. | `SELECT ProductName, ISNULL(Price, 0) AS Price FROM Products;` |
| | Retrieve all products where the price is greater than 50. | `SELECT * FROM Products WHERE Price > 50;` |
| | Retrieve all products where the price is greater than all products in the 'Clothing' category. | `SELECT * FROM Products WHERE Price > ALL (SELECT Price FROM Products WHERE Category = 'Clothing');` |
| | Retrieve all customers where the `phone_number` is NULL. | `SELECT * FROM Customers WHERE phone_number IS NULL;` |
| | Retrieve all sales where the sale amount is between 100 and 500. | `SELECT * FROM Sales WHERE SaleAmount BETWEEN 100 AND 500;` |
| | Retrieve all products where the category is either 'Electronics' or 'Clothing'. | `SELECT * FROM Products WHERE Category = 'Electronics' OR Category = 'Clothing';` |
| | Retrieve all products that are not in the 'Clothing' category. | `SELECT * FROM Products WHERE Category <> 'Clothing';` |
| | Retrieve all product names that start with the letter 'S'. | `SELECT ProductName FROM Products WHERE ProductName LIKE 'S%';` |
| **Aggregations and Grouping** | Find the total sales amount from the `Sales` table. | `SELECT SUM(SaleAmount) AS TotalSales FROM Sales;` |
| | Group all products by category and count how many products exist in each category. | `SELECT Category, COUNT(*) AS ProductCount FROM Products GROUP BY Category HAVING COUNT(*) > 5;` |
| | Retrieve the minimum and maximum product prices. | `SELECT MIN(Price) AS MinPrice, MAX(Price) AS MaxPrice FROM Products;` |
| | Find the average price of all products. | `SELECT AVG(Price) AS AveragePrice FROM Products;` |
| **Joins & Set Operations** | Retrieve all sales with the corresponding product name by joining `Sales` and `Products` tables. | `SELECT Sales.*, Products.ProductName FROM Sales INNER JOIN Products ON Sales.ProductID = Products.ProductID;` |
|  | Retrieve the customer name and the corresponding sales amount by joining `Customers` and `Sales` tables. | `SELECT Customers.CustomerName, Sales.SaleAmount FROM Sales INNER JOIN Customers ON Sales.CustomerID = Customers.CustomerID;` |
| | Retrieve all products and their sales data, showing NULL for products that haven’t been sold. | `SELECT Products.ProductName, Sales.SaleAmount FROM Products LEFT JOIN Sales ON Products.ProductID = Sales.ProductID;` |
| | Retrieve all sales and the product details, showing NULL for products that no longer exist. | `SELECT Sales.SaleID, Products.ProductName FROM Sales RIGHT JOIN Products ON Sales.ProductID = Products.ProductID;` |
| | Retrieve all products and their sales information, even if there is no match on either side. | `SELECT Products.ProductName, Sales.SaleAmount FROM Products FULL JOIN Sales ON Products.ProductID = Sales.ProductID;` |
| | Retrieve all products that have the same price by joining the `Products` table to itself. | `SELECT A.ProductName, B.ProductName FROM Products A INNER JOIN Products B ON A.Price = B.Price AND A.ProductID <> B.ProductID;` |
| | Retrieve all possible combinations of products and customers. | `SELECT Products.ProductName, Customers.CustomerName FROM Products CROSS JOIN Customers;` |
| | Retrieve the union of sales amounts from different sales channels. | `SELECT SaleAmount FROM OnlineSales UNION SELECT SaleAmount FROM StoreSales;` |
| | Retrieve sales that are not present in the online sales. | `SELECT SaleAmount FROM Sales EXCEPT SELECT SaleAmount FROM OnlineSales;` |
| | Retrieve sales amounts that are common to both online and store sales. | `SELECT SaleAmount FROM OnlineSales INTERSECT SELECT SaleAmount FROM StoreSales;` |
| **Conditional Expressions** | Retrieve all products and add a new column showing 'Expensive' if the price is above 100, otherwise 'Cheap'. | `SELECT ProductName, Price, StockQuantity, CASE WHEN Price > 200 THEN 'Luxury' WHEN Price BETWEEN 100 AND 200 THEN 'Expensive' WHEN Price BETWEEN 50 AND 100 THEN 'Moderate' ELSE 'Cheap' END AS PriceCategory, CASE WHEN StockQuantity = 0 THEN 'Out of Stock' ELSE 'In Stock' END AS StockStatus FROM Products;` |
| **Indexes** | Create an index on the `ProductName` column for faster searches. | `CREATE INDEX idx_ProductName ON Products (ProductName);` |
| | Retrieve all indexes on the `Products` table. | `SELECT * FROM sys.indexes WHERE object_id = OBJECT_ID('Products');` |
| | Create a unique index on the `ProductCode` column in the `Products` table. | `CREATE UNIQUE INDEX idx_ProductCode ON Products (ProductCode);` |
| | Create a clustered index on the `ProductID` column in the `Products` table. | `CREATE  CLUSTERED INDEX idx_ProductID ON Products (ProductID);` |
| | Create a non-clustered index on the `ProductName` column. | `CREATE NONCLUSTERED INDEX idx_ProductName ON Products (ProductName);` |
| | Drop the index on the `ProductName` column. | `DROP INDEX idx_ProductName ON Products;` |
| **Views** | Create a view that shows product names and their prices. | `CREATE VIEW ProductPriceView AS SELECT ProductName, Price FROM Products;` |
| | Update the ProductPriceView to include the stock quantity for each product. | `CREATE OR ALTER VIEW ProductPriceView AS SELECT ProductName, Price FROM Products;` |
| | Rename `ProductSalesView` to `ProductRevenueView`. | `EXEC sp_rename 'ProductSalesView', 'ProductRevenueView';` |
| | Drop the `ObsoleteView` from the database. | `DROP VIEW ObsoleteView;` |
| **Views & Permissions** | Create a view to show product details and grant select permissions to a user. | `CREATE VIEW vw_ProductDetails AS SELECT * FROM Products; GRANT SELECT ON vw_ProductDetails TO UserName;` |
| | Revoke select permissions on the view from the user. | `REVOKE SELECT ON vw_ProductDetails FROM UserName;` |
| **Stored Procedures** | Create a stored procedure to add a new sale record to the `Sales` table. | `CREATE PROCEDURE AddSale @ProductID INT, @SaleAmount DECIMAL(10, 2), @SaleDate DATE AS BEGIN INSERT INTO Sales (ProductID, SaleAmount, SaleDate) VALUES (@ProductID, @SaleAmount, @SaleDate); END;` |
| | Execute this procedure to add a sale record | `EXEC AddSale @ProductID = 1, @SaleAmount = 100.50, @SaleDate = '2024-09-17';` |
| | To update the procedure and add a `CustomerID` parameter | `CREATE OR ALTER PROCEDURE AddSale @ProductID INT, @SaleAmount DECIMAL(10, 2), @SaleDate DATE, @CustomerID INT AS BEGIN INSERT INTO Sales (ProductID, SaleAmount, SaleDate, CustomerID) VALUES (@ProductID, @SaleAmount, @SaleDate, @CustomerID); END;` |
| | Delete Stored Procedure | `DROP PROCEDURE AddSale;`
| **User-Defined Functions** | Create a scalar-valued function to calculate the total price after a discount. | `CREATE FUNCTION dbo.ApplyDiscount (@Price DECIMAL(10, 2), @Discount DECIMAL(5, 2)) RETURNS DECIMAL(10, 2) AS BEGIN RETURN @Price - (@Price * @Discount / 100); END;` |
| | Use the function to calculate the final price with a 10% discount. | `SELECT dbo.ApplyDiscount(100, 10) AS FinalPrice;` |
| **Trigger** | Create a trigger that updates the stock quantity in the `Products` table after every insert into `Sales`. | `CREATE TRIGGER UpdateStock AFTER INSERT ON Sales FOR EACH ROW BEGIN UPDATE Products SET StockQuantity = StockQuantity - 1 WHERE ProductID = NEW.ProductID; END;` |
| **Transactions** | Begin a transaction for inserting a new product with error handling | |

```sql

-- Save transaction state with a save point and partial rollback
BEGIN TRANSACTION;
BEGIN TRY
    INSERT INTO Products (ProductID, ProductName, Price, Category)
    VALUES (5, 'Mouse', 25, 'Accessories');

    -- Check the current transaction count
    SELECT @@TRANCOUNT;

    SAVE TRANSACTION SavePoint1;
    
    INSERT INTO Products (ProductID, ProductName, Price, Category)
    VALUES (6, 'Monitor', 150, 'Electronics');
    
    -- Rollback to save point if error occurs after SavePoint1
    IF (@@ERROR <> 0)
        ROLLBACK TRANSACTION SavePoint1;

    COMMIT TRANSACTION;
END TRY
BEGIN CATCH
    -- Handle errors
    ROLLBACK TRANSACTION;
    SELECT ERROR_MESSAGE(), ERROR_NUMBER(), ERROR_SEVERITY(), ERROR_STATE(), ERROR_PROCEDURE(), ERROR_LINE();
END CATCH;

```

| **Topic** | **Practice Question** | **T-SQL Answer** |
|-----------|------------------------|------------------|
| **Cursors** | Create a cursor to loop through all products and update prices for each product. |  |

```sql
-- Declare a cursor for selecting ProductID and ProductName from Products
DECLARE @ProductID INT, @ProductName NVARCHAR(50);

DECLARE ProductCursor CURSOR FOR
SELECT ProductID, ProductName
FROM Products;

-- Open the cursor
OPEN ProductCursor;

-- Fetch the first row
FETCH NEXT FROM ProductCursor INTO @ProductID, @ProductName;

-- Loop through the rows until there are no more rows to fetch
WHILE @@FETCH_STATUS = 0
BEGIN
    -- Process each row (for example, print the product details)
    PRINT 'Product ID: ' + CAST(@ProductID AS NVARCHAR(10)) + ', Product Name: ' + @ProductName;

    -- Fetch the next row
    FETCH NEXT FROM ProductCursor INTO @ProductID, @ProductName;
END;

-- Close the cursor when done
CLOSE ProductCursor;

-- Deallocate the cursor to free up resources
DEALLOCATE ProductCursor;
```

| **Topic** | **Practice Question** | **T-SQL Answer** |
|-----------|------------------------|------------------|
| **Common Table Expressions** | Use a CTE to retrieve the top 5 most expensive products. |  |

```sql

WITH ExpensiveProducts AS (
    SELECT 
        ProductName, 
        Price, 
        ROW_NUMBER() OVER (ORDER BY Price DESC) AS RowNum
    FROM Products
)
SELECT 
    ProductName, 
    Price
FROM ExpensiveProducts
WHERE RowNum <= 5;

```

| **Topic** | **Practice Question** | **T-SQL Answer** |
|-----------|------------------------|------------------|
| **Dynamic SQL** | Write a dynamic SQL query to retrieve products based on a variable category. | |

```sql
DECLARE @Category NVARCHAR(50);
SET @Category = 'Electronics';

DECLARE @SQL NVARCHAR(MAX);
SET @SQL = 'SELECT * FROM Products WHERE Category = ''' + @Category + '''';

EXEC sp_executesql @SQL;
```

| **Topic** | **Practice Question** | **T-SQL Answer** |
|-----------|------------------------|------------------|
| **Window Functions** | Calculate the cumulative sales amount for each product using `OVER()`. | `SELECT ProductID, SaleAmount, SUM(SaleAmount) OVER (PARTITION BY ProductID ORDER BY SaleDate) AS CumulativeSales FROM Sales;` |
| | Retrieve the rank of products based on their price. | `SELECT ProductName, Price, RANK() OVER (ORDER BY Price DESC) AS PriceRank FROM Products;` |
| **Partitioning** | Partition sales data by product category and calculate total sales in each partition. | `SELECT Category, SUM(SaleAmount) OVER (PARTITION BY Category) AS TotalSales FROM Products P INNER JOIN Sales S ON P.ProductID = S.ProductID;` |
| | Partition data and calculate row number within each partition for products. | `SELECT ProductName, Category, ROW_NUMBER() OVER (PARTITION BY Category ORDER BY Price DESC) AS RowNum FROM Products;` |
| **Full-Text Search** | Perform a full-text search for products containing the word 'Phone'. | `SELECT * FROM Products WHERE CONTAINS(ProductName, 'Phone');` |
| | Enable full-text search on the `ProductName` column and search for records containing 'Watch'. | `CREATE FULLTEXT CATALOG ftCatalog AS DEFAULT; CREATE FULLTEXT INDEX ON Products (ProductName) KEY INDEX PK_ProductID; SELECT * FROM Products WHERE CONTAINS(ProductName, 'Watch');` |
| **Pivoting** | Pivot the sales data to show total sales by product for each year. | `SELECT * FROM (SELECT ProductID, YEAR(SaleDate) AS SaleYear, SaleAmount FROM Sales) AS SourceTable PIVOT (SUM(SaleAmount) FOR SaleYear IN ([2021], [2022], [2023])) AS PivotTable;` |
| | Use `PIVOT` to display total sales for each product category. | `SELECT * FROM (SELECT Category, SaleAmount FROM Products P INNER JOIN Sales S ON P.ProductID = S.ProductID) AS SourceTable PIVOT (SUM(SaleAmount) FOR Category IN ('Electronics', 'Clothing', 'Accessories')) AS PivotTable;` |
| **Unpivoting** | Unpivot sales data to display product sales for each year in rows. | `SELECT ProductID, SaleYear, SaleAmount FROM (SELECT ProductID, [2021], [2022], [2023] FROM SalesByYear) AS PivotTable UNPIVOT (SaleAmount FOR SaleYear IN ([2021], [2022], [2023])) AS UnpivotTable;` |
| **XML Data Handling** | Create an XML column in a table to store product details. | `ALTER TABLE Products ADD ProductDetails XML;` |
| | Insert XML data into the `ProductDetails` column of the `Products` table. | `UPDATE Products SET ProductDetails = '<Product><Name>Smartphone</Name><Price>299</Price></Product>' WHERE ProductID = 1;` |
| | Query the `ProductDetails` XML column to retrieve the product name. | `SELECT ProductDetails.value('(/Product/Name)[1]', 'NVARCHAR(100)') AS ProductName FROM Products WHERE ProductID = 1;` |
| | Extract the price from the `ProductDetails` XML column. | `SELECT ProductDetails.value('(/Product/Price)[1]', 'DECIMAL(10, 2)') AS Price FROM Products WHERE ProductID = 1;` |
| | Update the `ProductDetails` XML to set a new price. | `UPDATE Products SET ProductDetails.modify('replace value of (/Product/Price/text())[1] with 350') WHERE ProductID = 1;` |
| **JSON Data Handling** | Create a column in a table to store JSON data. | `ALTER TABLE Products ADD ProductAttributes NVARCHAR(MAX);` |
| | Insert JSON data into the `ProductAttributes` column of the `Products` table. | `UPDATE Products SET ProductAttributes = '{"Color": "Black", "Warranty": "1 Year"}' WHERE ProductID = 1;` |
| | Query the `ProductAttributes` JSON column to retrieve the warranty information. | `SELECT JSON_VALUE(ProductAttributes, '$.Warranty') AS Warranty FROM Products WHERE ProductID = 1;` |
| | Extract the color from the `ProductAttributes` JSON column. | `SELECT JSON_VALUE(ProductAttributes, '$.Color') AS Color FROM Products WHERE ProductID = 1;` |
| | Update the `ProductAttributes` JSON to add a new attribute. | `UPDATE Products SET ProductAttributes = JSON_MODIFY(ProductAttributes, '$.Weight', '150g') WHERE ProductID = 1;` |

