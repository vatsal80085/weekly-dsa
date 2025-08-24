# Problem Title: Minimum Number of Perfect Squares to Sum to N  

**Company Tag:** Asked by Facebook  

---

## Problem Description  
You are given a positive integer `n`. Your task is to determine the smallest number of perfect square numbers that sum exactly to `n`.  

A perfect square is an integer that is the square of an integer (e.g., 1, 4, 9, 16, ...).  
The same perfect square can be used multiple times if necessary.  

---

## Approaches  

### 1. Dynamic Programming (Bottom-Up)  
- Treat this like a **coin change problem**.  
- `dp[i]` = minimum number of perfect squares that sum to `i`.  
- Transition:  
dp[i] = min(dp[i], 1 + dp[i - square]) for all square <= i

- Time Complexity: **O(n * sqrt(n))**  
- Space Complexity: **O(n)**  

---

### 2. Breadth-First Search (BFS)  
- Each state = current remainder after subtracting a square.  
- Level in BFS = number of squares used.  
- First time reaching `0` → answer.  
- Works like shortest path search.  
- Time Complexity: **O(n * sqrt(n))**  
- Space Complexity: **O(n)**  

---

### 3. Mathematical Approach (Lagrange’s Four-Square Theorem)  
- Any number can be represented as the sum of **at most 4 squares**.  
- Check in order:  
1. If `n` is itself a square → answer = 1  
2. If `n` can be written as sum of 2 squares → answer = 2  
3. If `n` can be written as sum of 3 squares → answer = 3  
4. Else → answer = 4  
- Most optimal for very large `n`.  

---

## Java Solutions  

### Approach 1: Dynamic Programming  

```java
import java.util.*;

public class PerfectSquaresDP {
  public static int numSquares(int n) {
      int[] dp = new int[n + 1];
      Arrays.fill(dp, Integer.MAX_VALUE);
      dp[0] = 0;

      for (int i = 1; i <= n; i++) {
          for (int j = 1; j * j <= i; j++) {
              dp[i] = Math.min(dp[i], dp[i - j * j] + 1);
          }
      }
      return dp[n];
  }

  public static void main(String[] args) {
      Scanner sc = new Scanner(System.in);
      int n = sc.nextInt();
      System.out.println(numSquares(n));
      sc.close();
  }
}
```

Approach 2: BFS

```java
import java.util.*;

public class PerfectSquaresBFS {
    public static int numSquares(int n) {
        Queue<Integer> queue = new LinkedList<>();
        boolean[] visited = new boolean[n + 1];
        queue.add(n);
        visited[n] = true;
        int level = 0;

        while (!queue.isEmpty()) {
            int size = queue.size();
            level++;
            for (int i = 0; i < size; i++) {
                int remainder = queue.poll();
                for (int j = 1; j * j <= remainder; j++) {
                    int next = remainder - j * j;
                    if (next == 0) return level;
                    if (!visited[next]) {
                        visited[next] = true;
                        queue.add(next);
                    }
                }
            }
        }
        return level;
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt();
        System.out.println(numSquares(n));
        sc.close();
    }
}
```

Approach 3: Mathematical (Optimized Check)

```java
import java.util.*;

public class PerfectSquaresMath {
    private static boolean isPerfectSquare(int n) {
        int sqrt = (int)Math.sqrt(n);
        return sqrt * sqrt == n;
    }

    public static int numSquares(int n) {
        if (isPerfectSquare(n)) return 1;

        // Check for 2 squares
        for (int i = 1; i * i <= n; i++) {
            if (isPerfectSquare(n - i * i)) return 2;
        }

        // Check for 4 squares condition (Legendre’s theorem)
        while (n % 4 == 0) n /= 4;
        if (n % 8 == 7) return 4;

        // Otherwise, answer is 3
        return 3;
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt();
        System.out.println(numSquares(n));
        sc.close();
    }
}