## Problem Statement: Deep Clone a Linked List with Random Pointer
The question asked by Snapchat

## Problem Description:
**Scenario:**
You are given a singly linked list where each node contains two pointers:
- `next`: Points to the next node in the list.
- `random`: Points to any node in the list (or `null`).

The task is to **create a deep copy** of this list. That means I should create a new list where:
- Each node is a **new object**.
- Each node has the same `val`, `next`, and `random` structure as the original.
- The cloned list must be completely independent (changes in the original do not affect the clone).

---

### Approach Used:
I Have solved this in **two main ways**:

1. **Using HashMap (O(N) space):**
   - Traverse the list and clone all nodes.
   - Store mapping: `original → clone`.
   - Second pass: set `next` and `random` pointers using the map.

2. **O(1) Space (Interleaving Method):**
   - Insert cloned nodes between original nodes.
   - Assign random pointers using `original.random.next`.
   - Split the interleaved list into original + clone.

---

### Key Observations:
- The tricky part is handling the `random` pointers correctly.
- O(1) space approach is interview-friendly but slightly more complex.
- O(N) space HashMap approach is simpler and easier to explain.

---

```java
import java.util.*;

class Node {
    int val;
    Node next;
    Node random;

    Node(int val) {
        this.val = val;
        this.next = null;
        this.random = null;
    }
}

public class DeepCloneLinkedList {

    // Approach 1: Using HashMap (O(N) space)
    public static Node copyRandomList(Node head) {
        if (head == null) return null;

        Map<Node, Node> map = new HashMap<>();

        // 1. Clone all nodes
        Node curr = head;
        while (curr != null) {
            map.put(curr, new Node(curr.val));
            curr = curr.next;
        }

        // 2. Assign next and random
        curr = head;
        while (curr != null) {
            Node clone = map.get(curr);
            clone.next = map.get(curr.next);
            clone.random = map.get(curr.random);
            curr = curr.next;
        }

        return map.get(head);
    }

    // Approach 2: O(1) space (interleaving)
    public static Node copyRandomListOptimized(Node head) {
        if (head == null) return null;

        Node curr = head;

        // 1. Interleave cloned nodes
        while (curr != null) {
            Node clone = new Node(curr.val);
            clone.next = curr.next;
            curr.next = clone;
            curr = clone.next;
        }

        // 2. Assign random pointers
        curr = head;
        while (curr != null) {
            if (curr.random != null) {
                curr.next.random = curr.random.next;
            }
            curr = curr.next.next;
        }

        // 3. Separate the lists
        curr = head;
        Node cloneHead = head.next;
        while (curr != null) {
            Node clone = curr.next;
            curr.next = clone.next;
            if (clone.next != null) {
                clone.next = clone.next.next;
            }
            curr = curr.next;
        }

        return cloneHead;
    }

    public static void main(String[] args) {
        // Example: Build a list [7 -> 13 -> 11 -> 10 -> 1] with random pointers
        Node n1 = new Node(7);
        Node n2 = new Node(13);
        Node n3 = new Node(11);
        Node n4 = new Node(10);
        Node n5 = new Node(1);

        n1.next = n2; n2.next = n3; n3.next = n4; n4.next = n5;

        n2.random = n1;
        n3.random = n5;
        n4.random = n3;
        n5.random = n1;

        Node cloneHead = copyRandomListOptimized(n1);
        System.out.println("Original Head: " + n1.val);
        System.out.println("Cloned Head: " + cloneHead.val);
    }
}