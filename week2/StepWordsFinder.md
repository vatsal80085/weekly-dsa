# Problem of the Week – Step Words Finder

**Asked by:** Pivotal  
**Difficulty:** Medium  
**Category:** String Matching, Anagram Check

## Problem Description

In a word game, a **step word** is formed by adding **exactly one letter** to a given word and then rearranging (anagramming) it to form a valid dictionary word.

### Example:
From `"apple"` → Add `"a"` → `"applea"` → anagram to `"appeal"` → **Valid Step Word**

### Task:
Given a word and a dictionary of valid English words, return **all valid step words** that can be formed from the input word using the above logic.

---

## Input Format
- First line: a lowercase string `W` (input word)
- Second line: integer `N` (number of words in dictionary)
- Next N lines: each contains a lowercase valid word from the dictionary

## Output Format
- Print each valid step word (one per line), in **lexicographical order**

---

## Constraints
- 1 ≤ len(W) ≤ 15  
- 1 ≤ N ≤ 10⁵  
- All words are lowercase and alphabetic

---

## Example

**Input:**
```
apple
5
appeal
apply
pepla
papple
apples
```

**Output:**
```
appeal
papple
```

### Explanation:
- `"appeal"` ← add `"a"` to `"apple"` → `"applea"` → anagram → valid  
- `"papple"` ← add `"p"` to `"apple"` → `"applep"` → anagram → valid  
- `"apply"` → missing `"e"` → invalid  
- `"pepla"` → just reordering, not adding a character → invalid  
- `"apples"` → added `"s"` but overall letter frequencies don’t match properly → invalid

---

## Approach

We use **character frequency arrays** to detect valid step words.

### Steps:
1. Build a frequency array `baseFreq[26]` for the input word.
2. For each word in the dictionary:
   - Skip if `word.length() != base.length() + 1`
   - Count character frequency of the dictionary word.
   - Check:
     - All characters in `baseFreq` must be ≤ in dictionary word.
     - Only one extra character should exist (i.e., total extra = 1)
3. Sort and print all valid step words.

### Time Complexity:
- **O(N * K)** where `N` = number of words, `K` = average word length (≤15)

---

## Java Code

```java
import java.util.*;

public class StepWordsFinder {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        String base = sc.nextLine().trim().toLowerCase();
        int n = Integer.parseInt(sc.nextLine());
        List<String> dictionary = new ArrayList<>();
        for (int i = 0; i < n; i++) {
            dictionary.add(sc.nextLine().trim().toLowerCase());
        }

        List<String> result = new ArrayList<>();

        int[] baseFreq = buildFreq(base);

        for (String word : dictionary) {
            if (word.length() != base.length() + 1) continue;

            int[] wordFreq = buildFreq(word);

            int extraChars = 0;
            boolean valid = true;

            for (int i = 0; i < 26; i++) {
                if (wordFreq[i] < baseFreq[i]) {
                    valid = false;
                    break;
                }
                extraChars += wordFreq[i] - baseFreq[i];
            }

            if (valid && extraChars == 1) {
                result.add(word);
            }
        }

        Collections.sort(result);
        for (String res : result) {
            System.out.println(res);
        }
    }

    private static int[] buildFreq(String word) {
        int[] freq = new int[26];
        for (char c : word.toCharArray()) {
            freq[c - 'a']++;
        }
        return freq;
    }
}
```
