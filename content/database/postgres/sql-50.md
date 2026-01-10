---
{"publish":true,"title":"SQL 50","cssclasses":""}
---

tags: [postgres](database/postgres/), [[backend/java/04-functional-programming/02-lambdas-and-streams]]

[1757. Recyclable and Low Fat Products](https://leetcode.com/problems/recyclable-and-low-fat-products/)
**Write a solution to find the ids of products that are both low fat and recyclable.**

<div style="display:flex; gap:40px;">
  <div>
    <table>
      <tr>
        <th colspan="3">Input: products table</th>
      </tr>
      <tr>
        <th>product_id</th>
        <th>low_fats</th>
        <th>recyclable</th>
      </tr>
      <tr><td>0</td><td>Y</td><td>N</td></tr>
      <tr><td>1</td><td>Y</td><td>Y</td></tr>
      <tr><td>2</td><td>N</td><td>Y</td></tr>
      <tr><td>3</td><td>Y</td><td>Y</td></tr>
      <tr><td>4</td><td>N</td><td>N</td></tr>
    </table>
  </div>
  <div>
    <table>
      <tr>
        <th>Output</th>
      </tr>
      <tr><th>product_id</th></tr>
      <tr><td>1</td></tr>
      <tr><td>3</td></tr>
    </table>
  </div>
</div>

```sql
SELECT product_id FROM products
WHERE low_fats = 'Y' AND recyclable = 'Y';
```

```java
products.stream()
	.filter(p -> p.low_fats == 'Y' && p.recyclable == 'Y')
	.map(p -> p.product_id)
	.collect(Collectors.toList());
```

---

[584. Find Customer Referee](https://leetcode.com/problems/find-customer-referee/)
**Find the names of the customer that are either:**
1. **referred by** any customer with `id != 2`.
2. **not referred by** any customer.

<div style="display:flex; gap:40px;">
  <div>
    <table>
      <tr>
        <th colspan="3">Input: customer table</th>
      </tr>
      <tr>
        <th>id</th>
        <th>name</th>
        <th>referee_id</th>
      </tr>
      <tr><td>1</td><td>Will</td><td>null</td></tr>
      <tr><td>2</td><td>Jane</td><td>null</td></tr>
      <tr><td>3</td><td>Alex</td><td>2</td></tr>
      <tr><td>4</td><td>Bill</td><td>null</td></tr>
      <tr><td>5</td><td>Zack</td><td>1</td></tr>
      <tr><td>6</td><td>Mark</td><td>2</td></tr>
    </table>
  </div>
  <div>
    <table>
      <tr>
        <th>Output</th>
      </tr>
      <tr><th>name</th></tr>
      <tr><td>Will</td></tr>
      <tr><td>Jane</td></tr>
      <tr><td>Bill</td></tr>
      <tr><td>Zack</td></tr>
    </table>
  </div>
</div>

```sql
SELECT name FROM customer
WHERE referee_id != 2 OR referee_id IS NULL;
```

```java
customer.stream()
	.filter(c -> c.referee_id == null || c.referee_id != 2)
	.map(c -> c.name)
	.collect(Collectors.toList());
```

---

**[595. Big Countries](https://leetcode.com/problems/big-countries/)**
Write a solution to find the name, population, and area of the **big countries**.
A country is **big** if:
- it has an area of at least three million (i.e., `3000000 km2`), or
- it has a population of at least twenty-five million (i.e., `25000000`).

<div style="display:flex; gap:40px;">
  <div>
    <table>
      <tr>
        <th colspan="5">Input: World table</th>
      </tr>
      <tr>
        <th>name</th>
        <th>continent</th>
        <th>area</th>
        <th>population</th>
        <th>gdp</th>
      </tr>
      <tr><td>Afghanistan</td><td>Asia</td><td>652230</td><td>25500100</td><td>20343000000</td></tr>
      <tr><td>Albania</td><td>Europe</td><td>28748</td><td>2831741</td><td>12960000000</td></tr>
      <tr><td>Algeria</td><td>Africa</td><td>2381741</td><td>37100000</td><td>188681000000</td></tr>
      <tr><td>Andorra</td><td>Europe</td><td>468</td><td>78115</td><td>3712000000</td></tr>
      <tr><td>Angola</td><td>Africa</td><td>1246700</td><td>20609294</td><td>100990000000</td></tr>
    </table>
  </div>
  <div>
    <table>
      <tr>
        <th colspan = "3">Output</th>
      </tr>
      <tr>
        <th>name</th>
        <th>population</th>
        <th>area</th>
      </tr>
      <tr><td>Afghanistan</td><td>25500100</td><td>652230</td></tr>
      <tr><td>Algeria</td><td>37100000</td><td>2381741</td></tr>
    </table>
  </div>
</div>

```sql
SELECT name, population, area FROM world
WHERE area >= 3000000 OR population >= 25000000;
```

```java
record WorldInfo(String name, long population, long area) {}

world.stream()
	.filter(w -> w.area >= 3000000 || w.population >= 25000000)
	.map(w -> new WorldInfo(w.name, w.population, w.area))
	.collect(Collectors.toList());
```

---

[1148. Article Views I](https://leetcode.com/problems/article-views-i/)
Write a solution to find all the authors that viewed at least one of their own articles. Return the result table sorted by `id` in ascending order.

<div style="display:flex; gap:40px;">
  <div>
    <table>
      <tr>
        <th colspan="4">Input: Views table</th>
      </tr>
      <tr>
        <th>article_id</th>
        <th>author_id</th>
        <th>viewer_id</th>
        <th>view_date</th>
      </tr>
      <tr><td>1</td><td>3</td><td>5</td><td>2019-08-01</td></tr>
      <tr><td>1</td><td>3</td><td>6</td><td>2019-08-02</td></tr>
      <tr><td>2</td><td>7</td><td>7</td><td>2019-08-01</td></tr>
      <tr><td>2</td><td>7</td><td>6</td><td>2019-08-02</td></tr>
      <tr><td>4</td><td>7</td><td>1</td><td>2019-07-22</td></tr>
      <tr><td>3</td><td>4</td><td>4</td><td>2019-07-21</td></tr>
      <tr><td>3</td><td>4</td><td>4</td><td>2019-07-21</td></tr>
    </table>
  </div>
  <div>
    <table>
      <tr>
        <th>Output</th>
      </tr>
      <tr><th>id</th></tr>
      <tr><td>4</td></tr>
      <tr><td>7</td></tr>
    </table>
  </div>
</div>

```sql
SELECT author_id as id FROM views
WHERE author_id = viewer_id
GROUP BY author_id
ORDER BY author_id

SELECT distinct author_id as id FROM views
WHERE aauthor_id = viewer_id
ORDER BY author_id
```

```java
views.stream()
	.filter(v -> v.author_id == v.viewer_id)
	.collect(Collectors.groupingBy(v -> v.author_id))
	.keySet()
	.stream()
	.sorted()
	.collect(Collectors.toList());
	
views.stream()
	.filter(v -> v.author_id == v.viewer_id)
	.map(v -> v.author_id)
	.distinct()
	.sorted()
	.collect(Collectors.toList());
```

---

[1683. Invalid Tweets](https://leetcode.com/problems/invalid-tweets/)
Write a solution to find the IDs of the invalid tweets. The tweet is invalid if the number of characters used in the content of the tweet is **strictly greater** than `15`.

<div style="display:flex; gap:40px;">
  <div>
    <table>
      <tr>
        <th colspan="2">Input: Tweets table</th>
      </tr>
      <tr>
        <th>tweet_id</th>
        <th>content</th>
      </tr>
      <tr><td>1</td><td>Let us Code</td></tr>
      <tr><td>2</td><td>More than fifteen chars are here!</td></tr>
    </table>
  </div>
  <div>
    <table>
      <tr>
        <th>Output</th>
      </tr>
      <tr><th>tweet_id</th></tr>
      <tr><td>2</td></tr>
    </table>
  </div>
</div>

```sql
SELECT tweet_id FROM tweets
WHERE LENGTH(content) > 15;
```

```java
tweets.stream()
	.filter(t -> t.length() > 15)
	.map(t -> t.tweet_id)
	.collect(Collectors.toList());
```

---

[1378. Replace Employee ID With The Unique Identifier](https://leetcode.com/problems/replace-employee-id-with-the-unique-identifier/)
Write a solution to show the **unique ID** of each user, If a user does not have a unique ID replace just show `null`.

<div style="display:flex; gap:40px;">
  <div>
    <table>
      <tr>
        <th colspan="2">Input: Employees table</th>
      </tr>
      <tr>
        <th>id</th>
        <th>name</th>
      </tr>
      <tr><td>1</td><td>Alice</td></tr>
      <tr><td>7</td><td>Bob</td></tr>
      <tr><td>11</td><td>Meir</td></tr>
      <tr><td>90</td><td>Winston</td></tr>
      <tr><td>3</td><td>Jonathan</td></tr>
    </table>
  </div>
  <div>
    <table>
      <tr>
        <th colspan="2">Input: EmployeeUNI table</th>
      </tr>
      <tr>
        <th>id</th>
        <th>unique_id</th>
      </tr>
      <tr><td>3</td><td>1</td></tr>
      <tr><td>11</td><td>2</td></tr>
      <tr><td>90</td><td>3</td></tr>
    </table>
  </div>
  <div>
    <table>
      <tr>
        <th colspan="2">Output</th>
      </tr>
      <tr>
        <th>unique_id</th>
        <th>name</th>
      </tr>
      <tr><td>null</td><td>Alice</td></tr>
      <tr><td>null</td><td>Bob</td></tr>
      <tr><td>2</td><td>Meir</td></tr>
      <tr><td>3</td><td>Winston</td></tr>
      <tr><td>1</td><td>Jonathan</td></tr>
    </table>
  </div>
</div>

```sql
SELECT unique_id, name FROM employees e
LEFT JOIN employeeuni eu ON e.id = eu.id;
```

```java
record EmployeeJoin(Integer uniqueId, String name) {}

Map<Integer, Integer> euMap = employeeUni.stream()
    .collect(Collectors.toMap(eu -> eu.id, eu.unique_id);

List<EmployeeJoin> result = employees.stream()
    .map(e -> new EmployeeJoin(euMap.get(e.id), e.name)
    .toList();
```

---

[1068. Product Sales Analysis I](https://leetcode.com/problems/product-sales-analysis-i/)
Write a solution to report the `product_name`, `year`, and `price` for each `sale_id` in the `Sales` table.

<div style="display:flex; gap:40px;">
  <div>
    <table>
      <tr>
        <th colspan="5">Input: Sales table</th>
      </tr>
      <tr>
        <th>sale_id</th>
        <th>product_id</th>
        <th>year</th>
        <th>quantity</th>
        <th>price</th>
      </tr>
      <tr><td>1</td><td>100</td><td>2008</td><td>10</td><td>5000</td></tr>
      <tr><td>2</td><td>100</td><td>2009</td><td>12</td><td>5000</td></tr>
      <tr><td>7</td><td>200</td><td>2011</td><td>15</td><td>9000</td></tr>
    </table>
  </div>
  <div>
    <table>
      <tr>
        <th colspan="2">Input: Product table</th>
      </tr>
      <tr>
        <th>product_id</th>
        <th>product_name</th>
      </tr>
      <tr><td>100</td><td>Nokia</td></tr>
      <tr><td>200</td><td>Apple</td></tr>
      <tr><td>300</td><td>Samsung</td></tr>
    </table>
  </div>
  <div>
    <table>
      <tr>
        <th colspan="3">Output</th>
      </tr>
      <tr>
        <th>product_name</th>
        <th>year</th>
        <th>price</th>
      </tr>
      <tr><td>Nokia</td><td>2008</td><td>5000</td></tr>
      <tr><td>Nokia</td><td>2009</td><td>5000</td></tr>
      <tr><td>Apple</td><td>2011</td><td>9000</td></tr>
    </table>
  </div>
</div>

```sql
SELECT product_name, year, price FROM sales s
JOIN product p ON p.product_id = s.product_id
```

---

[1581. Customer Who Visited but Did Not Make Any Transactions](https://leetcode.com/problems/customer-who-visited-but-did-not-make-any-transactions/)
Write a solution to find the IDs of the users who visited without making any transactions and the number of times they made these types of visits.

<div style="display:flex; gap:40px;">
  <div>
    <table>
      <tr>
        <th colspan="2">Input: Visits table</th>
      </tr>
      <tr>
        <th>visit_id</th>
        <th>customer_id</th>
      </tr>
      <tr><td>1</td><td>23</td></tr>
      <tr><td>2</td><td>9</td></tr>
      <tr><td>4</td><td>30</td></tr>
      <tr><td>5</td><td>54</td></tr>
      <tr><td>6</td><td>96</td></tr>
      <tr><td>7</td><td>54</td></tr>
      <tr><td>8</td><td>54</td></tr>
    </table>
  </div>
  <div>
    <table>
      <tr>
        <th colspan="3">Input: Transactions table</th>
      </tr>
      <tr>
        <th>transaction_id</th>
        <th>visit_id</th>
        <th>amount</th>
      </tr>
      <tr><td>2</td><td>5</td><td>310</td></tr>
      <tr><td>3</td><td>5</td><td>300</td></tr>
      <tr><td>9</td><td>5</td><td>200</td></tr>
      <tr><td>12</td><td>1</td><td>910</td></tr>
      <tr><td>13</td><td>2</td><td>970</td></tr>
    </table>
  </div>
  <div>
    <table>
      <tr>
        <th colspan="2">Output</th>
      </tr>
      <tr>
        <th>customer_id</th>
        <th>count_no_trans</th>
      </tr>
      <tr><td>54</td><td>2</td></tr>
      <tr><td>30</td><td>1</td></tr>
      <tr><td>96</td><td>1</td></tr>
    </table>
  </div>
</div>

```sql
SELECT customer_id, COUNT(*) AS count_no_trans FROM visits v
WHERE NOT EXISTS(
    SELECT t.visit_id FROM transactions t
    WHERE t.visit_id = v.visit_id
)
GROUP BY customer_id;

SELECT customer_id, count(*) AS count_no_trans FROM visits v
LEFT JOIN transactions t ON v.visit_id = t.visit_id
WHERE transaction_id IS NULL
GROUP BY customer_id;
```

---

[197. Rising Temperature](https://leetcode.com/problems/rising-temperature/)
Write a solution to find all dates' `id` with higher temperatures compared to its previous dates (yesterday).

<div style="display:flex; gap:40px;">
  <div>
    <table>
      <tr>
        <th colspan="3">Input: Weather table</th>
      </tr>
      <tr>
        <th>id</th>
        <th>recordDate</th>
        <th>temperature</th>
      </tr>
      <tr><td>1</td><td>2015-01-01</td><td>10</td></tr>
      <tr><td>2</td><td>2015-01-02</td><td>25</td></tr>
      <tr><td>3</td><td>2015-01-03</td><td>20</td></tr>
      <tr><td>4</td><td>2015-01-04</td><td>30</td></tr>
    </table>
  </div>
  <div>
    <table>
      <tr>
        <th colspan="1">Output</th>
      </tr>
      <tr>
        <th>id</th>
      </tr>
      <tr><td>2</td></tr>
      <tr><td>4</td></tr>
    </table>
  </div>
</div>

```sql
SELECT w1.id FROM weather w1
JOIN weather w2 on w1.recorddate = w2.recorddate + INTERVAL '1 day'
WHERE w1.temperature > w2.temperature;

SELECT w1.id FROM weather w1
CROSS JOIN weather w2
WHERE w1.recorddate - w2.recorddate = 1
AND w1.temperature > w2.temperature
```

---

[1661. Average Time of Process per Machine](https://leetcode.com/problems/average-time-of-process-per-machine/)
Write a solution to find the average time each machine takes to complete a process. Round the answer to 3 decimal places.

<div style="display:flex; gap:40px;"> <div> <table> <tr> <th colspan="4">Input: Activity table</th> </tr> <tr> <th>machine_id</th> <th>process_id</th> <th>activity_type</th> <th>timestamp</th> </tr> <tr><td>0</td><td>0</td><td>start</td><td>0.712</td></tr> <tr><td>0</td><td>0</td><td>end</td><td>1.520</td></tr> <tr><td>0</td><td>1</td><td>start</td><td>3.140</td></tr> <tr><td>0</td><td>1</td><td>end</td><td>4.120</td></tr> <tr><td>1</td><td>0</td><td>start</td><td>0.550</td></tr> <tr><td>1</td><td>0</td><td>end</td><td>1.550</td></tr> <tr><td>1</td><td>1</td><td>start</td><td>0.430</td></tr> <tr><td>1</td><td>1</td><td>end</td><td>1.420</td></tr> </table> </div> <div> <table> <tr> <th colspan="2">Output</th> </tr> <tr><th>machine_id</th><th>processing_time</th></tr> <tr><td>0</td><td>0.894</td></tr> <tr><td>1</td><td>0.995</td></tr> </table> </div> </div>

```sql
SELECT a1.machine_id, ROUND((SUM(a2.timestamp - a1.timestamp) / COUNT(*))::numeric, 3) AS processing_time
FROM activity a1
JOIN activity a2 ON a1.activity_type = 'start' AND a2.activity_type = 'end'
AND a1.machine_id = a2.machine_id AND a1.process_id = a2.process_id
GROUP BY a1.machine_id;

SELECT a1.machine_id, ROUND(AVG(a2.timestamp - a1.timestamp)::numeric, 3) AS processing_time
FROM activity a1
JOIN activity a2 ON a1.activity_type = 'start' AND a2.activity_type = 'end'
AND a1.machine_id = a2.machine_id AND a1.process_id = a2.process_id
GROUP BY a1.machine_id;
```

---

[577. Employee Bonus](https://leetcode.com/problems/employee-bonus/)
Write a solution to report the name and bonus amount of each employee with a bonus less than 1000.

<div style="display:flex; gap:40px;"> <div> <table> <tr> <th colspan="4">Input: Employee table</th> </tr> <tr> <th>empId</th> <th>name</th> <th>supervisor</th> <th>salary</th> </tr> <tr><td>3</td><td>Brad</td><td>null</td><td>4000</td></tr> <tr><td>1</td><td>John</td><td>3</td><td>1000</td></tr> <tr><td>2</td><td>Dan</td><td>3</td><td>2000</td></tr> <tr><td>4</td><td>Thomas</td><td>3</td><td>4000</td></tr> </table> </div> <div><table> <tr> <th colspan="2">Input: Bonus table</th> </tr> <tr> <th>empId</th> <th>bonus</th> </tr> <tr><td>2</td><td>500</td></tr> <tr><td>4</td><td>2000</td></tr> </table> </div> <div> <table> <tr> <th colspan="2">Output</th> </tr> <tr><th>name</th><th>bonus</th></tr> <tr><td>Brad</td><td>null</td></tr> <tr><td>John</td><td>null</td></tr> <tr><td>Dan</td><td>500</td></tr> </table> </div> </div>

```sql
SELECT name, bonus FROM employee e
LEFT JOIN bonus b ON e.empid = b.empid
WHERE bonus IS NULL OR bonus < 1000;
```

---

[1280. Students and Examinations](https://leetcode.com/problems/students-and-examinations/)
Write a solution to find the number of times each student attended each exam. Return the result table ordered by student_id and subject_name.

<div style="display:flex; gap:40px;"> <div> <table> <tr> <th colspan="2">Input: Students table</th> </tr> <tr> <th>student_id</th> <th>student_name</th> </tr> <tr><td>1</td><td>Alice</td></tr> <tr><td>2</td><td>Bob</td></tr> <tr><td>13</td><td>John</td></tr> <tr><td>6</td><td>Alex</td></tr> </table> <br> <table> <tr> <th colspan="2">Input: Examinations table</th> </tr> <tr> <th>student_id</th> <th>subject_name</th> </tr> <tr><td>1</td><td>Math</td></tr> <tr><td>1</td><td>Physics</td></tr> <tr><td>1</td><td>Programming</td></tr> <tr><td>2</td><td>Programming</td></tr> <tr><td>1</td><td>Physics</td></tr> <tr><td>1</td><td>Math</td></tr> <tr><td>13</td><td>Math</td></tr> <tr><td>13</td><td>Programming</td></tr> <tr><td>13</td><td>Physics</td></tr> <tr><td>2</td><td>Math</td></tr> <tr><td>1</td><td>Math</td></tr> </table> </div> <div> <table> <tr> <th colspan="1">Input: Subjects table</th> </tr> <tr> <th>subject_name</th> </tr> <tr><td>Math</td></tr> <tr><td>Physics</td></tr> <tr><td>Programming</td></tr> </table> <br> <table> <tr> <th colspan="4">Output</th> </tr> <tr><th>student_id</th><th>student_name</th><th>subject_name</th><th>attended_exams</th></tr> <tr><td>1</td><td>Alice</td><td>Math</td><td>3</td></tr> <tr><td>1</td><td>Alice</td><td>Physics</td><td>2</td></tr> <tr><td>1</td><td>Alice</td><td>Programming</td><td>1</td></tr> <tr><td>2</td><td>Bob</td><td>Math</td><td>1</td></tr> <tr><td>2</td><td>Bob</td><td>Physics</td><td>0</td></tr> <tr><td>2</td><td>Bob</td><td>Programming</td><td>1</td></tr> <tr><td>6</td><td>Alex</td><td>Math</td><td>0</td></tr> <tr><td>6</td><td>Alex</td><td>Physics</td><td>0</td></tr> <tr><td>6</td><td>Alex</td><td>Programming</td><td>0</td></tr> <tr><td>13</td><td>John</td><td>Math</td><td>1</td></tr> <tr><td>13</td><td>John</td><td>Physics</td><td>1</td></tr> <tr><td>13</td><td>John</td><td>Programming</td><td>1</td></tr> </table> </div> </div>

```sql
SELECT s.student_id, student_name, sub.subject_name, count(e) AS attended_exams FROM students s 
CROSS JOIN subjects sub 
LEFT JOIN examinations e ON s.student_id = e.student_id AND sub.subject_name = e.subject_name 
GROUP BY s.student_id, student_name, sub.subject_name
ORDER BY s.student_id, sub.subject_name;
```

---

[570. Managers with at Least 5 Direct Reports](https://leetcode.com/problems/managers-with-at-least-5-direct-reports/)
Write a solution to find managers with at least five direct reports.

<div style="display:flex; gap:40px;"> <div> <table> <tr> <th colspan="4">Input: Employee table</th> </tr> <tr> <th>id</th> <th>name</th> <th>department</th> <th>managerId</th> </tr> <tr><td>101</td><td>John</td><td>A</td><td>null</td></tr> <tr><td>102</td><td>Dan</td><td>A</td><td>101</td></tr> <tr><td>103</td><td>James</td><td>A</td><td>101</td></tr> <tr><td>104</td><td>Amy</td><td>A</td><td>101</td></tr> <tr><td>105</td><td>Anne</td><td>A</td><td>101</td></tr> <tr><td>106</td><td>Ron</td><td>B</td><td>101</td></tr> </table> </div> <div> <table> <tr> <th>Output</th> </tr> <tr><th>name</th></tr> <tr><td>John</td></tr> </table> </div> </div>

```sql
SELECT m.name FROM Employee m 
JOIN Employee e ON m.id = e.managerId 
GROUP BY m.id, m.name HAVING COUNT(1) >= 5;
```

---

[1934. Confirmation Rate](https://leetcode.com/problems/confirmation-rate/)
Write a solution to find the confirmation rate of each user. Round to two decimal places. The confirmation rate is the number of 'confirmed' messages divided by the total number of requested confirmation messages.

<div style="display:flex; gap:40px;"> <div> <table> <tr> <th colspan="3">Input: Signups table</th> </tr> <tr> <th>user_id</th> <th>time_stamp</th> </tr> <tr><td>3</td><td>2020-03-21 10:16:13</td></tr> <tr><td>7</td><td>2020-01-04 13:57:59</td></tr> <tr><td>2</td><td>2020-07-29 23:09:44</td></tr> <tr><td>6</td><td>2020-12-09 10:39:37</td></tr> </table> </div> <div> <table> <tr> <th colspan="3">Input: Confirmations table</th> </tr> <tr> <th>user_id</th> <th>time_stamp</th> <th>action</th> </tr> <tr><td>3</td><td>2021-01-06 03:30:46</td><td>timeout</td></tr> <tr><td>3</td><td>2021-07-14 14:00:00</td><td>timeout</td></tr> <tr><td>7</td><td>2021-06-12 11:57:29</td><td>confirmed</td></tr> <tr><td>7</td><td>2021-06-13 12:58:28</td><td>confirmed</td></tr> <tr><td>7</td><td>2021-06-14 13:59:27</td><td>confirmed</td></tr> <tr><td>2</td><td>2021-01-22 00:00:00</td><td>confirmed</td></tr> <tr><td>2</td><td>2021-02-28 23:59:59</td><td>timeout</td></tr> </table> </div> <div> <table> <tr> <th colspan="2">Output</th> </tr> <tr><th>user_id</th><th>confirmation_rate</th></tr> <tr><td>6</td><td>0.00</td></tr> <tr><td>3</td><td>0.00</td></tr> <tr><td>7</td><td>1.00</td></tr> <tr><td>2</td><td>0.50</td></tr> </table> </div> </div>

```sql
SELECT s.user_id,
ROUND(AVG(CASE WHEN c.action = 'confirmed' THEN 1 ELSE 0 END), 2) as confirmation_rate
FROM signups s
LEFT JOIN confirmations c on s.user_id = c.user_id
GROUP BY s.user_id
```

---

[620. Not Boring Movies](https://leetcode.com/problems/not-boring-movies/)

Write a solution to report the movies with an odd-numbered ID and a description that is not "boring". Return the result table ordered by rating in descending order.

<div style="display:flex; gap:40px;"> <div> <table> <tr> <th colspan="4">Input: Cinema table</th> </tr> <tr> <th>id</th> <th>movie</th> <th>description</th> <th>rating</th> </tr> <tr><td>1</td><td>War</td><td>great 3D</td><td>8.9</td></tr> <tr><td>2</td><td>Science</td><td>fiction</td><td>8.5</td></tr> <tr><td>3</td><td>irish</td><td>boring</td><td>6.2</td></tr> <tr><td>4</td><td>Ice song</td><td>Fantacy</td><td>8.6</td></tr> <tr><td>5</td><td>House card</td><td>Interesting</td><td>9.1</td></tr> </table> </div> <div> <table> <tr> <th colspan="4">Output</th> </tr> <tr><th>id</th><th>movie</th><th>description</th><th>rating</th></tr> <tr><td>5</td><td>House card</td><td>Interesting</td><td>9.1</td></tr> <tr><td>1</td><td>War</td><td>great 3D</td><td>8.9</td></tr> </table> </div> </div>

```sql
SELECT * FROM cinema
WHERE description <> 'boring' AND id % 2 = 1
ORDER BY id DESC;
```

---

[1251. Average Selling Price](https://leetcode.com/problems/average-selling-price/)

Write a solution to find the average selling price for each product. Round to 2 decimal places. If a product does not have any sold units, its average price is 0.

<div style="display:flex; gap:40px;"> <div> <table> <tr> <th colspan="4">Input: Prices table</th> </tr> <tr> <th>product_id</th> <th>start_date</th> <th>end_date</th> <th>price</th> </tr> <tr><td>1</td><td>2019-02-17</td><td>2019-02-28</td><td>5</td></tr> <tr><td>1</td><td>2019-03-01</td><td>2019-03-22</td><td>20</td></tr> <tr><td>2</td><td>2019-02-01</td><td>2019-02-20</td><td>15</td></tr> <tr><td>2</td><td>2019-02-21</td><td>2019-03-31</td><td>30</td></tr> </table> </div> <div><table> <tr> <th colspan="3">Input: UnitsSold table</th> </tr> <tr> <th>product_id</th> <th>purchase_date</th> <th>units</th> </tr> <tr><td>1</td><td>2019-02-25</td><td>100</td></tr> <tr><td>1</td><td>2019-03-01</td><td>15</td></tr> <tr><td>2</td><td>2019-02-10</td><td>200</td></tr> <tr><td>2</td><td>2019-03-22</td><td>30</td></tr> </table> </div> <div> <table> <tr> <th colspan="2">Output</th> </tr> <tr><th>product_id</th><th>average_price</th></tr> <tr><td>1</td><td>6.96</td></tr> <tr><td>2</td><td>16.96</td></tr> </table> </div> </div>

```sql
SELECT p.product_id, COALESCE(ROUND(SUM(units * price)/SUM(units)::numeric, 2), 0) AS average_price
FROM prices p
LEFT JOIN unitssold u ON p.product_id = u.product_id
AND u.purchase_date BETWEEN p.start_date AND p.end_date
GROUP BY p.product_id;
```

---

[1075. Project Employees I](https://leetcode.com/problems/project-employees-i/)

Write a solution to report the average experience years of all the employees for each project, rounded to 2 digits.

<div style="display:flex; gap:40px;"> <div> <table> <tr> <th colspan="2">Input: Project table</th> </tr> <tr> <th>project_id</th> <th>employee_id</th> </tr> <tr><td>1</td><td>1</td></tr> <tr><td>1</td><td>2</td></tr> <tr><td>1</td><td>3</td></tr> <tr><td>2</td><td>1</td></tr> <tr><td>2</td><td>4</td></tr> </table> </div><div> <table> <tr> <th colspan="2">Employee table</th> </tr> <tr> <th>employee_id</th> <th>name</th> <th>experience_years</th> </tr> <tr><td>1</td><td>Khaled</td><td>3</td></tr> <tr><td>2</td><td>Ali</td><td>2</td></tr> <tr><td>3</td><td>John</td><td>1</td></tr> <tr><td>4</td><td>Doe</td><td>2</td></tr> </table> </div> <div> <table> <tr> <th colspan="2">Output</th> </tr> <tr><th>project_id</th><th>average_years</th></tr> <tr><td>1</td><td>2.00</td></tr> <tr><td>2</td><td>2.50</td></tr> </table> </div> </div>

```sql
SELECT p.project_id, ROUND(AVG(e.experience_years), 2) AS average_years 
FROM Project p 
JOIN Employee e ON p.employee_id = e.employee_id 
GROUP BY p.project_id;
```

---

[1633. Percentage of Users Attended a Contest](https://leetcode.com/problems/percentage-of-users-attended-a-contest/)

Write a solution to find the percentage of the users registered in each contest rounded to two decimals. Return the result table ordered by percentage in descending order. In case of a tie, order by contest_id in ascending order.

<div style="display:flex; gap:40px;"> <div> <table> <tr> <th colspan="2">Input: Users table</th> </tr> <tr> <th>user_id</th> <th>user_name</th> </tr> <tr><td>6</td><td>Alice</td></tr> <tr><td>2</td><td>Bob</td></tr> <tr><td>7</td><td>Alex</td></tr> </table></div><div> <table> <tr> <th colspan="2">Register table</th> </tr> <tr> <th>contest_id</th> <th>user_id</th> </tr> <tr><td>215</td><td>6</td></tr> <tr><td>209</td><td>2</td></tr> <tr><td>208</td><td>2</td></tr> <tr><td>210</td><td>6</td></tr> <tr><td>208</td><td>6</td></tr> <tr><td>209</td><td>7</td></tr> <tr><td>209</td><td>6</td></tr> <tr><td>215</td><td>7</td></tr> <tr><td>208</td><td>7</td></tr> <tr><td>210</td><td>2</td></tr> <tr><td>207</td><td>2</td></tr> <tr><td>210</td><td>7</td></tr> </table> </div> <div> <table> <tr> <th colspan="2">Output</th> </tr> <tr><th>contest_id</th><th>percentage</th></tr> <tr><td>208</td><td>100.00</td></tr> <tr><td>209</td><td>100.00</td></tr> <tr><td>210</td><td>100.00</td></tr> <tr><td>215</td><td>66.67</td></tr> <tr><td>207</td><td>33.33</td></tr> </table> </div> </div>

```sql
SELECT r.contest_id, 
ROUND(COUNT(DISTINCT r.user_id)::numeric * 100 / (SELECT COUNT(*) FROM Users), 2) AS percentage
FROM Register r 
GROUP BY r.contest_id 
ORDER BY percentage DESC, r.contest_id;
```

```sql
WITH total AS (SELECT count(1) AS c from users)

SELECT contest_id,
ROUND(count(distinct user_id)::numeric * 100 / total.c, 2) as percentage
FROM register
CROSS JOIN total
GROUP BY contest_id, c
ORDER BY percentage desc, contest_id;
```

---

[1211. Queries Quality and Percentage](https://leetcode.com/problems/queries-quality-and-percentage/)

Write a solution to find each query_name, the quality and poor_query_percentage. Both quality and poor_query_percentage should be rounded to 2 decimal places. Quality is the average of the ratio between query rating and position. A query is poor if its rating is less than 3.

<div style="display:flex; gap:40px;"> <div> <table> <tr> <th colspan="4">Input: Queries table</th> </tr> <tr> <th>query_name</th> <th>result</th> <th>position</th> <th>rating</th> </tr> <tr><td>Dog</td><td>Golden Retriever</td><td>1</td><td>5</td></tr> <tr><td>Dog</td><td>German Shepherd</td><td>2</td><td>5</td></tr> <tr><td>Dog</td><td>Mule</td><td>200</td><td>1</td></tr> <tr><td>Cat</td><td>Shirazi</td><td>5</td><td>2</td></tr> <tr><td>Cat</td><td>Siamese</td><td>3</td><td>3</td></tr> <tr><td>Cat</td><td>Sphynx</td><td>7</td><td>4</td></tr> </table> </div> <div> <table> <tr> <th colspan="3">Output</th> </tr> <tr><th>query_name</th><th>quality</th><th>poor_query_percentage</th></tr> <tr><td>Dog</td><td>2.50</td><td>33.33</td></tr> <tr><td>Cat</td><td>0.66</td><td>33.33</td></tr> </table> </div> </div>

```sql
SELECT query_name, ROUND(AVG(rating::numeric / position), 2) AS quality,
ROUND(AVG(CASE WHEN rating < 3 THEN 1 ELSE 0 END)::numeric * 100, 2) AS poor_query_percentage
FROM queries
GROUP BY query_name;
```

---

## 1193. Monthly Transactions I

[1193. Monthly Transactions I](https://leetcode.com/problems/monthly-transactions-i/)

Write a solution to find for each month and country, the number of transactions and their total amount, the number of approved transactions and their total amount.

<div style="display:flex; gap:40px;"> <div> <table> <tr> <th colspan="5">Input: Transactions table</th> </tr> <tr> <th>id</th> <th>country</th> <th>state</th> <th>amount</th> <th>trans_date</th> </tr> <tr><td>121</td><td>US</td><td>approved</td><td>1000</td><td>2018-12-18</td></tr> <tr><td>122</td><td>US</td><td>declined</td><td>2000</td><td>2018-12-19</td></tr> <tr><td>123</td><td>US</td><td>approved</td><td>2000</td><td>2019-01-01</td></tr> <tr><td>124</td><td>DE</td><td>approved</td><td>2000</td><td>2019-01-07</td></tr> </table> </div> <div> <table> <tr> <th colspan="6">Output</th> </tr> <tr><th>month</th><th>country</th><th>trans_count</th><th>approved_count</th><th>trans_total_amount</th><th>approved_total_amount</th></tr> <tr><td>2018-12</td><td>US</td><td>2</td><td>1</td><td>3000</td><td>1000</td></tr> <tr><td>2019-01</td><td>US</td><td>1</td><td>1</td><td>2000</td><td>2000</td></tr> <tr><td>2019-01</td><td>DE</td><td>1</td><td>1</td><td>2000</td><td>2000</td></tr> </table> </div> </div>

```sql
SELECT TO_CHAR(trans_date, 'YYYY-MM') AS month, country, count(*) AS trans_count,
SUM(CASE WHEN state = 'approved' THEN 1 ELSE 0 END) AS approved_count,
SUM(amount) AS trans_total_amount,
SUM(CASE WHEN state = 'approved' THEN amount ELSE 0 END) AS approved_total_amount
FROM transactions
GROUP BY month, country;
```

---

## 1174. Immediate Food Delivery II

[1174. Immediate Food Delivery II](https://leetcode.com/problems/immediate-food-delivery-ii/)

Write a solution to find the percentage of immediate orders in the first orders of all customers. Round to 2 decimal places. An order is immediate if the order date equals the customer preferred delivery date.

<div style="display:flex; gap:40px;"> <div> <table> <tr> <th colspan="4">Input: Delivery table</th> </tr> <tr> <th>delivery_id</th> <th>customer_id</th> <th>order_date</th> <th>customer_pref_delivery_date</th> </tr> <tr><td>1</td><td>1</td><td>2019-08-01</td><td>2019-08-02</td></tr> <tr><td>2</td><td>2</td><td>2019-08-02</td><td>2019-08-02</td></tr> <tr><td>3</td><td>1</td><td>2019-08-11</td><td>2019-08-12</td></tr> <tr><td>4</td><td>3</td><td>2019-08-24</td><td>2019-08-24</td></tr> <tr><td>5</td><td>3</td><td>2019-08-21</td><td>2019-08-22</td></tr> <tr><td>6</td><td>2</td><td>2019-08-11</td><td>2019-08-13</td></tr> <tr><td>7</td><td>4</td><td>2019-08-09</td><td>2019-08-09</td></tr> </table> </div> <div> <table> <tr> <th>Output</th> </tr> <tr><th>immediate_percentage</th></tr> <tr><td>50.00</td></tr> </table> </div> </div>

```sql
SELECT ROUND(AVG(CASE WHEN order_date = customer_pref_delivery_date THEN 1 ELSE 0 END) * 100, 2) AS immediate_percentage
FROM delivery
WHERE (customer_id, order_date) IN (
    SELECT customer_id, MIN(order_date) AS first_order FROM delivery
    GROUP BY customer_id
)
```

```sql
WITH first_orders AS (
    SELECT customer_id, MIN(order_date) AS first_order_date FROM Delivery
    GROUP BY customer_id 
) 

SELECT ROUND(SUM(CASE WHEN d.order_date = d.customer_pref_delivery_date THEN 1 ELSE 0 END)::numeric * 100 / COUNT(*), 2) AS immediate_percentage 
FROM Delivery d 
JOIN first_orders f ON d.customer_id = f.customer_id  AND d.order_date = f.first_order_date;
```

---

## 550. Game Play Analysis IV

[550. Game Play Analysis IV](https://leetcode.com/problems/game-play-analysis-iv/)

Write a solution to report the fraction of players that logged in again on the day after the day they first logged in, rounded to 2 decimal places.

<div style="display:flex; gap:40px;"> <div> <table> <tr> <th colspan="3">Input: Activity table</th> </tr> <tr> <th>player_id</th> <th>device_id</th> <th>event_date</th> <th>games_played</th> </tr> <tr><td>1</td><td>2</td><td>2016-03-01</td><td>5</td></tr> <tr><td>1</td><td>2</td><td>2016-03-02</td><td>6</td></tr> <tr><td>2</td><td>3</td><td>2017-06-25</td><td>1</td></tr> <tr><td>3</td><td>1</td><td>2016-03-02</td><td>0</td></tr> <tr><td>3</td><td>4</td><td>2018-07-03</td><td>5</td></tr> </table> </div> <div> <table> <tr> <th>Output</th> </tr> <tr><th>fraction</th></tr> <tr><td>0.33</td></tr> </table> </div> </div>

```sql
WITH first_login AS (
    SELECT player_id, MIN(event_date) AS first_login_date FROM activity
    GROUP BY player_id
)

SELECT ROUND(COUNT(a2.player_id)::NUMERIC / COUNT(a1.player_id), 2) AS fraction
FROM first_login a1
LEFT JOIN activity a2 ON a1.player_id = a2.player_id
AND a1.first_login_date = a2.event_date - INTERVAL '1 day';
```

---

## 2356. Number of Unique Subjects Taught by Each Teacher

[2356. Number of Unique Subjects Taught by Each Teacher](https://leetcode.com/problems/number-of-unique-subjects-taught-by-each-teacher/)

Write a solution to calculate the number of unique subjects each teacher teaches in the university.

<div style="display:flex; gap:40px;"> <div> <table> <tr> <th colspan="3">Input: Teacher table</th> </tr> <tr> <th>teacher_id</th> <th>subject_id</th> <th>dept_id</th> </tr> <tr><td>1</td><td>2</td><td>3</td></tr> <tr><td>1</td><td>2</td><td>4</td></tr> <tr><td>1</td><td>3</td><td>3</td></tr> <tr><td>2</td><td>1</td><td>1</td></tr> <tr><td>2</td><td>2</td><td>1</td></tr> <tr><td>2</td><td>3</td><td>1</td></tr> <tr><td>2</td><td>4</td><td>1</td></tr> </table> </div> <div> <table> <tr> <th colspan="2">Output</th> </tr> <tr><th>teacher_id</th><th>cnt</th></tr> <tr><td>1</td><td>2</td></tr> <tr><td>2</td><td>4</td></tr> </table> </div> </div>

```sql
SELECT teacher_id, COUNT(DISTINCT subject_id) AS cnt FROM teacher 
GROUP BY teacher_id;
```

---
## 1141. User Activity for the Past 30 Days I

[1141. User Activity for the Past 30 Days I](https://leetcode.com/problems/user-activity-for-the-past-30-days-i/)

Write a solution to find the daily active user count for a period of 30 days ending 2019-07-27 inclusively. A user was active on some day if they made at least one activity on that day.

<div style="display:flex; gap:40px;"> <div> <table> <tr> <th colspan="4">Input: Activity table</th> </tr> <tr> <th>user_id</th> <th>session_id</th> <th>activity_date</th> <th>activity_type</th> </tr> <tr><td>1</td><td>1</td><td>2019-07-20</td><td>open_session</td></tr> <tr><td>1</td><td>1</td><td>2019-07-20</td><td>scroll_down</td></tr> <tr><td>1</td><td>1</td><td>2019-07-20</td><td>end_session</td></tr> <tr><td>2</td><td>4</td><td>2019-07-20</td><td>open_session</td></tr> <tr><td>2</td><td>4</td><td>2019-07-21</td><td>send_message</td></tr> <tr><td>2</td><td>4</td><td>2019-07-21</td><td>end_session</td></tr> <tr><td>3</td><td>2</td><td>2019-07-21</td><td>open_session</td></tr> <tr><td>3</td><td>2</td><td>2019-07-21</td><td>send_message</td></tr> <tr><td>3</td><td>2</td><td>2019-07-21</td><td>end_session</td></tr> <tr><td>4</td><td>3</td><td>2019-06-25</td><td>open_session</td></tr> <tr><td>4</td><td>3</td><td>2019-06-25</td><td>end_session</td></tr> </table> </div> <div> <table> <tr> <th colspan="2">Output</th> </tr> <tr><th>day</th><th>active_users</th></tr> <tr><td>2019-07-20</td><td>2</td></tr> <tr><td>2019-07-21</td><td>2</td></tr> </table> </div> </div>

```sql
SELECT activity_date AS day, count(DISTINCT user_id) AS active_users FROM activity
WHERE activity_date BETWEEN '2019-07-27'::date - INTERVAL '29 DAYS' AND '2019-07-27'
GROUP BY activity_date;
```

---

## 1070. Product Sales Analysis III

[1070. Product Sales Analysis III](https://leetcode.com/problems/product-sales-analysis-iii/)

Write a solution to select the product id, year, quantity, and price for the first year of every product sold.

<div style="display:flex; gap:40px;"> <div> <table> <tr> <th colspan="5">Input: Sales table</th> </tr> <tr> <th>sale_id</th> <th>product_id</th> <th>year</th> <th>quantity</th> <th>price</th> </tr> <tr><td>1</td><td>100</td><td>2008</td><td>10</td><td>5000</td></tr> <tr><td>2</td><td>100</td><td>2009</td><td>12</td><td>5000</td></tr> <tr><td>7</td><td>200</td><td>2011</td><td>15</td><td>9000</td></tr> </table> <table> <tr> <th colspan="2">Product table</th> </tr> <tr> <th>product_id</th> <th>product_name</th> </tr> <tr><td>100</td><td>Nokia</td></tr> <tr><td>200</td><td>Apple</td></tr> <tr><td>300</td><td>Samsung</td></tr> </table> </div> <div> <table> <tr> <th colspan="4">Output</th> </tr> <tr><th>product_id</th><th>first_year</th><th>quantity</th><th>price</th></tr> <tr><td>100</td><td>2008</td><td>10</td><td>5000</td></tr> <tr><td>200</td><td>2011</td><td>15</td><td>9000</td></tr> </table> </div> </div>

```sql
WITH first_year AS (
    SELECT product_id, MIN(year) as year FROM sales
    GROUP BY product_id
)

SELECT s.product_id, s.year AS first_year, s.quantity, s.price FROM sales s
JOIN first_year f ON s.product_id = f.product_id AND s.year = f.year;
```

---
