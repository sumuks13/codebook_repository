---
{"publish":true,"title":"Backtracking","cssclasses":""}
---


**What is Backtracking?**

Backtracking is a recursive, trial‑and‑error technique where you build a solution step by step, abandon it as soon as it breaks constraints, and go back (“backtrack”) to try other choices.

**When is it used?**

- Combinatorial generation: subsets, permutations, combinations, power set, palindromic partitions.​
- Constraint satisfaction and puzzles: N‑Queens, Sudoku, crosswords, cryptarithmetic, graph coloring, Hamiltonian paths/cycles.​
- Search in state spaces with choices: maze/route problems, word search in grids, path with obstacles or constraints.

**How to know a problem needs backtracking?**

- You must “generate all / count all / find one valid configuration” under explicit constraints (e.g., “all permutations where”, “all ways to place”)
- The solution can be constructed decision by decision (place item, choose number, pick next letter), and at each step you can check if constraints are still satisfiable.​
- Brute force is clearly exponential, but the problem allows early rejection of many partial attempts (e.g., a queen attacking another, Sudoku rule broken, sum exceeded).

**Algorithm**

1. If the current partial state is a complete valid solution, process/store it and return.​
2. Otherwise, iterate over all possible next choices from this state.​
3. For each choice:
    - If this choice violates constraints, skip it (prune).​
    - Apply the choice (modify state).
    - Recurse to continue building the solution.
    - Undo the choice (restore state) before trying the next choice.


```java
import java.util.*;

public class Backtracking{
    
    List<List<Integer>> result = new ArrayList<>();

    // Generate all subsets of nums[]
    public static List<List<Integer>> subsets(int[] nums) {
        backtrackSubsets(nums, 0, new ArrayList<>());
        return result;
    }

    private static void backtrackSubsets(int[] nums, int index, List<Integer> path) {
        // Every prefix is a valid subset
        result.add(new ArrayList<>(path));

        for (int i = index; i < nums.length; i++) {
            path.add(nums[i]);                     // choose
            backtrackSubsets(nums, i + 1, path);   // explore
            path.remove(path.size() - 1);          // backtrack
        }
    }
```
