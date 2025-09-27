# Problem of the Week – Fixed Point in a Sorted Array  

## Problem Statement  
We need to find an index `i` in a sorted array where `arr[i] == i`. Return the value if found, else return `False`.

---

## Intuition  
Because the array is sorted and contains distinct integers, the relation between `arr[mid]` and `mid` allows us to decide which half to search:  
- If `arr[mid] == mid` → fixed point found.  
- If `arr[mid] > mid` → all right-side indices will be larger, so search left.  
- If `arr[mid] < mid` → all left-side indices will be smaller, so search right.  

---

## Approaches  

### 1. Brute Force – O(N)  
- Iterate through each index and check `arr[i] == i`.  
- Return the first such value if found; otherwise `False`.  

### 2. Efficient Binary Search – O(log N) – Recommended  
- Use binary search:  
  - Compute `mid`.  
  - If `arr[mid] == mid` → return `mid`.  
  - If `arr[mid] > mid` → search left half.  
  - Else search right half.  

---

## Complexity  
- **Time:** `O(log N)` for binary search.  
- **Space:** `O(1)`.  

---

## Java Code Solution  

```java
import java.util.*;

public class FixedPointInSortedArray {
    public static int findFixedPoint(int[] arr) {
        int left = 0, right = arr.length - 1;

        while (left <= right) {
            int mid = left + (right - left) / 2;

            if (arr[mid] == mid) {
                return mid; // fixed point found
            } else if (arr[mid] > mid) {
                right = mid - 1; // search left half
            } else {
                left = mid + 1; // search right half
            }
        }
        return -1; // no fixed point
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt();
        int[] arr = new int[n];
        for (int i = 0; i < n; i++) {
            arr[i] = sc.nextInt();
        }
        int fixedPoint = findFixedPoint(arr);
        if (fixedPoint != -1) {
            System.out.println(fixedPoint);
        } else {
            System.out.println("False");
        }
    }
}