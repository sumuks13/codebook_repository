---
{"publish":true,"title":"Tabulation: Bottom-Up Dynamic Programming","cssclasses":""}
---

**What is Tabulation (Bottom-Up Dynamic Programming)?**

Tabulation is an iterative technique where you solve all possible subproblems first, starting from the smallest "base cases," and store their results in a table (usually an array). You then use those stored results to build up to the final solution.

**When is it used?**

- **Efficiency requirements:** When you want to avoid the memory overhead of the recursive call stack (preventing `StackOverflowError`).
- **Complete State Space:** When you know you will likely need to solve almost all subproblems anyway.
- **Space Optimization:** When the current state only depends on a few previous states, allowing you to discard old data and save memory.

**How to know a problem needs tabulation?**

- The problem requires the "maximum," "minimum," or "total number of ways" to reach a certain state.
- The dependency between states is clear and directional (e.g., $dp[i]$ only depends on $dp[i-1]$ or $dp[i-2]$).
- You need to optimize for space (e.g., moving from $O(N)$ space to $O(1)$ space).

**Algorithm**

1. Initialize a table (array or matrix) of the required size.
2. Fill in the base case values (e.g., $dp[0] = 0$).
3. Iterate through the table from the smallest subproblem to the target.
4. Use the values already present in the table to compute the current state.

```java
public class Tabulation {

    public int tribonacci(int n) {
        // Base cases
        if (n == 0) return 0;
        if (n == 1 || n == 2) return 1;

        // 1. Initialize table
        int[] dp = new int[n + 1];
        dp[0] = 0;
        dp[1] = 1;
        dp[2] = 1;

        // 2. Iterative fill
        for (int i = 3; i <= n; i++) {
            dp[i] = dp[i - 1] + dp[i - 2] + dp[i - 3];
        }

        return dp[n];
    }
}
```
