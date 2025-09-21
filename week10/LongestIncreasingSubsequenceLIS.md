# Longest Increasing Subsequence (LIS)  
**Company:** Microsoft  
**Difficulty:** Medium  
**Topic:** Dynamic Programming  

## Problem Statement  
Given an array of numbers, find the length of the **Longest Increasing Subsequence (LIS)**.  
The subsequence does not need to be contiguous, but the order must be maintained. 

---

## Approaches  

### Approach 1: Recursive + Memoization (Top-Down DP)  
- Try including or excluding each element.  
- Memoize results to avoid recomputation.  
- **Time Complexity:** O(n²)  
- **Space Complexity:** O(n²)  

### Approach 2: Bottom-Up Dynamic Programming ✅  
- Use an array `dp[i]` = length of LIS ending at index `i`.  
- Transition: `dp[i] = 1 + max(dp[j])` for all `j < i` where `arr[j] < arr[i]`.  
- Answer = max(dp[i]) for all `i`.  
- **Time Complexity:** O(n²)  
- **Space Complexity:** O(n)  

### Approach 3: Optimized with Binary Search (Patience Sorting Method) 🔥  
- Maintain a temporary list.  
- For each number:  
  - If it’s greater than the largest element, append it.  
  - Else, replace the smallest element ≥ current number using binary search.  
- **Time Complexity:** O(n log n)  
- **Space Complexity:** O(n)  

---

## Java Code (Optimized Approach – O(n log n))  

```java
import java.util.*;

public class LongestIncreasingSubsequence {
    public static int lengthOfLIS(int[] nums) {
        List<Integer> lis = new ArrayList<>();
        for (int num : nums) {
            int idx = Collections.binarySearch(lis, num);
            if (idx < 0) idx = -(idx + 1);
            if (idx == lis.size()) {
                lis.add(num);
            } else {
                lis.set(idx, num);
            }
        }
        return lis.size();
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt();
        int[] nums = new int[n];
        for (int i = 0; i < n; i++) {
            nums[i] = sc.nextInt();
        }
        System.out.println(lengthOfLIS(nums));
    }
}
```

## Complexity Analysis

Time Complexity: O(n log n) using binary search for placement.

Space Complexity: O(n) for the temporary LIS list.