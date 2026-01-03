---
{"publish":true,"title":"Memoization: Top-Down Dynamic Programming","cssclasses":""}
---

**What is Memoization (Top-Down Dynamic Programming)?**

Memoization is an optimization technique that speeds up recursive algorithms by storing the results of expensive function calls and returning the cached result when the same inputs occur again. It starts with the final goal and breaks it down into subproblems only as needed.

**When is it used?**

- **Overlapping Subproblems:** When the recursive tree contains the same function calls with the same parameters multiple times (e.g., calculating $Fib(5)$ multiple times while finding $Fib(10)$).
- **Optimal Substructure:** When the optimal solution to a problem can be constructed from the optimal solutions of its subproblems.
- **Recursive Intuition:** When the problem is more naturally expressed as a recurrence relation (like DFS or tree-based problems).

**How to know a problem needs memoization?**

- The naive recursive solution has exponential time complexity due to redundant calculations.
- The state can be easily represented by a few variables (e.g., an index `i`, a remaining `capacity`, or a string `index`).
- You want a solution that is often easier to write and reason about than iterative loops.

**Algorithm**

1. Check if the current state (input parameters) already exists in the cache (Map or Array).
2. If it exists, return the cached value immediately.
3. If not, calculate the result using the standard recursive recurrence.
4. Store the result in the cache before returning it.

```java
import java.util.*;

public class Memoization {
    private Map<Integer, Integer> memo = new HashMap<>();

    public int tribonacci(int n) {
        // Base cases
        if (n == 0) return 0;
        if (n == 1 || n == 2) return 1;

        // 1. Check if result is already in memo
        if (memo.containsKey(n)) {
            return memo.get(n);
        }

        // 2. Recursive step + 3. Store in memo
        int result = tribonacci(n - 1) + tribonacci(n - 2) + tribonacci(n - 3);
        memo.put(n, result);

        return result;
    }
}
```
