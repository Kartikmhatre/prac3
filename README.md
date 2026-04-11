# Practical 3
Create snowflake schema with dimension tables, sub-dimension tables, and fact table.

## Code

```python
# Practical 5: Snowflake Schema (Final Code - No Errors)
import sqlite3

conn = sqlite3.connect("sales_warehouse.db")
cursor = conn.cursor()

cursor.executescript("""
CREATE TABLE IF NOT EXISTS Dim_Category (
    CategoryID INTEGER PRIMARY KEY,
    CategoryName TEXT
);

CREATE TABLE IF NOT EXISTS Dim_Country (
    CountryID INTEGER PRIMARY KEY,
    CountryName TEXT
);

CREATE TABLE IF NOT EXISTS Dim_State (
    StateID INTEGER PRIMARY KEY,
    StateName TEXT,
    CountryID INTEGER,
    FOREIGN KEY (CountryID) REFERENCES Dim_Country(CountryID)
);

CREATE TABLE IF NOT EXISTS Dim_City (
    CityID INTEGER PRIMARY KEY,
    CityName TEXT,
    StateID INTEGER,
    FOREIGN KEY (StateID) REFERENCES Dim_State(StateID)
);

CREATE TABLE IF NOT EXISTS Dim_Product (
    ProductID INTEGER PRIMARY KEY,
    ProductName TEXT,
    CategoryID INTEGER,
    FOREIGN KEY (CategoryID) REFERENCES Dim_Category(CategoryID)
);

CREATE TABLE IF NOT EXISTS Dim_Customer (
    CustomerID INTEGER PRIMARY KEY,
    CustomerName TEXT,
    CityID INTEGER,
    FOREIGN KEY (CityID) REFERENCES Dim_City(CityID)
);

CREATE TABLE IF NOT EXISTS Dim_Date (
    DateID INTEGER PRIMARY KEY,
    Day INTEGER,
    Month TEXT,
    Year INTEGER,
    Quarter TEXT
);

CREATE TABLE IF NOT EXISTS Fact_Sales (
    SalesID INTEGER PRIMARY KEY,
    ProductID INTEGER,
    CustomerID INTEGER,
    DateID INTEGER,
    Quantity INTEGER,
    Amount REAL,
    FOREIGN KEY (ProductID) REFERENCES Dim_Product(ProductID),
    FOREIGN KEY (CustomerID) REFERENCES Dim_Customer(CustomerID),
    FOREIGN KEY (DateID) REFERENCES Dim_Date(DateID)
);
""")

cursor.executescript("""
INSERT OR REPLACE INTO Dim_Category VALUES (1, 'Electronics');
INSERT OR REPLACE INTO Dim_Product VALUES (101, 'Laptop', 1);
INSERT OR REPLACE INTO Dim_Country VALUES (1, 'India');
INSERT OR REPLACE INTO Dim_State VALUES (10, 'Maharashtra', 1);
INSERT OR REPLACE INTO Dim_City VALUES (100, 'Mumbai', 10);
INSERT OR REPLACE INTO Dim_Customer VALUES (1001, 'Rahul Sharma', 100);
INSERT OR REPLACE INTO Dim_Date VALUES (501, 12, 'November', 2025, 'Q4');
INSERT OR REPLACE INTO Fact_Sales VALUES (1, 101, 1001, 501, 2, 120000.00);
""")

conn.commit()

print("\n===== SUB-DIMENSION TABLES =====")
print("\nDim_Category:")
for row in cursor.execute("SELECT * FROM Dim_Category"):
    print(row)

print("\nDim_Country:")
for row in cursor.execute("SELECT * FROM Dim_Country"):
    print(row)

print("\nDim_State:")
for row in cursor.execute("SELECT * FROM Dim_State"):
    print(row)

print("\nDim_City:")
for row in cursor.execute("SELECT * FROM Dim_City"):
    print(row)

print("\n===== DIMENSION TABLES =====")
print("\nDim_Product:")
for row in cursor.execute("SELECT * FROM Dim_Product"):
    print(row)

print("\nDim_Customer:")
for row in cursor.execute("SELECT * FROM Dim_Customer"):
    print(row)

print("\nDim_Date:")
for row in cursor.execute("SELECT * FROM Dim_Date"):
    print(row)

print("\n===== FACT TABLE =====")
print("\nFact_Sales:")
for row in cursor.execute("SELECT * FROM Fact_Sales"):
    print(row)

conn.close()
```
