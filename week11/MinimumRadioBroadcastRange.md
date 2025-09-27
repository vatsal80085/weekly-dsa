# Problem of the Week – Minimum Radio Broadcast Range

Company: Spotify
Difficulty: Medium
Topic: Greedy, Binary Search, Arrays

## Problem Statement

Given:
• A list of N listeners’ positions.
• A list of M radio towers’ positions.
Find the minimum broadcast range R such that every listener is within range of at least
one tower

## Intuition

For each listener, the closest tower defines how far the signal must reach for that listener.
If we take the maximum of these nearest distances across all listeners, that’s the minimum broadcast range needed to cover everyone.

This is almost identical to the “Heaters” problem (LeetCode 475).

## Approaches

1. Brute Force (O(N × M))
For each listener:
Compute its distance to every tower.
Take the minimum distance.
Finally, take the maximum of all these minimum distances.
Works but too slow for large N and M.

2. Efficient Binary Search (O(N log M))
Sort tower positions.
For each listener:
Use binary search to locate the closest tower (left/right neighbor).
Take the min distance.
Track the max of these min distances.
Much faster for large inputs.

3. Two-Pointer Greedy (O(N + M))
Sort listeners and towers.
Use two pointers:
Move through listeners.
For each listener, advance the tower pointer while the next tower is closer.
This gives the closest tower in O(1) amortized per listener.
Keep updating the max distance.

## Java Code (Binary Search Approach)
```java
import java.util.*;

public class MinimumBroadcastRange {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        
        int N = sc.nextInt();              // number of listeners
        int[] listeners = new int[N];
        for (int i = 0; i < N; i++) listeners[i] = sc.nextInt();
        
        int M = sc.nextInt();              // number of towers
        int[] towers = new int[M];
        for (int i = 0; i < M; i++) towers[i] = sc.nextInt();
        
        // Solve
        int result = findMinBroadcastRange(listeners, towers);
        System.out.println(result);
    }
    
    static int findMinBroadcastRange(int[] listeners, int[] towers) {
        Arrays.sort(listeners);
        Arrays.sort(towers);
        int maxRange = 0;
        
        for (int listener : listeners) {
            // find closest tower for this listener
            int idx = Arrays.binarySearch(towers, listener);
            if (idx >= 0) {
                // listener exactly at tower
                continue; // distance 0, no need to update maxRange
            } else {
                idx = -idx - 1; // insertion point
                int dist1 = (idx < towers.length) ? Math.abs(towers[idx] - listener) : Integer.MAX_VALUE;
                int dist2 = (idx > 0) ? Math.abs(listener - towers[idx - 1]) : Integer.MAX_VALUE;
                int nearest = Math.min(dist1, dist2);
                maxRange = Math.max(maxRange, nearest);
            }
        }
        return maxRange;
    }
}
```

## How it Works

Arrays.binarySearch finds where the listener would be inserted among towers.
We check distance to the closest tower on either side.
We track the largest of these minimum distances = answer.

## Complexity

Sorting: O(N log N + M log M)
For each listener, binary search in towers: O(log M)
Total: O(N log M + (N+M) log) ≈ fast for 10^5 scale.