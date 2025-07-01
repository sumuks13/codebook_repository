tags: [[Algorithms]]

The greatest common divisor (GCD) of two or more numbers is the greatest common factor number that divides them, exactly. It is also called the [highest common factor (HCF)](https://byjus.com/maths/hcf/). For example, the greatest common factor of 15 and 10 is 5, since both the numbers can be divided by 5.

15/5 = 3
10/5 = 2

If a and b are two numbers then the greatest common divisor of both the numbers is denoted by gcd(a, b). To find the gcd of numbers, we need to list all the factors of the numbers and find the largest common factor. 

Suppose, 4, 8 and 16 are three numbers. Then the factors of 4, 8 and 16 are:

4 → 1,2,4
8 → 1,2,4,8
16 → 1,2,4,8,16

Therefore, we can conclude that 4 is the highest common factor among all three numbers.

```run-java
// Function to return gcd of a and b
    static int gcd(int a, int b)
    {
        // Find Minimum of a and b
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

```run-java
// Helper method to calculate the greatest common divisor using the Euclidean algorithm
    public int calculateGCDEuclidean(int a, int b) {
        while (b != 0) {
            int temp = b;
            b = a % b;
            a = temp;
        }
        return a;
    }
```
