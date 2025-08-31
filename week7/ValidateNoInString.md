# Validate Number in a String

**Problem Statement**  
I need to check if a given string represents a valid number.  
Valid formats include integers, decimals, and scientific notation (`e/E`).  
Invalid examples include strings with internal spaces, missing digits after `e`, or only a sign.

**Approach**  
I have solved this using **Regex**:  
- Optional leading/trailing spaces allowed.  
- Optional sign `+/-`.  
- Valid numeric formats: integers, decimals, `.5`, `3.` etc.  
- Scientific notation with exponent part.  

Time Complexity: **O(n)** (regex parsing)  
Space Complexity: **O(1)**  

---

```java
class Solution {
    public boolean isNumber(String s) {
        String pattern = "^\\s*[+-]?((\\d+(\\.\\d*)?)|(\\.\\d+))([eE][+-]?\\d+)?\\s*$";
        return s != null && s.matches(pattern);
    }

    // For testing
    public static void main(String[] args) {
        Solution sol = new Solution();
        System.out.println(sol.isNumber("10"));     // true
        System.out.println(sol.isNumber("-10.1"));  // true
        System.out.println(sol.isNumber("1e5"));    // true
        System.out.println(sol.isNumber("a -2"));   // false
        System.out.println(sol.isNumber("."));      // false
    }
}
