# Problem of the Week (Hard)

**Company:** Adobe  
**Topic:** Trees, DFS  

## Problem Description
You are given a tree (undirected, connected graph with **N nodes** and **N-1 edges**), where **N is even**.  
Your task is to **remove as many edges as possible** such that every resulting connected component (subtree) has an **even number of nodes**.


**Explanation**
- Subtree sizes:  
  - Removing edge `(3,4)` → subtree `{4,6,7,8}` of size `4` (even).  
  - Removing edge `(1,3)` → subtree `{3,5}` of size `2` (even).  
- Both removals are valid → total answer is **2**.  

---

## Approach

### Approach 1: DFS with Subtree Sizes (Efficient)
1. Perform a **DFS** from the root (say node 1).  
2. Compute the size of each subtree.  
3. If a subtree rooted at node `v` has an **even size**, then the edge connecting it to its parent can be removed.  
4. Count such edges.  

- **Time Complexity:** `O(N)`  
- **Space Complexity:** `O(N)`  

---

### Approach 2: Brute Force (Not Recommended)
- Try removing edges one by one and check if all resulting subtrees have even sizes.  
- Too costly → `O(N^2)` in worst case.  

---

## Java Implementation

```java
import java.util.*;

public class EvenTree {
    static List<List<Integer>> adj;
    static int removableEdges = 0;

    // DFS function to calculate subtree sizes
    public static int dfs(int node, int parent) {
        int subtreeSize = 1; // count self
        for (int child : adj.get(node)) {
            if (child != parent) {
                int childSize = dfs(child, node);
                if (childSize % 2 == 0) {
                    removableEdges++; // edge can be removed
                } else {
                    subtreeSize += childSize;
                }
            }
        }
        return subtreeSize;
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt();

        adj = new ArrayList<>();
        for (int i = 0; i <= n; i++) adj.add(new ArrayList<>());

        for (int i = 0; i < n - 1; i++) {
            int u = sc.nextInt();
            int v = sc.nextInt();
            adj.get(u).add(v);
            adj.get(v).add(u);
        }

        dfs(1, -1);
        System.out.println(removableEdges);
    }
}

