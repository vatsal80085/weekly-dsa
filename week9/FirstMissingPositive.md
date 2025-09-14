# Problem of the Week – First Missing Positive Integer  
Company: Stripe  
Difficulty: Hard  
Topic: Arrays, Hashing, In-place Rearrangement  

## Problem Statement  
Given an unsorted array of integers arr[], return the smallest positive integer missing from the array.  
- The array may contain duplicates and negative numbers.  
- Modify the input array in-place.  
- Time: O(n), Space: O(1).  

### Approaches  
1. Naive Approach (Sorting / Hashing)  
   - Sort array or use a HashSet to find missing positive.  
   - Time O(n log n) or O(n), Space O(n) (fails requirement).  

2. Optimal Approach (Index Placement / In-Place Hashing)  
   - Place each number x into index x-1 if 1 ≤ x ≤ N.  
   - Traverse: first index i where arr[i] != i+1 → answer = i+1.  
   - If all positions correct → return N+1.  

## Java Code  
```java
import java.util.*;

public class FirstMissingPositive {
    public static int firstMissingPositive(int[] nums) {
        int n = nums.length;
        for (int i = 0; i < n; i++) {
            while (nums[i] > 0 && nums[i] <= n && nums[nums[i]-1] != nums[i]) {
                int temp = nums[i];
                nums[i] = nums[temp-1];
                nums[temp-1] = temp;
            }
        }
        for (int i = 0; i < n; i++) {
            if (nums[i] != i + 1) return i + 1;
        }
        return n + 1;
    }
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt();
        int[] arr = new int[n];
        for (int i = 0; i < n; i++) arr[i] = sc.nextInt();
        System.out.println(firstMissingPositive(arr));
    }
}
```
