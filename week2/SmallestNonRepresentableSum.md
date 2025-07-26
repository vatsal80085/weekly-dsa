# Problem of the Week – Smallest Non-Representable Sum

**Asked by:** Amazon  
**Difficulty:** Medium  
**Category:** Greedy Algorithm

## Problem Description

You are designing a secure digital wallet system. Each user has a set of coin denominations (represented as a sorted array of positive integers).  
For internal validation, you must determine the **smallest amount of money that cannot be formed** using any subset of the given denominations.

This functionality is crucial for detecting missing denominations and optimizing wallet design.

### Input Format
- A single line containing space-separated sorted positive integers:  
  `a1 a2 a3 ... an`

### Output Format
- A single integer: the smallest positive integer that **cannot** be formed as the sum of any subset of the array.

### Constraints
- 1 ≤ N ≤ 10⁵  
- 1 ≤ a[i] ≤ 10⁹  
- The array is **sorted in increasing order**  
- All elements are positive integers

---

## Example

**Input:**
```
1 2 3 10
```

**Output:**
```
7
```

**Explanation:**  
We can form:  
1, 2, 3, 1+2=3, 1+3=4, 2+3=5, 1+2+3=6  
But we **cannot** form **7**, which is the **smallest non-representable sum**.

---

## Approach

We use a **greedy approach**:

1. Initialize `res = 1`, the smallest value we can't form yet.
2. Traverse the array:
   - If `arr[i] > res`, we cannot build `res`, so it's the answer.
   - Else, we can form all sums up to `res + arr[i] - 1` → update `res += arr[i]`

### Why It Works:
By always tracking the next smallest unrepresentable sum and extending the range when possible, we can detect the first gap in representability.

### Time Complexity
- **O(N)** — one linear pass through the array  
- **O(1)** extra space (excluding input)

---

## Java Code

```java
import java.util.Scanner;

public class SmallestNonRepresentableSum {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        String[] tokens = sc.nextLine().trim().split("\\s+");
        int n = tokens.length;
        long[] arr = new long[n];
        
        for (int i = 0; i < n; i++) {
            arr[i] = Long.parseLong(tokens[i]);
        }

        long res = 1;

        for (int i = 0; i < n; i++) {
            if (arr[i] > res) {
                break;
            }
            res += arr[i];
        }

        System.out.println(res);
    }
}
```
