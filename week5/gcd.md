# Problem Title: Find the Greatest Common Divisor (GCD) of N Numbers  

**Company Tag:** Asked by Amazon  

---

## Problem Statement  
Given n numbers, find the greatest common divisor between them.  

## Approach  
- I have Used the **Euclidean Algorithm** for efficiency:  
  - gcd(a, b) = gcd(b, a % b)  
  - Extend to `n` numbers by iteratively applying gcd(result, arr[i]).  
- Time Complexity: **O(n log M)** (where `M` is the largest number).  

---

## Java Solution  

```java
import java.util.*;

public class GCDofNNumbers {

    // Function to calculate gcd of two numbers using Euclidean Algorithm
    private static int gcd(int a, int b) {
        while (b != 0) {
            int temp = b;
            b = a % b;
            a = temp;
        }
        return a;
    }

    // Function to calculate gcd of n numbers
    private static int findGCD(int[] arr, int n) {
        int result = arr[0];
        for (int i = 1; i < n; i++) {
            result = gcd(result, arr[i]);

            // Early exit if gcd becomes 1
            if (result == 1) {
                return 1;
            }
        }
        return result;
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        // Input size of array
        int n = sc.nextInt();
        int[] arr = new int[n];

        // Input array elements
        for (int i = 0; i < n; i++) {
            arr[i] = sc.nextInt();
        }

        // Compute and print GCD
        System.out.println(findGCD(arr, n));

        sc.close();
    }