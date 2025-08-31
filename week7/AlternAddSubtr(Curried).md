# Alternating Add and Subtract (Curried Function)

**Problem Statement**  
Implement a `add_subtract` function in curried style that alternately adds and subtracts numbers.  
- 1st number → add  
- 2nd number → subtract  
- 3rd number → add, etc.  

**Approach**  
- Use a class to store running sum and toggle sign on each call.  
- Return a new object on every call to allow chaining (curried style).  

Time Complexity: **O(N)** (N = number of chained calls)  
Space Complexity: **O(1)**  

---

```java
public class AlternatingSum {
    private int value;
    private boolean addNext;

    private AlternatingSum(int value, boolean addNext) {
        this.value = value;
        this.addNext = addNext;
    }

    public static AlternatingSum add_subtract(int num) {
        return new AlternatingSum(num, false);
    }

    public AlternatingSum apply(int num) {
        if (addNext) {
            value += num;
        } else {
            value -= num;
        }
        addNext = !addNext;
        return this;
    }

    public int result() {
        return value;
    }

    public static void main(String[] args) {
        // Example usage:
        System.out.println(AlternatingSum.add_subtract(7).result()); // 7
        System.out.println(AlternatingSum.add_subtract(1).apply(2).apply(3).result()); // 0
        System.out.println(AlternatingSum.add_subtract(-5).apply(10).apply(3).apply(9).result()); // 11
    }
}
