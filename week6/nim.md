# Problem Title  
Nim (misère version) — Determine if First Player Has a Forced Win  

**Company:** Google  

---

## Scenario & Description  
The game of Nim is played on several heaps of stones. Two players alternate turns. On a turn, a player removes one or more stones from exactly one heap.  

In this variant (**misère Nim**) the player who takes the **last stone loses**.  

Given three non-zero starting heap sizes `[a, b, c]`, decide whether the first player (the player who moves first) has a forced win assuming both players play optimally.  

---


## Key Observation (Misère Nim Theory)  

1. If **all heaps have size 1**:  
   - First player wins **iff number of heaps is even**.  
   - (If number of heaps is odd → first player loses).  

2. Otherwise (there exists a heap with size ≥ 2):  
   - Compute the **XOR (nim-sum)** of heap sizes.  
   - If `nim-sum != 0` → first player has a winning strategy.  
   - If `nim-sum == 0` → first player loses.  

---

## Algorithm  
1. Check if all heaps = 1:  
   - If yes → return **True if count is even**, else **False**.  
2. Otherwise:  
   - Compute XOR of all heap sizes.  
   - If `xor != 0` → return True.  
   - Else return False.  

**Time Complexity:** `O(1)` (constant for 3 heaps, `O(n)` for n heaps).  
**Space Complexity:** `O(1)`.  

---

## Java Implementation  

```java
import java.util.Scanner;

public class MisereNim {
    public static boolean hasFirstPlayerWin(int[] heaps) {
        // Case 1: All heaps are 1
        boolean allOnes = true;
        for (int h : heaps) {
            if (h != 1) {
                allOnes = false;
                break;
            }
        }

        if (allOnes) {
            // First player wins iff number of heaps is even
            return heaps.length % 2 == 0;
        }

        // Case 2: Standard Nim strategy
        int xor = 0;
        for (int h : heaps) {
            xor ^= h;
        }
        return xor != 0;
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int[] heaps = new int[3];
        for (int i = 0; i < 3; i++) {
            heaps[i] = sc.nextInt();
        }
        System.out.println(hasFirstPlayerWin(heaps));
    }
}
