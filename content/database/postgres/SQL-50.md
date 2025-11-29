tags: [[postgres]], [[backend/java/15-lambdas-and-streams]]


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


