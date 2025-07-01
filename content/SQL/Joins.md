tags: [[SQL]]

A `JOIN` clause is used to combine rows from two or more tables, based on a related column between them.
\
**Table 1: Order**

| OrderID | ==CustomerID== | OrderName | ProductName |
| ------- | -------------- | --------- | ----------- |
| 12025   | 101            | Peter     | ABC         |
| 12030   | 105            | Robert    | XYX         |
| 12032   | 110            | James     | XYZ         |
| 12034   | 115            | Andrew    | PQR         |
| 12035   | 120            | Mathew    | AAA         |

**Table 2: Customer**

| ==CustomerID== | CustomerName    | Country   |
| -------------- | --------------- | --------- |
| 100            | Messy           | Maxico    |
| 101            | Prince          | Taiwan    |
| 103            | Maria Fernandez | Turkey    |
| 105            | Jasmine         | Paris     |
| 110            | Faf Weasel      | Indonesia |
| 120            | Romen Rocket    | Russia    |

**Inner Join:**

```sql
SELECT ord.OrderID, cust.CustomerName, cust.Country, ord.ProductName 
FROM Order ord INNER JOIN Customer cust 
ON ord.CustomerID = cust.CustomerID;
```

|OrderID|CustomerName|Country|ProductName|
|---|---|---|---|
|12025|Prince|Taiwan|ABC|
|12030|Jasmine|Paris|XYX|
|12032|Faf Weasel|Indonesia|XYZ|
|12035|Romen Rocket|Russia|AAA|

## Different Types of SQL JOINs

Here are the different types of the JOINs in SQL:

- `(INNER) JOIN`: Returns records that have matching values in both tables
- `LEFT (OUTER) JOIN`: Returns all records from the left table, and the matched records from the right table
- `RIGHT (OUTER) JOIN`: Returns all records from the right table, and the matched records from the left table
- `FULL (OUTER) JOIN`: Returns all records when there is a match in either left or right table
- `CROSS JOIN`: Returns the Cartesian product of two or more joined tables. The cross join produces a table that merges each row from the first table with each second table row. It is not required to include any condition in CROSS JOIN.
- `SELF JOIN`: Used to create a table by joining itself as there were two tables.

![[Pasted image 20241012182913.png]]


> [!NOTE] Notes
> ```sql
> SELECT p.product_name, s.year, s.price
> FROM Product p JOIN Sales s
> ON p.product_id = s.product_id
>
> --is faster than
> 
> SELECT product_name, year, price
> FROM Product JOIN Sales
> ON Product.product_id = Sales.product_id
> ```

