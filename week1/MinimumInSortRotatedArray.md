Given an array that was initially sorted in ascending order but then rotated at an unknown
pivot, write an efficient algorithm to find the minimum element in the array.
• The array contains no duplicate elements.
• Time Complexity: O(log N) 
Approach (Binary Search):
	• Initialize low = 0, high = N - 1
	• While low < high:
	o Find mid = (low + high) / 2
	o If arr[mid] > arr[high], the minimum is in the right half → low = mid + 1
	o Else, the minimum is in the left half (including mid) → high = mid
	• Return arr[low]
	
	Solution:
```java
class Solution {
    public int findMin(int[] nums) {
        int n= nums.length;
        int low=0;
        int high=n-1;
        while(low<high){
            int mid= low+ (high-low)/2;
            if(nums[mid]>nums[high]){
                low=mid+1;
            }
            else{
                high=mid;
            }
        }
        return nums[low];
    }
}
```
