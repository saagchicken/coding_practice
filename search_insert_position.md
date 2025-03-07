# 35. Search Insert Position

## Step1

-Given a sorted array of distinct integers and a target value, 
-return the index if the target is found. 
-If not, return the index where it would be if it were inserted in order.
-You must write an algorithm with O(log n) runtime complexity.


```python
class Solution:
    def searchInsert(self, nums: List[int], target: int) -> int:
        def binarySearch(left: int, right: int) -> int:
            if nums[left] > target:
                return left
            if nums[left] == target:
                return left
            if nums[right] < target:
                return right + 1
            if nums[right] == target:
                return right
            if left == right:
                return left
            mid = (left + right) // 2
            if mid == left:
                return right
            if nums[mid] == target:
                return mid
            if nums[mid] < target:
                return binarySearch(mid, right)
            else:
                return binarySearch(left, mid)

        return binarySearch(0, len(nums) - 1)
```

## Step2




## Step3
