# Problem of the Week – Swap Even and Odd Bits  
Company: Cisco  
Difficulty: Medium  
Topic: Bit Manipulation  

## Problem Statement  
Given an unsigned 8-bit integer n (0 ≤ n ≤ 255), swap every even-positioned bit with its adjacent odd-positioned bit and return the result.  

### Approaches  
1. Brute Force Bit-by-Bit Swap  
   - Loop through pairs of bits, swap manually. O(8) constant time.  

2. Efficient Bitmasking Approach  
   - Use masks: even mask = 0xAA (10101010), odd mask = 0x55 (01010101).  
   - Shift even bits right, odd bits left, combine: `((n & 0xAA) >> 1) | ((n & 0x55) << 1)`.  

## Java Code  
```java
import java.util.*;

public class SwapEvenOddBits {
    public static int swapBits(int n) {
        return ((n & 0xAA) >> 1) | ((n & 0x55) << 1);
    }
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt();
        System.out.println(swapBits(n));
    }
}
