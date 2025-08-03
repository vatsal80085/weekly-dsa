## Problem Statement: Word Search  

## Problem Description:
Given an `m x n` grid of characters `board` and a string `word`, return `true` if `word` exists in the grid.

The word can be constructed from letters of sequentially adjacent cells, where "adjacent" cells are those horizontally or vertically neighboring. The same letter cell may not be used more than once.

---

### Constraints:
- `m == board.length`
- `n == board[i].length`
- `1 <= m, n <= 6`
- `1 <= word.length <= 15`
- `board` and `word` consist of only lowercase and/or uppercase English letters.

---

### Approach Used:
I've used **Depth-First Search (DFS)** with **backtracking** to explore all possible paths from each cell in the grid.

Steps:
1. Iterate over each cell in the grid.
2. Start DFS if the character matches the first letter of the word.
3. In DFS:
   - Mark the current cell as visited (e.g., by replacing it with a placeholder).
   - Explore its four adjacent neighbors (up, down, left, right).
   - If any path leads to successfully forming the word, return true.
   - Backtrack by restoring the cell to its original value after recursion.

---

### Key Observations:
- DFS with backtracking ensures all possible paths are explored while preventing reuse of a cell in the same path.
- Grid size is small (maximum 6x6), so a brute-force DFS approach is efficient enough.
- Early pruning is applied by checking bounds and character match at each step.

---

### Java Code (LeetCode-Compatible)

```java
class Solution {
    public boolean exist(char[][] board, String word) {
        int rows = board.length;
        int cols = board[0].length;

        for (int i = 0; i < rows; i++) {
            for (int j = 0; j < cols; j++) {
                if (dfs(board, i, j, word, 0)) {
                    return true;
                }
            }
        }

        return false;
    }

    private boolean dfs(char[][] board, int i, int j, String word, int index) {
        if (index == word.length()) {
            return true;
        }

        if (i < 0 || i >= board.length || j < 0 || j >= board[0].length 
            || board[i][j] != word.charAt(index)) {
            return false;
        }

        char temp = board[i][j];
        board[i][j] = '#'; // mark visited

        boolean found = dfs(board, i + 1, j, word, index + 1) ||
                        dfs(board, i - 1, j, word, index + 1) ||
                        dfs(board, i, j + 1, word, index + 1) ||
                        dfs(board, i, j - 1, word, index + 1);

        board[i][j] = temp; // backtrack
        return found;
    }
}
