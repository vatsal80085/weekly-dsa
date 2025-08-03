##Problem Title: Equal Sum Partition (Asked by Facebook)
####Problem Statement:
We are given a multiset (a list that can have duplicate integers). Determine whether it can be
partitioned into two subsets such that the sum of elements in both subsets is equal.

####Approach
-> If total sum is odd, it can’t be split evenly.
-> Used Dynamic Programming + memoization

```java 
class Solution {
    public boolean canPartition(int[] nums) {
        int totalSum = 0;
        for (int num : nums) {
            totalSum += num;
        }

        // If total sum is odd, can't be partitioned equally
        if (totalSum % 2 != 0) return false;

        int target = totalSum / 2;
        boolean[] dp = new boolean[target + 1];
        dp[0] = true;

        for (int num : nums) {
            for (int i = target; i >= num; i--) {
                dp[i] = dp[i] || dp[i - num];
            }
        }

        return dp[target];
    }
}
```