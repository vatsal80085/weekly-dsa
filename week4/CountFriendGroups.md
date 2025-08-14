## Problem Statement: Count Friend Groups
The question asked by Twitter

## Problem Description:
**Scenario:**
Imagine a classroom of `N` students. Students can be friends with one another, and this
friendship relationship is mutual (i.e., if A is a friend of B, B is also a friend of A).

We’re given these relationships as an adjacency list, where:
- Each key is a student.
- Each value is the list of students they are directly friends with.

A **friend group** is a set of students where every student is connected (directly or indirectly) to every other student in that set.

**Goal:**  
Find the total number of friend groups (connected components in an undirected graph).
---

### Approach Used:
- Treat the problem as **graph connected components** counting.
- Use **DFS** or **BFS** to explore each connected component.
- Maintain a **visited** set to ensure we don’t recount nodes.
- For each unvisited student:
  - Start DFS/BFS.
  - Mark all connected students as visited.
  - Increment group count.

---

### Key Observations:
- Friendship is mutual, so the graph is **undirected**.
- Every connected component represents one friend group.
- Complexity:
  - **Time:** O(N + E) where E = total friendships.
  - **Space:** O(N) for visited set.

---

```java
import java.util.*;

public class CountFriendGroups {

    public static int countFriendGroups(int N, Map<Integer, List<Integer>> friendship) {
        Set<Integer> visited = new HashSet<>();
        int groups = 0;

        for (int student = 0; student < N; student++) {
            if (!visited.contains(student)) {
                dfs(student, friendship, visited);
                groups++;
            }
        }
        return groups;
    }

    private static void dfs(int student, Map<Integer, List<Integer>> friendship, Set<Integer> visited) {
        visited.add(student);
        for (int friend : friendship.getOrDefault(student, Collections.emptyList())) {
            if (!visited.contains(friend)) {
                dfs(friend, friendship, visited);
            }
        }
    }

    public static void main(String[] args) {
        Map<Integer, List<Integer>> friendship1 = new HashMap<>();
        friendship1.put(0, Arrays.asList(1, 2));
        friendship1.put(1, Arrays.asList(0, 5));
        friendship1.put(2, Arrays.asList(0));
        friendship1.put(3, Arrays.asList(6));
        friendship1.put(4, Arrays.asList());
        friendship1.put(5, Arrays.asList(1));
        friendship1.put(6, Arrays.asList(3));

        System.out.println(countFriendGroups(7, friendship1)); // Output: 3

        Map<Integer, List<Integer>> friendship2 = new HashMap<>();
        friendship2.put(0, Arrays.asList(1));
        friendship2.put(1, Arrays.asList(0, 2));
        friendship2.put(2, Arrays.asList(1));
        friendship2.put(3, Arrays.asList(4));
        friendship2.put(4, Arrays.asList(3));

        System.out.println(countFriendGroups(5, friendship2)); // Output: 2
    }
}
