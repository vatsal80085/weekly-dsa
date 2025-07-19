Given an integer array nums representing the calories burned each day, and an integer k
representing a target calorie goal, return the total number of continuous subarrays whose
sum is exactly equal to k.
Input Format:
• An integer n — the number of days.
• An array nums of n integers — calories burned each day.
• An integer k — the target calorie burn.
Output Format:
• A single integer — the total number of continuous subarrays whose sum equals k.

Approach:
	Brute Force Approach — Nested Loops
		Loop through all possible subarrays using two nested loops:
		Outer loop picks the starting index i
		Inner loop picks the ending index j and calculates the sum from i to j
		If the subarray sum equals k, increment the result count.
```java
public class Solution {
	public int subarraySum(int[] nums, int k) {
		int count = 0;
		
		for (int i = 0; i < nums.length; i++) {
		int sum = 0;
			for (int j = i; j < nums.length; j++) {
		        sum += nums[j];
				if (sum == k) {
		            count++;
		        }
		    }
		}
		return count;
	}
}```

	Efficient Approach — Prefix Sum + HashMap (Time: O(n), Space: O(n))
	1. Use a variable sum to track the running sum.
	2. Use a HashMap to store how many times each prefix sum has occurred.
	3. For each element:
	o Add it to the running sum.
	o Check if sum - k is already in the map — if yes, add the count to the result.
	o Update the map with the new sum.
	
	Key Insight:
	If:
	sum(i to j) = sum(j) - sum(i-1)
	Then for subarrays ending at j, we check if (sum - k) was seen before.
	
```java
class Solution {
    public int subarraySum(int[] nums, int k) {
        HashMap<Integer, Integer> prefixSumCount = new HashMap<>();
        prefixSumCount.put(0, 1);
        int currSum = 0;
        int count = 0;

        for (int num : nums) {
            currSum += num;
            if (prefixSumCount.containsKey(currSum - k)) {
                count += prefixSumCount.get(currSum - k);
            }

            prefixSumCount.put(currSum, prefixSumCount.getOrDefault(currSum, 0) + 1);
        }
        return count;
    }
}
```