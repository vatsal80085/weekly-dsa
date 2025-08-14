## Problem Statement: First Recurring Character in a String
The question asked by Google

**Problem Statement:**
Given a string `s`, return the first character that appears more than once, with the earliest
second appearance.
If there is no such character, return `null`.

---

### Approach Used:
- Used a `HashSet` to keep track of characters seen so far.
- Traverse the string from left to right.
- As soon as a character is seen again, return it.
- If the loop finishes, return `null`.

### Key Observations:
- The first recurring character is determined by the earliest *second occurrence*, not the earliest *first occurrence*.
- Using a `HashSet` ensures O(1) average lookup and insert time, making the algorithm O(n) overall.

```java
import java.util.HashSet;

public class FirstRecurringCharacter {
    public static String firstRecurringChar(String s) {
        HashSet<Character> seen = new HashSet<>();
        
        for (char c : s.toCharArray()) {
            if (seen.contains(c)) {
                return String.valueOf(c); // Return first recurring char
            }
            seen.add(c);
        }
        return null; // No recurring char found
    }

    public static void main(String[] args) {
        // Test cases
        String s1 = "acbbac";
        String s2 = "abcdef";
        String s3 = "abca";

        System.out.println(firstRecurringChar(s1)); // Output: b
        System.out.println(firstRecurringChar(s2)); // Output: null
        System.out.println(firstRecurringChar(s3)); // Output: a
    }
}
