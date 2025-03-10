# 33. Search in Rotated Sorted Array

There is an integer array nums sorted in ascending order (with distinct values).

Prior to being passed to your function, nums is possibly rotated at an unknown pivot index k (1 <= k < nums.length) such that the resulting array is [nums[k], nums[k+1], ..., nums[n-1], nums[0], nums[1], ..., nums[k-1]] (0-indexed). For example, [0,1,2,4,5,6,7] might be rotated at pivot index 3 and become [4,5,6,7,0,1,2].

Given the array nums after the possible rotation and an integer target, return the index of target if it is in nums, or -1 if it is not in nums.

You must write an algorithm with O(log n) runtime complexity.

# Step1
まずアクセプトされるコードを書く

二分探索を、 [false, false, false, ..., false, true, true, ture, ..., true] と並んだ配列があったとき、 false と true の境界の位置を求める問題、または一番左の true の位置を求める問題と捉えているか？
→targetあるいはtargetが入るべき場所の要素を一番左のtrueと考える

位置を求めるにあたり、答えが含まれる範囲を狭めていく問題と捉えているか？
範囲を考えるにあたり、閉区間・開区間・半開区間の違いを理解できているか？
→バリエーションは理解していないが、とりあえず今回は閉区間で考える。

用いた区間の種類に対し、適切な初期値を、理由を理解したうえで、設定できるか？
→そのため、left=0, right=len(nums)-1から始める。

用いた区間の種類に対し、適切なループ不変条件を、理由を理解したうえで、設定できるか？
→閉区間なので、要素が一つだけになったときにそれが答えという考え方で行く。したがって、不変条件は、while left<right

用いた区間の種類に対し、範囲を狭めるためのロジックを、理由を理解したうえで、適切に記述できるか？
→midがtrueならright=mid, midがfalseならleft=mid+1

```python

class Solution:
    def search(self, nums: List[int], target: int) -> int:

        def targetCanBeInThisInterval(left_closed, right_closed):
            if nums[left_closed] <= nums[right_closed]:
                return (nums[left_closed] <= target) & (target <= nums[right_closed])
            else:
                return (nums[left_closed] <= target) | (target <= nums[right_closed])

        left = 0
        right = len(nums) - 1

        while left < right:
            mid = (left + right) // 2

            if targetCanBeInThisInterval(left, mid):
                right = mid
            else:
                left = mid + 1

        if nums[left] == target:
            return left
        else:
            return -1

```






# Step2
ほかの人のコードを読みながら、読みやすいコードを書く

これ(https://github.com/Mike0121/LeetCode/pull/45/files)などを読んだ

回転の切れ目を見つけて、ソートされてる部分についてサーチする方法は賢いと思った。
しかし、たぶん二倍くらい遅いのかな？

この問題でbisectを使うには、keyをうまくクラフトする必要がありそう

二分探索のコードを書く練習として、回転の切れ目を見つけて、ソートされてる部分についてサーチする方法を使ってみます

```python

class Solution:
    def search(self, nums: List[int], target: int) -> int:
        left = 0
        right = len(nums) - 1
        while left < right:
            if nums[left] <= nums[right]:
                right = left
            else:
                mid = (left + right) // 2
                if nums[mid] >= nums[left]:
                    left = mid + 1
                else:
                    right = mid
        min = left

        if min == 0:
            left = 0
            right = len(nums) - 1
        elif target < nums[0]:
            left = min
            right = len(nums) - 1
        else:
            left = 0
            right = min - 1

        while left < right:
            mid = (left + right) // 2
            if nums[mid] >= target:
                right = mid
            else:
                left = mid + 1
        if nums[left] == target:
            return left
        return -1

```





# Step3
時間を測って三回清書する

閉区間の二分探索についてはスラスラかけるようになったと思う

ちょっと変数名など細部変えてるが、このコードに関してはもうフォーマット除いて一言一句同じものを書くこともできそう


```python


class Solution:
    def search(self, nums: List[int], target: int) -> int:

        def targetCanBeInThisInterval(leftClosedEnd, rightClosedEnd):
            if nums[leftClosedEnd] <= nums[rightClosedEnd]:
                return (nums[leftClosedEnd] <= target) & (
                    target <= nums[rightClosedEnd]
                )
            return (nums[leftClosedEnd] <= target) | (target <= nums[rightClosedEnd])

        left = 0
        right = len(nums) - 1

        while left < right:
            mid = (left + right) // 2
            if targetCanBeInThisInterval(left, mid):
                right = mid
            else:
                left = mid + 1
        if nums[left] == target:
            return left
        return -1



```

# 復習

33. Search in Rotated Sorted Arrayの復習です。

閉区間が好きなので今回も閉区間で解いています。

学んだこととしては、pythonにおける関数命名のマナーや、論理記号などについてです。

```python

class Solution:
    def search(self, nums: List[int], target: int) -> int:
        def target_can_be_in_this_interval(left_closed_end, right_closed_end):
            if nums[left_closed_end] <= nums[right_closed_end]:
                return (
                    nums[left_closed_end] <= target and target <= nums[right_closed_end]
                )
            else:
                return (
                    nums[left_closed_end] <= target or target <= nums[right_closed_end]
                )

        left = 0
        right = len(nums) - 1
        while left < right:
            mid = (left + right) // 2
            if target_can_be_in_this_interval(left, mid):
                right = mid
            else:
                left = mid + 1
        if nums[left] == target:
            return left
        else:
            return -1

```
