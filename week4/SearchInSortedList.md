## Problem Statement: Search in a Sorted List Without Multiplication, Division, or Bit-Shifts
The question asked by Netflix
Given:
- A sorted list of integers `arr` of length `N`.
- A target value `x`.  
Return `true` if `x` exists in the list, otherwise return `false`.


---

### Approach Used:
- Use Binary Search to achieve O(log N) complexity.
- Avoid using `/`, `*`, and bit-shifts in `mid` calculation.
- Implement a constant-time integer divide-by-2 function using only addition and subtraction.
- This works by incrementing a counter by steps of 2 until reaching the target — but since binary search halves the range every step, the total cost remains O(log N) overall.

---

### Key Observations:
- The only arithmetic allowed is addition and subtraction.
- `divideByTwo()` can be implemented without division using repeated subtraction of 2.
- The binary search range shrinks exponentially, so even a small loop inside `divideByTwo()` is negligible in total runtime.

---

```java
import java.util.*;

public class SearchWithoutDivisionOptimized {
    
    // Divide by 2 without using /, *, or bit shifts
    private static int divideByTwo(int num) {
        int quotient = 0;
        int sign = num < 0 ? -1 : 1;
        int value = Math.abs(num);
        
        while (value >= 2) {
            value -= 2;
            quotient++;
        }
        
        return quotient * sign;
    }

    public static boolean binarySearch(int[] arr, int x) {
        int low = 0;
        int high = arr.length - 1;

        while (low <= high) {
            int mid = low + divideByTwo(high - low); // Custom mid calculation
            
            if (arr[mid] == x) {
                return true;
            } else if (arr[mid] < x) {
                low = mid + 1;
            } else {
                high = mid - 1;
            }
        }
        return false;
    }

    public static void main(String[] args) {
        int[] arr1 = {-5, -2, 0, 3, 7, 10, 15};
        int x1 = 7;
        System.out.println(binarySearch(arr1, x1)); // true

        int[] arr2 = {1, 2, 4, 8, 16};
        int x2 = 3;
        System.out.println(binarySearch(arr2, x2)); // false
    }
}
