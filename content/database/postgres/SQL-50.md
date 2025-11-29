tags: [[postgresql]], [[backend/java/15-lambdas-and-streams]]

**Write a solution to find the ids of products that are both low fat and recyclable.**

<div style="display:flex; gap:40px;">
  <div>
    <h5>Input: products table</h5>
    <table>
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
    <h5>Output:</h5>
    <table>
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

