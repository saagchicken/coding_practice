# 33 Search in Rotated Sorted Array

There is an integer array nums sorted in ascending order (with distinct values).

Prior to being passed to your function, nums is possibly rotated at an unknown pivot index k (1 <= k < nums.length) such that the resulting array is [nums[k], nums[k+1], ..., nums[n-1], nums[0], nums[1], ..., nums[k-1]] (0-indexed). For example, [0,1,2,4,5,6,7] might be rotated at pivot index 3 and become [4,5,6,7,0,1,2].

Given the array nums after the possible rotation and an integer target, return the index of target if it is in nums, or -1 if it is not in nums.

You must write an algorithm with O(log n) runtime complexity.

# Step1


```python

class Solution:
    def search(self, nums: List[int], target: int) -> int:

        def targetCanBeInThisInterval(a, b):
            if nums[a] < nums[b]:
                return (nums[a] <= target) & (target <= nums[b])
            if nums[a] > nums[b]:
                return (nums[a] <= target) | (target <= nums[b])
            else:
                return target == nums[a]

        left = 0
        right = len(nums) - 1
        while targetCanBeInThisInterval(left, right):
            if nums[left] == target:
                return left
            if nums[right] == target:
                return right
            mid = (left + right) // 2
            if nums[mid] == target:
                return mid
            if targetCanBeInThisInterval(left, mid):
                right = mid
            else:
                left = mid+1
        return -1

```
