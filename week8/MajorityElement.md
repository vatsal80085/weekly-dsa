# Find the Majority Element 

## Problem Description
You are given a list of integers.  
A **majority element** in a list is defined as the element that appears more than ⌊n/2⌋ times, where `n` is the length of the list.  

It can be assumed that such an element always exists.

## Approach

I have solved this in two ways:  

1. **HashMap Frequency Count**  
   - Count the occurrences of each number.  
   - Find the one with frequency > ⌊n/2⌋.  
   - Time Complexity: `O(n)`, Space: `O(n)`.  

2. **Boyer–Moore Majority Vote Algorithm (Optimized)** ✅  
   - Maintain a candidate and a counter.  
   - Traverse the array:
     - If counter = 0 → choose current element as candidate.  
     - If element = candidate → increment counter.  
     - Else → decrement counter.  
   - At the end, the candidate will be the majority element.  
   - Time Complexity: `O(n)`, Space: `O(1)`.  

I have used the **Boyer–Moore Algorithm** for efficiency.  

## Java Solution

```java
import java.util.*;

public class MajorityElement {
    public static int findMajorityElement(int[] nums) {
        int candidate = nums[0];
        int count = 0;

        for (int num : nums) {
            if (count == 0) {
                candidate = num;
            }
            if (num == candidate) {
                count++;
            } else {
                count--;
            }
        }

        return candidate;
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        // Input size
        int n = sc.nextInt();
        int[] arr = new int[n];

        // Input array elements
        for (int i = 0; i < n; i++) {
            arr[i] = sc.nextInt();
        }

        // Print majority element
        System.out.println(findMajorityElement(arr));

        sc.close();
    }
}
