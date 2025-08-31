# Prime Numbers with Multiple Occurrences

**Problem Statement**  
Given an integer array, find all prime numbers that appear more than once.  
Return them in the order of first appearance.  
If no such prime exists, return `-1`.

**Approach**  
1. Use a prime-check function.  
2. Traverse array and count frequency of primes using `LinkedHashMap` (preserves order).  
3. Collect primes that occur more than once.  
4. Print result or `-1` if none exist.  

Time Complexity: **O(N√M)** (N = array size, M = max element)  
Space Complexity: **O(N)**  


```java
import java.util.*;

public class PrimeDuplicates {
    
    static boolean isPrime(int num) {
        if (num <= 1) return false;
        if (num == 2) return true;
        if (num % 2 == 0) return false;
        for (int i = 3; i * i <= num; i += 2) {
            if (num % i == 0) return false;
        }
        return true;
    }

    static List<Integer> findPrimeDuplicates(int[] arr) {
        Map<Integer, Integer> freq = new LinkedHashMap<>();
        for (int num : arr) {
            if (isPrime(num)) {
                freq.put(num, freq.getOrDefault(num, 0) + 1);
            }
        }
        List<Integer> result = new ArrayList<>();
        for (Map.Entry<Integer, Integer> entry : freq.entrySet()) {
            if (entry.getValue() > 1) {
                result.add(entry.getKey());
            }
        }
        return result;
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int N = sc.nextInt();
        int[] A = new int[N];
        for (int i = 0; i < N; i++) {
            A[i] = sc.nextInt();
        }
        List<Integer> result = findPrimeDuplicates(A);
        if (result.isEmpty()) {
            System.out.println("-1");
        } else {
            for (int num : result) {
                System.out.print(num + " ");
            }
        }
        sc.close();
    }
}
