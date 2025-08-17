# Problem Title: Flood Fill Algorithm  
**Company Tag:** Facebook  

---

## Problem Scenario
I am given an image represented as a 2D matrix of characters, where each character represents a pixel color. You’re also given the coordinates of a pixel `(sr, sc)` and a new color `C`.  

Perform a **flood fill operation** starting from `(sr, sc)` — change the color of the pixel and all connected pixels (4-directionally) that share the same original color to the new color `C`.

## Approaches
Approach 1: DFS (Depth First Search)

We recursively visit pixels that are connected 4-directionally and have the same original color.

Approach 2: BFS (Breadth First Search)

We use a queue to iteratively traverse connected pixels and fill them.

```java
import java.util.*;

public class FloodFill {

    // -------- DFS Solution --------
    public static char[][] floodFillDFS(char[][] image, int sr, int sc, char newColor) {
        int rows = image.length, cols = image[0].length;
        char original = image[sr][sc];

        if (original == newColor) return image;

        dfs(image, sr, sc, original, newColor, rows, cols);
        return image;
    }

    private static void dfs(char[][] image, int r, int c, char original, char newColor, int rows, int cols) {
        if (r < 0 || r >= rows || c < 0 || c >= cols || image[r][c] != original) return;

        image[r][c] = newColor;

        dfs(image, r + 1, c, original, newColor, rows, cols); // down
        dfs(image, r - 1, c, original, newColor, rows, cols); // up
        dfs(image, r, c + 1, original, newColor, rows, cols); // right
        dfs(image, r, c - 1, original, newColor, rows, cols); // left
    }

    // -------- BFS Solution --------
    public static char[][] floodFillBFS(char[][] image, int sr, int sc, char newColor) {
        int rows = image.length, cols = image[0].length;
        char original = image[sr][sc];

        if (original == newColor) return image;

        Queue<int[]> queue = new LinkedList<>();
        queue.add(new int[]{sr, sc});

        while (!queue.isEmpty()) {
            int[] cell = queue.poll();
            int r = cell[0], c = cell[1];

            if (r < 0 || r >= rows || c < 0 || c >= cols || image[r][c] != original) continue;

            image[r][c] = newColor;

            queue.add(new int[]{r + 1, c}); // down
            queue.add(new int[]{r - 1, c}); // up
            queue.add(new int[]{r, c + 1}); // right
            queue.add(new int[]{r, c - 1}); // left
        }

        return image;
    }

    // -------- Helper to print the image --------
    public static void printImage(char[][] image) {
        for (char[] row : image) {
            System.out.println(Arrays.toString(row));
        }
        System.out.println();
    }

    // -------- Main Method for Testing --------
    public static void main(String[] args) {
        char[][] image = {
            {'B', 'B', 'W'},
            {'W', 'W', 'W'},
            {'W', 'W', 'W'},
            {'B', 'B', 'B'}
        };

        int sr = 2, sc = 2;
        char newColor = 'G';

        System.out.println("Original Image:");
        printImage(image);

        // Run DFS
        char[][] resultDFS = floodFillDFS(copyImage(image), sr, sc, newColor);
        System.out.println("After Flood Fill (DFS):");
        printImage(resultDFS);

        // Run BFS
        char[][] resultBFS = floodFillBFS(copyImage(image), sr, sc, newColor);
        System.out.println("After Flood Fill (BFS):");
        printImage(resultBFS);
    }

    // Utility to avoid in-place modification while testing
    private static char[][] copyImage(char[][] image) {
        char[][] copy = new char[image.length][image[0].length];
        for (int i = 0; i < image.length; i++) {
            copy[i] = Arrays.copyOf(image[i], image[i].length);
        }
        return copy;
    }
}
