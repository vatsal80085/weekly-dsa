# Problem of the Week – Longest Common Subsequence of Three Strings

Asked by: YouTube  
Difficulty: Medium  

## Problem Statement

In real-world applications like version control, spell checking, or DNA sequencing, comparing multiple strings and identifying common subsequences is crucial.

Given three strings, your task is to compute the length of the longest common subsequence (LCS) that appears in all three strings. A subsequence is a sequence that appears in the same relative order, but not necessarily contiguously.

### Input
- Three lines of input, each containing a string: `S1`, `S2`, `S3`

### Output
- A single integer: length of the longest common subsequence among the three strings

### Constraints
- 1 ≤ |S1|, |S2|, |S3| ≤ 100
- Strings may contain lowercase or uppercase English letters

### Example
Input:
```
epidemiologist
refrigeration
supercalifragilisticexpialodocious
```

Output:
```
5
```

Explanation:  
The longest common subsequence is "eieio", hence the output is 5.

---

## Approach

We use Dynamic Programming with a 3D DP table `dp[i][j][k]` where:

- `i`, `j`, and `k` represent indices in `S1`, `S2`, and `S3`
- `dp[i][j][k]` stores the length of the LCS of the first `i` characters of `S1`, first `j` of `S2`, and first `k` of `S3`

### Transition:
- If `S1[i-1] == S2[j-1] == S3[k-1]`:  
  `dp[i][j][k] = 1 + dp[i-1][j-1][k-1]`
- Else:  
  `dp[i][j][k] = max(dp[i-1][j][k], dp[i][j-1][k], dp[i][j][k-1])`

### Time Complexity
- O(N^3), where N ≤ 100 — acceptable for this constraint

---

## Java Code

```java
import java.util.Scanner;

public class LongestCommonSubsequenceOfThree {
    public static int lcsOfThree(String s1, String s2, String s3) {
        int n1 = s1.length();
        int n2 = s2.length();
        int n3 = s3.length();

        int[][][] dp = new int[n1 + 1][n2 + 1][n3 + 1];

        for (int i = 1; i <= n1; i++) {
            for (int j = 1; j <= n2; j++) {
                for (int k = 1; k <= n3; k++) {
                    if (s1.charAt(i - 1) == s2.charAt(j - 1) && s1.charAt(i - 1) == s3.charAt(k - 1)) {
                        dp[i][j][k] = 1 + dp[i - 1][j - 1][k - 1];
                    } else {
                        dp[i][j][k] = Math.max(dp[i - 1][j][k],
                                        Math.max(dp[i][j - 1][k], dp[i][j][k - 1]));
                    }
                }
            }
        }

        return dp[n1][n2][n3];
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        String s1 = sc.nextLine().trim();
        String s2 = sc.nextLine().trim();
        String s3 = sc.nextLine().trim();
        System.out.println(lcsOfThree(s1, s2, s3));
    }
}
```
