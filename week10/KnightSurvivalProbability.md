# Knight’s Survival Probability  
**Company:** Two Sigma  
**Difficulty:** Medium / Hard  
**Topic:** Dynamic Programming, Probability, Recursion  

## Problem Statement  
A knight is placed on an 8×8 chessboard at position `(r, c)` and moves exactly `k` times. Each move is chosen uniformly at random from the 8 possible knight moves, even if some moves land off-board. If the knight moves off the board, it disappears.  
Compute the probability that after `k` moves, the knight remains on the board.  
---

## Approaches  

### Approach 1: Dynamic Programming (Bottom-Up) 
- Define `dp[t][i][j]` = probability the knight is at `(i,j)` after `t` moves and is still on the board.  
- Initialize `dp[0][r][c] = 1.0`.  
- For each move `t` in `0..k-1`:  
  - For each cell `(i,j)`, distribute probability equally among valid knight moves:  
    ```
    dp[t+1][ni][nj] += dp[t][i][j] / 8
    ```  
- The result is the sum of all probabilities in `dp[k]`.  
- **Time Complexity:** O(k * 64 * 8) ≈ O(k).  
- **Space Complexity:** O(64) if rolling arrays.  

### Approach 2: Recursion + Memoization (Top-Down)  
- Use recursion:  
P(t, i, j) = (1/8) * Σ P(t-1, ni, nj)

pgsql
Copy code
where `(ni, nj)` are knight moves.  
- Memoize `(t,i,j)` to avoid recomputation.  

---

## Java Code (Dynamic Programming – Bottom-Up)  

```java
import java.util.*;

public class KnightsSurvivalProbability {
  static int[][] directions = {
      {2, 1}, {1, 2}, {-1, 2}, {-2, 1},
      {-2, -1}, {-1, -2}, {1, -2}, {2, -1}
  };

  public static double knightProbability(int r, int c, int k) {
      double[][] dp = new double[8][8];
      dp[r][c] = 1.0;

      for (int step = 0; step < k; step++) {
          double[][] next = new double[8][8];
          for (int i = 0; i < 8; i++) {
              for (int j = 0; j < 8; j++) {
                  if (dp[i][j] > 0) {
                      for (int[] dir : directions) {
                          int ni = i + dir[0];
                          int nj = j + dir[1];
                          if (ni >= 0 && ni < 8 && nj >= 0 && nj < 8) {
                              next[ni][nj] += dp[i][j] / 8.0;
                          }
                      }
                  }
              }
          }
          dp = next;
      }

      double result = 0.0;
      for (int i = 0; i < 8; i++) {
          for (int j = 0; j < 8; j++) {
              result += dp[i][j];
          }
      }
      return result;
  }

  public static void main(String[] args) {
      Scanner sc = new Scanner(System.in);
      int r = sc.nextInt();
      int c = sc.nextInt();
      int k = sc.nextInt();
      System.out.printf("%.6f\n", knightProbability(r, c, k));
  }
}
```

## Complexity Analysis
Time Complexity: O(k * 64 * 8) = O(k) (since board size is fixed).

Space Complexity: O(64) using rolling DP arrays.

