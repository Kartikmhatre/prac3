## Code

```python
# Practical 5: Snowflake Schema
import sqlite3

conn = sqlite3.connect("sales_warehouse.db")
cursor = conn.cursor()

cursor.executescript("""
DROP TABLE IF EXISTS Fact_Sales;
DROP TABLE IF EXISTS Dim_Customer;
DROP TABLE IF EXISTS Dim_Product;
DROP TABLE IF EXISTS Dim_State;
DROP TABLE IF EXISTS Dim_Category;

CREATE TABLE Dim_Category (CategoryID INTEGER PRIMARY KEY, CategoryName TEXT);
CREATE TABLE Dim_State (StateID INTEGER PRIMARY KEY, StateName TEXT, Country TEXT);
CREATE TABLE Dim_Product (ProductID INTEGER PRIMARY KEY, ProductName TEXT, CategoryID INTEGER, FOREIGN KEY (CategoryID) REFERENCES Dim_Category(CategoryID));
CREATE TABLE Dim_Customer (CustomerID INTEGER PRIMARY KEY, CustomerName TEXT, StateID INTEGER, FOREIGN KEY (StateID) REFERENCES Dim_State(StateID));
CREATE TABLE Fact_Sales (SalesID INTEGER PRIMARY KEY, ProductID INTEGER, CustomerID INTEGER, Quantity INTEGER, Amount REAL, FOREIGN KEY (ProductID) REFERENCES Dim_Product(ProductID), FOREIGN KEY (CustomerID) REFERENCES Dim_Customer(CustomerID));
""")

cursor.executescript("""
INSERT OR REPLACE INTO Dim_Category VALUES (1, 'Electronics');
INSERT OR REPLACE INTO Dim_State VALUES (10, 'Maharashtra', 'India');
INSERT OR REPLACE INTO Dim_Product VALUES (101, 'Laptop', 1);
INSERT OR REPLACE INTO Dim_Customer VALUES (1001, 'Rahul Sharma', 10);
INSERT OR REPLACE INTO Fact_Sales VALUES (1, 101, 1001, 2, 120000.00);
""")

conn.commit()

print("\n=== Dim_Category ===")
for row in cursor.execute("SELECT * FROM Dim_Category"):
    print(row)

print("\n=== Dim_State ===")
for row in cursor.execute("SELECT * FROM Dim_State"):
    print(row)

print("\n=== Dim_Product ===")
for row in cursor.execute("SELECT * FROM Dim_Product"):
    print(row)

print("\n=== Dim_Customer ===")
for row in cursor.execute("SELECT * FROM Dim_Customer"):
    print(row)

print("\n=== Fact_Sales ===")
for row in cursor.execute("SELECT * FROM Fact_Sales"):
    print(row)

conn.close()
```
