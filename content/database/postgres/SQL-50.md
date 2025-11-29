tags: [[postgresql]], [[backend/java/15-lambdas-and-streams]]

**Write a solution to find the ids of products that are both low fat and recyclable.**

products table:

| product_id | low_fats | recyclable |
|------------|----------|------------|
| 0          | Y        | N          |
| 1          | Y        | Y          |
| 2          | N        | Y          |
| 3          | Y        | Y          |
| 4          | N        | N          |

```postgresql
SELECT product_id FROM products
WHERE low_fats = 'Y' AND recyclable = 'Y';
```

```java
products.stream()
	.filter(p -> p.low_fats == 'Y' && p.recyclable == 'Y')
	.map(p -> p.product_id)
	.collect(Collectors.toList());
```

| product_id |
|------------|
| 1          |
| 3          |

---

