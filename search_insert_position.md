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
-最初のrecursionを使ったやり方だと空間計算量が悪いことに気づいたので、
-while文を使って書いてみました

```python
class Solution:
    def searchInsert(self, nums: List[int], target: int) -> int:
        left_index=0
        right_index=len(nums)-1
        while True:
            if target<=nums[left_index]:
                return left_index
            if target==nums[right_index]:
                return right_index
            if target>nums[right_index]:
                return right_index+1
            mid=(left_index+right_index)//2
            if mid==left_index:
                return right_index
            if target==nums[mid]:
                return mid
            if target>nums[mid]:
                left_index=mid
            else:
                right_index=mid
```




## Step3

-LeetCodeの答えとしても、CharGPTに聞いた答えとしても、以下のようなコードが返ってきたので、これを三回写経しました

```python
class Solution:
    def searchInsert(self, nums: List[int], target: int) -> int:
        left_index=0
        right_index=len(nums)-1

        while left_index<=right_index:
            mid=(left_index+right_index)//2
            if nums[mid]==target:
                return mid
            elif nums[mid]<target:
                left_index=mid+1
            else:
                right_index=mid-1
        return left_index
```

