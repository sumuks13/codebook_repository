tags: [[Algorithms/Algorithms]]

- The greatest common divisor (GCD) of two or more numbers is the greatest common factor number that divides them, exactly. 
- It is also called the [highest common factor (HCF)](https://byjus.com/maths/hcf/). 

Suppose, 4, 8 and 16 are three numbers. Then the factors of 4, 8 and 16 are:

- 4 → 1 x 2 x **4**
- 8 → 1 x 2 x **4** x 8
- 16 → 1 x 2 x **4** x 8 x 16

Therefore, we can conclude that 4 is the highest common factor among all three numbers.

### GCD without Euclidean Algorithm:

```java
	//Function to return gcd of a and b
	static int gcd(int a, int b)
    {
        int result = Math.min(a, b);
        while (result > 0) {
            if (a % result == 0 && b % result == 0) {
                break;
            }
            result--;
        }

        // Return gcd of a and b
        return result;
    }
```

### Euclidean Algorithm

To find the **Greatest Common Divisor (GCD)** of two positive integers:

> Use **repeated division with remainder** until the remainder is 0.  
> The last non-zero remainder is the **GCD**.

**Example: Find GCD(252, 105)**

Step 1: Divide 252 by 105
$$
252 \div 105 = 2 \text{ remainder } 42 \quad \Rightarrow \quad 252 = 2 \times 105 + 42
$$

Step 2: Divide 105 by 42
$$
105 \div 42 = 2 \text{ remainder } 21 \quad \Rightarrow \quad 105 = 2 \times 42 + 21
$$

Step 3: Divide 42 by 21
$$
42 \div 21 = 2 \text{ remainder } 0 \quad \Rightarrow \quad 42 = 2 \times 21 + 0
$$

```java
	//Calculate GCD using the Euclidean algorithm
    public int calculateGCDEuclidean(int a, int b) {
        while (b != 0) {
            int temp = b;
            b = a % b;
            a = temp;
        }
        return a;
    }
```
