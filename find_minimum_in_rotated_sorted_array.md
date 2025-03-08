# 153. Find Minimum in Rotated Sorted Array

Suppose an array of length n sorted in ascending order is rotated between 1 and n times. For example, the array nums = [0,1,2,4,5,6,7] might become:

[4,5,6,7,0,1,2] if it was rotated 4 times.
[0,1,2,4,5,6,7] if it was rotated 7 times.
Notice that rotating an array [a[0], a[1], a[2], ..., a[n-1]] 1 time results in the array [a[n-1], a[0], a[1], a[2], ..., a[n-2]].

Given the sorted rotated array nums of unique elements, return the minimum element of this array.

You must write an algorithm that runs in O(log n) time.

# Step 1

Binary Search 二問目

```python

class Solution:
    def findMin(self, nums: List[int]) -> int:
        left_index = 0
        right_index = len(nums) - 1
        while nums[left_index] >= nums[right_index]:
            mid = (left_index + right_index) // 2
            if nums[mid] > nums[left_index]:
                left_index = mid
            elif nums[mid] < nums[left_index]:
                right_index = mid
            else:
                return nums[right_index]
        return nums[left_index]

```

# Step 2

35. Search Insert Positionを解いたときにmidに+1したり-1したりする理由がよくわかっていなかったので、
開区間とか閉区間のことを考えつつコードを書いてみた
nums[mid]>nums[left_index]の場合にleft_index=mid+1としたのは、
その場合nums[mid]が求める最小値になることはないので、
考える区間の左端をmid+1に設定してよいから

```python

class Solution:
    def findMin(self, nums: List[int]) -> int:
        left_index=0
        right_index=len(nums)-1
        while nums[left_index]>nums[right_index]:
            mid=(left_index+right_index)//2
            if nums[mid]<nums[left_index]:
                right_index=mid
            elif nums[mid]>nums[left_index]:
                left_index=mid+1
            else:
                return nums[right_index]
        return nums[left_index]

```

# Step 3

とりあえず、以下のコードに関してはほぼ同じものがスラスラかけるようになった

```python
class Solution:
    def findMin(self, nums: List[int]) -> int:
        left_index = 0
        right_index = len(nums) - 1
        while nums[left_index] > nums[right_index]:
            mid = (left_index + right_index) // 2
            if nums[mid] < nums[left_index]:
                right_index = mid
            elif nums[mid] > nums[left_index]:
                left_index = mid + 1
            else:
                return nums[right_index]
        return nums[left_index]


```
