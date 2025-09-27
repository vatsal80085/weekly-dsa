# Problem of the Week – Dropbox Question  

## Problem Description  

Given a string `s` and a list of words `words` (all words of equal length),  
find all starting indices of substrings in `s` that are the concatenation of **every word in `words` exactly once**, in any order and without extra characters in between.  

If no such substring exists, return an empty list.  

---
## Intuition  
Because all words have the same length, the candidate substring can be broken into chunks of `wordLen`.  
We maintain a frequency map of all words in `words`.  
Then slide a window of size `totalLen` over `s` and check if the words inside match the frequency map.

---

## Approaches  

### 1. Naive Approach  
- For each index in `s`, take substring of length `totalLen`.  
- Split into `wordLen` chunks.  
- Check if each word appears the correct number of times.  
- Time: `O(N * numWords * wordLen)`  
Inefficient for large input.

### 2. Optimized Approach (Sliding Window + HashMap) – Recommended  
- Precompute `wordCount` map from `words`.  
- For each possible offset (0 to `wordLen - 1`):  
  - Slide window in steps of `wordLen`.  
  - Use a secondary map to track counts in the current window.  
  - When window size equals `numWords`, compare counts with `wordCount`.  
- Time: `O(N * wordLen)` with small constant factors.  

---

## Complexity  
- **Time:** `O(N * wordLen)`  
- **Space:** `O(numWords)` for maps.

---

## Java Code Solution  

```java
import java.util.*;

public class DropboxQuestion {
    public static List<Integer> findSubstring(String s, String[] words) {
        List<Integer> result = new ArrayList<>();
        if (s == null || s.length() == 0 || words == null || words.length == 0)
            return result;

        int wordLen = words[0].length();
        int numWords = words.length;
        int totalLen = wordLen * numWords;

        if (s.length() < totalLen) return result;

        // Build frequency map of words
        Map<String, Integer> wordCount = new HashMap<>();
        for (String w : words) {
            wordCount.put(w, wordCount.getOrDefault(w, 0) + 1);
        }

        // We need to check for each possible offset
        for (int i = 0; i < wordLen; i++) {
            int left = i, right = i;
            Map<String, Integer> windowCount = new HashMap<>();
            int count = 0; // number of matched words

            while (right + wordLen <= s.length()) {
                String word = s.substring(right, right + wordLen);
                right += wordLen;

                if (wordCount.containsKey(word)) {
                    windowCount.put(word, windowCount.getOrDefault(word, 0) + 1);
                    count++;

                    // If frequency exceeded, shrink window
                    while (windowCount.get(word) > wordCount.get(word)) {
                        String leftWord = s.substring(left, left + wordLen);
                        windowCount.put(leftWord, windowCount.get(leftWord) - 1);
                        left += wordLen;
                        count--;
                    }

                    // All words matched
                    if (count == numWords) {
                        result.add(left);
                        // Move window forward
                        String leftWord = s.substring(left, left + wordLen);
                        windowCount.put(leftWord, windowCount.get(leftWord) - 1);
                        left += wordLen;
                        count--;
                    }
                } else {
                    // Reset window if word not in list
                    windowCount.clear();
                    count = 0;
                    left = right;
                }
            }
        }

        return result;
    }

    // testing code
    public static void main(String[] args) {
        String s1 = "dogcatcatcodecatdog";
        String[] words1 = {"cat", "dog"};
        System.out.println(findSubstring(s1, words1)); // [0, 13]

        String s2 = "barfoobazbitbyte";
        String[] words2 = {"dog", "cat"};
        System.out.println(findSubstring(s2, words2)); // []
    }
}