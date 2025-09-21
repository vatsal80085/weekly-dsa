# Bitwise AND of a Range  
**Company:** Yahoo  
**Difficulty:** Medium  
**Topic:** Bit Manipulation  

## Problem Statement  
Write a function that returns the bitwise AND of all integers between **M** and **N** (inclusive).  
Formally:
result = M & (M+1) & (M+2) & ... & N

---

## Approaches  

### Approach 1: Naive Iteration  
- Iterate from M to N.  
- Keep applying `&` operator to all numbers in the range.  
- **Time Complexity:** O(N-M) (inefficient for large ranges).  

### Approach 2: Optimized Bitwise Trick   
- The result is the common binary prefix of M and N.  
- Keep shifting M and N right until they are equal.  
- Shift the result back left by the number of shifts performed.  
- **Time Complexity:** O(log N)  
- **Space Complexity:** O(1)  

---

## Java Code (Optimized)  

```java
import java.util.*;

public class BitwiseANDRange {
    public static int rangeBitwiseAnd(int m, int n) {
        int shift = 0;
        // Find common prefix
        while (m < n) {
            m >>= 1;
            n >>= 1;
            shift++;
        }
        return m << shift;
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int m = sc.nextInt();
        int n = sc.nextInt();
        System.out.println(rangeBitwiseAnd(m, n));
    }
}
```

## Complexity Analysis

Time Complexity: O(log N) (number of bits in N).
Space Complexity: O(1).