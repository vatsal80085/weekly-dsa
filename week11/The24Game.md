# Problem of the Week – The 24 Game  

**Company:** Twitter | PayPal (Coding Variant)  
**Difficulty:** Hard  
**Topic:** Recursion, Backtracking, Expression Evaluation  

---

## Scenario  
The 24 Game is a popular math puzzle. You’re given four integers (1–9) in a fixed order, and you must determine if it’s possible to reach exactly 24 by inserting the operators `+`, `-`, `*`, `/` and grouping with parentheses.  

This tests recursion and backtracking skills.  

---

## Problem Statement  
Given a list of 4 integers, determine whether it is possible to reach the value 24 by applying arithmetic operations (`+`, `-`, `*`, `/`) and parentheses in any valid arrangement.  

Return `true` if possible, otherwise `false`.

## Approaches  

### 1. Brute Force Enumeration  
- Try all permutations of numbers.  
- Try all operator combinations.  
- Try all parenthesization orders.  
- Time complexity: large but feasible since N=4.  

### 2. Recursion & Backtracking (Efficient)  
- Treat it as a search problem.  
- At each step, pick two numbers, apply an operation, and recurse with the reduced list.  
- Base case: when only one number remains, check if it is 24 (within floating-point tolerance).  

This is the standard approach used in competitive programming.  

---
### Explanation

We convert the 4 integers into a list of doubles.

Recursively pick two numbers, combine them with all possible operations, and recurse on the new list.

If we reach a list with one number and it is approximately 24, we return true.

The algorithm explores all valid operator/parenthesis placements implicitly.

## Java Solution (Recursion & Backtracking)

```java
import java.util.*;

public class Game24 {
    private static final double EPS = 1e-6;

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        double[] nums = new double[4];
        for (int i = 0; i < 4; i++) {
            nums[i] = sc.nextDouble();
        }
        sc.close();
        System.out.println(judgePoint24(nums));
    }

    public static boolean judgePoint24(double[] nums) {
        List<Double> list = new ArrayList<>();
        for (double n : nums) list.add(n);
        return solve(list);
    }

    private static boolean solve(List<Double> nums) {
        if (nums.size() == 1) {
            return Math.abs(nums.get(0) - 24) < EPS;
        }

        int size = nums.size();
        for (int i = 0; i < size; i++) {
            for (int j = 0; j < size; j++) {
                if (i == j) continue;

                List<Double> next = new ArrayList<>();
                for (int k = 0; k < size; k++) {
                    if (k != i && k != j) next.add(nums.get(k));
                }

                for (double val : compute(nums.get(i), nums.get(j))) {
                    next.add(val);
                    if (solve(next)) return true;
                    next.remove(next.size() - 1);
                }
            }
        }
        return false;
    }

    private static List<Double> compute(double a, double b) {
        List<Double> res = new ArrayList<>();
        res.add(a + b);
        res.add(a - b);
        res.add(b - a);
        res.add(a * b);
        if (Math.abs(b) > EPS) res.add(a / b);
        if (Math.abs(a) > EPS) res.add(b / a);
        return res;
    }
}
```