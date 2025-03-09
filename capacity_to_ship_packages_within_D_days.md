# 1011 Capacity to Ship Packages Within D Days

A conveyor belt has packages that must be shipped from one port to another within days days.

The ith package on the conveyor belt has a weight of weights[i]. Each day, we load the ship with packages on the conveyor belt (in the order given by weights). We may not load more weight than the maximum weight capacity of the ship.

Return the least weight capacity of the ship that will result in all the packages on the conveyor belt being shipped within days days.

#Step 1

とりあえず通るコードを書く

正直言って、深い考察はせず、二分探索の問題ということが知られていたので二分探索を選びました。

しかし、capacityの数直線みたいなものを考えたとき[false, false, ..., false, true, ..., true]になってるのはわかるので、この形式だと二分探索が使えるということなんだと理解しています。

そのtrueとfalseを見分ける関数として、allPackagesCanBeShippedWith(capacity)を作っておくと二分探索の部分のコードが単純になりそうだったので、こういう形式にしています。

このコードの計算量は、n=len(weights), m=sum(weights)として、O(n * log m)です。



```python

class Solution:
    def shipWithinDays(self, weights: List[int], days: int) -> int:
        def allPackagesCanBeShippedWith(capacity):
            daysRequired = 0
            capacityLeft = 0
            for weight in weights:
                if capacityLeft >= weight:
                    capacityLeft -= weight
                else:
                    daysRequired += 1
                    capacityLeft = capacity - weight
            return daysRequired <= days

        lowerBound = max(weights)
        upperBound = sum(weights)

        while lowerBound < upperBound:
            mid = (lowerBound + upperBound) // 2
            if allPackagesCanBeShippedWith(mid):
                upperBound = mid
            else:
                lowerBound = mid + 1
        return lowerBound

```


#Step2

https://github.com/Ryotaro25/leetcode_first60/pull/51/files
など読みましたが、ほぼ同じ内容でびっくりしました。

https://github.com/fhiyo/leetcode/pull/45/files
これもそうですね。

私の中では、二分探索の勉強はひと段落しつつあるということになったので、この問題は変数名など整えて清書して、さらっと流すことにしました。

ChatGPTがpythonはスネークケースが良いと教えてくれたのでこれからそうします
→よく考えたら問題でshipWithinDaysが与えられているので、それと統一するという考え方もありますね

```python

class Solution:
    def shipWithinDays(self, weights: List[int], days: int) -> int:
        def can_ship_with_capacity(capacity):
            required_days = 0
            remaining_capacity = 0
            for weight in weights:
                if remaining_capacity >= weight:
                    remaining_capacity -= weight
                else:
                    required_days += 1
                    remaining_capacity = capacity - weight
            return required_days <= days

        min_capacity = max(weights)
        max_capacity = sum(weights)

        while min_capacity < max_capacity:
            mid_capacity = (min_capacity + max_capacity) // 2
            if can_ship_with_capacity(mid_capacity):
                max_capacity = mid_capacity
            else:
                min_capacity = mid_capacity + 1
        return min_capacity

```

