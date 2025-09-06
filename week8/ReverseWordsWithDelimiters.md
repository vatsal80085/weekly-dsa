# Reverse Words with Delimiters

You are given a string S consisting of words and delimiters. You need to reverse the words in
the string while keeping the relative order of the delimiters intact.

## Approaches

### Approach 1 — Tokenize + Reverse Words
1. Parse the string into a list of tokens where each token is either:
   - A word token (continuous letters), or
   - A delimiter token (continuous non-letters).
2. Collect the word tokens in order.
3. Reconstruct the final string by iterating tokens; whenever a token is a word, replace it with the next word from the **reversed** list — keep delimiter tokens unchanged.

**Complexity:**  
- Time: `O(n)` (single pass to tokenize + single pass to rebuild)  
- Space: `O(n)` for token list and word list

### Approach 2 — Stack-based (Push words, pop while rebuilding)
1. Tokenize the string into word/delimiter tokens (same as above).
2. Push all word tokens onto a stack.
3. Re-scan the token list and build the output: replace each word token by `stack.pop()` (so words come out reversed), keep delimiters as-is.

**Complexity:**  
- Time: `O(n)`  
- Space: `O(n)` for tokens and stack

Both approaches are straightforward and robust when delimiters are arbitrary sequences and when there are leading/trailing delimiters.

---

## Java Implementations

```java
import java.util.*;

public class ReverseWordsWithDelimiters {

    /**
     * Approach 1: Tokenize + reverse words list
     */
    public static String reverseWordsByTokens(String s) {
        if (s == null || s.length() == 0) return s;

        List<String> tokens = new ArrayList<>();
        StringBuilder cur = new StringBuilder();
        boolean curIsLetter = Character.isLetter(s.charAt(0));

        for (int i = 0; i < s.length(); i++) {
            char ch = s.charAt(i);
            boolean isLetter = Character.isLetter(ch);
            if (cur.length() == 0) {
                cur.append(ch);
                curIsLetter = isLetter;
            } else {
                if (isLetter == curIsLetter) {
                    cur.append(ch);
                } else {
                    tokens.add(cur.toString());
                    cur.setLength(0);
                    cur.append(ch);
                    curIsLetter = isLetter;
                }
            }
        }
        if (cur.length() > 0) tokens.add(cur.toString());

        // Collect words and reverse index
        List<String> words = new ArrayList<>();
        for (String t : tokens) {
            if (!t.isEmpty() && Character.isLetter(t.charAt(0))) {
                words.add(t);
            }
        }
        int wIdx = words.size() - 1;

        // Reconstruct
        StringBuilder out = new StringBuilder();
        for (String t : tokens) {
            if (!t.isEmpty() && Character.isLetter(t.charAt(0))) {
                out.append(words.get(wIdx--));
            } else {
                out.append(t);
            }
        }
        return out.toString();
    }

    /**
     * Approach 2: Stack-based (push words, pop when reconstructing)
     */
    public static String reverseWordsByStack(String s) {
        if (s == null || s.length() == 0) return s;

        List<String> tokens = new ArrayList<>();
        StringBuilder cur = new StringBuilder();
        boolean curIsLetter = Character.isLetter(s.charAt(0));

        for (int i = 0; i < s.length(); i++) {
            char ch = s.charAt(i);
            boolean isLetter = Character.isLetter(ch);
            if (cur.length() == 0) {
                cur.append(ch);
                curIsLetter = isLetter;
            } else {
                if (isLetter == curIsLetter) {
                    cur.append(ch);
                } else {
                    tokens.add(cur.toString());
                    cur.setLength(0);
                    cur.append(ch);
                    curIsLetter = isLetter;
                }
            }
        }
        if (cur.length() > 0) tokens.add(cur.toString());

        // Push words on stack
        Deque<String> stack = new ArrayDeque<>();
        for (String t : tokens) {
            if (!t.isEmpty() && Character.isLetter(t.charAt(0))) {
                stack.push(t);
            }
        }

        // Reconstruct
        StringBuilder out = new StringBuilder();
        for (String t : tokens) {
            if (!t.isEmpty() && Character.isLetter(t.charAt(0))) {
                out.append(stack.pop());
            } else {
                out.append(t);
            }
        }
        return out.toString();
    }

    // ----------- Simple test driver -----------
    public static void main(String[] args) {
        String[] tests = {
            "hello/world:here",
            "hello/world:here/",
            "hello//world:here",
            "/leading/delim",
            "trailing/delim/",
            "singleword",
            "//::",
            ""
        };

        System.out.println("=== reverseWordsByTokens ===");
        for (String t : tests) {
            System.out.printf("Input : \"%s\"\nOutput: \"%s\"\n\n", t, reverseWordsByTokens(t));
        }

        System.out.println("=== reverseWordsByStack ===");
        for (String t : tests) {
            System.out.printf("Input : \"%s\"\nOutput: \"%s\"\n\n", t, reverseWordsByStack(t));
        }
    }
}
