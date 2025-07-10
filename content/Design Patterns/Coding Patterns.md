### **Two Pointers**

**Abstract Idea:** 
- Use two pointers (or indices) to traverse data **from one or both ends**, often to find a condition involving pairs or subsequences.

**Pattern:**
- You’re comparing elements from **two places** (beginning & end, or two sequences).
- You’re told to **find pairs** or triplets.
- Problem involves sorted data or **linked list**.
- Problem involves reversing a given string or list or array.
- Palindrome checks

**Problems:**
- [1768. Merge Strings Alternately](https://leetcode.com/problems/merge-strings-alternately/)
- [345. Reverse Vowels of a String](https://leetcode.com/problems/reverse-vowels-of-a-string/)

---
### **String: Unequal length handling between two strings**

**Abstract Idea:**
- While looping through two strings, one string runs out before the other — the solution must gracefully handle **incomplete data** or **imbalanced structure**.

**Pattern:**
- You’re merging, comparing or aligning multiple structures of **different sizes**.
- One side might finish early or the strings “may not be the same length”.

**Problems:**
- [1768. Merge Strings Alternately](https://leetcode.com/problems/merge-strings-alternately/)

---
### **String: Repeating pattern detection between two strings**

**Abstract Idea:**
- This pattern checks if two strings are built from the **same repeating base pattern**.
- If you concatenate them in different orders and get different results, it means **they're not structurally compatible**.

**Pattern:**
- By doing `(str1 + str2).equals(str2 + str1)`, you're testing whether **repetition order doesn't matter** - a sign they come from the same root.
- If they **don’t match**, the strings cannot be evenly divided into the same repeating unit, so you return empty (`""`) or exit early.

**Problems:**
- [1071. Greatest Common Divisor of Strings](https://leetcode.com/problems/greatest-common-divisor-of-strings/)

---
