# Stock Market Database

This repository contains SQL scripts for creating and populating two tables related to a stock market dataset: `Companies` and `StockPrices`. These tables represent information about companies and their corresponding stock prices.

## Table Definitions

### 1. `Companies` Table:

```sql
CREATE TABLE Companies (
    CompanyId INT PRIMARY KEY,
    CompanyName VARCHAR(50),
    Sector VARCHAR(50)
);

INSERT INTO Companies VALUES
(1, 'ABC Corp', 'Technology'),
(2, 'XYZ Inc', 'Finance'),
(3, 'Sky Corp', 'Technology'),
(4, 'Ray gold', 'Finance'),
(5, 'KK business Corp', 'Technology'),
(6, 'Yummy Inc', 'Finance');
```

### 2. `StockPrices` Table:

```sql
CREATE TABLE StockPrices (
    StockId INT PRIMARY KEY,
    CompanyId INT,
    StockDate DATE,
    StockPrice DECIMAL(10, 2),
    TradingVolume INT
);

INSERT INTO StockPrices VALUES
(1, 6, '2023-07-01', 100.50, 100000),
(2, 8, '2023-08-21', 50.75, 75000),
(1, 9, '2023-01-01', 100.50, 10000),
(7, 8, '2023-06-01', 50.75, 5000),
(1, 4, '2023-09-01', 100.50, 10000),
(2, 7, '2023-01-11', 50.75, 76000);
```

## Data Retrieval Queries

### 1. Retrieve all stock prices for a specific company:

```sql
SELECT * FROM StockPrices WHERE CompanyId = 1;
```

### 2. Retrieve the highest and lowest stock prices for a given date range:

```sql
SELECT MAX(StockPrice) AS HighestPrice, MIN(StockPrice) AS LowestPrice
FROM StockPrices
WHERE StockDate BETWEEN '2023-01-01' AND '2023-01-31';
```

### 3. List all companies in a specific sector:

```sql
SELECT * FROM Companies WHERE Sector = 'Technology';
```

### 4. Display the closing prices of a specific stock for the last 30 days:

```sql
SELECT StockDate, StockPrice
FROM StockPrices
WHERE CompanyId = 1
ORDER BY StockDate DESC
LIMIT 30;
```

### 5. Retrieve the top 10 performing stocks based on percentage change:

```sql
SELECT CompanyId, MAX(StockPrice) AS MaxPrice, MIN(StockPrice) AS MinPrice,
    ((MAX(StockPrice) - MIN(StockPrice)) / MIN(StockPrice)) * 100 AS PercentageChange
FROM StockPrices
GROUP BY CompanyId
ORDER BY PercentageChange DESC
LIMIT 10;
```

### 6. Get the total trading volume for a specific stock:

```sql
SELECT CompanyId, SUM(TradingVolume) AS TotalVolume
FROM StockPrices
WHERE CompanyId = 1
GROUP BY CompanyId;
```

### 7. List the stocks that have experienced a significant price increase in the last week:

```sql
SELECT CompanyId, MAX(StockPrice) AS CurrentPrice, MIN(StockPrice) AS LastWeekPrice,
    ((MAX(StockPrice) - MIN(StockPrice)) / MIN(StockPrice)) * 100 AS PercentageChange
FROM StockPrices
WHERE StockDate BETWEEN '2023-11-01' AND '2023-11-07'
GROUP BY CompanyId
HAVING PercentageChange > 5;
```

### 8. Display the average stock price for each month in the last year:

```sql
SELECT MONTH(StockDate) AS Month, AVG(StockPrice) AS AveragePrice
FROM StockPrices
WHERE StockDate BETWEEN '2022-11-01' AND '2023-11-01'
GROUP BY MONTH(StockDate)
ORDER BY Month;
```

### 9. Retrieve the stocks with the highest trading volume for a specific date:

```sql
SELECT CompanyId, StockPrice, TradingVolume
FROM StockPrices
WHERE StockDate = '2023-01-01'
ORDER BY TradingVolume DESC
LIMIT 5;
```

### 10. List the top 5 stocks by market capitalization:

```sql
SELECT Companies.CompanyId, CompanyName, MAX(StockPrice) AS MaxPrice, TradingVolume
FROM StockPrices
JOIN Companies ON StockPrices.CompanyId = Companies.CompanyId
GROUP BY Companies.CompanyId
ORDER BY MaxPrice * TradingVolume DESC
LIMIT 5;
```
