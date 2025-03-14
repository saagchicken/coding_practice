62. Unique Paths

There is a robot on an m x n grid. The robot is initially located at the top-left corner (i.e., grid[0][0]). The robot tries to move to the bottom-right corner (i.e., grid[m - 1][n - 1]). The robot can only move either down or right at any point in time.

Given the two integers m and n, return the number of possible unique paths that the robot can take to reach the bottom-right corner.

The test cases are generated so that the answer will be less than or equal to 2 * 109.

# Step 1

まずアクセプトされるコードを書く

nC2で計算できることはわかるが、プログラミングの問題なので動的計画法で解こう

失敗例

list * n
のような操作をするとき、同じポインターを複製してしまって、同じ内容のリストのコピーを作成してくれない場合があるらしい。

巨大な結果が出てきて10分くらいバグが直せず、とてもつらい気持ちになりました。


```python

class Solution:
    def uniquePaths(self, m: int, n: int) -> int:
        dp_paths = [[0] * m] * n
        dp_paths[0][0] = 1
        for i in range(m):
            for j in range(n):
                if i > 0:
                    dp_paths[j][i] += dp_paths[j][i - 1]
                if j > 0:
                    dp_paths[j][i] += dp_paths[j - 1][i]
        return dp_paths[-1][-1]

```

これはAC

内包表記というものがあるらしい

```python

class Solution:
    def uniquePaths(self, m: int, n: int) -> int:
        dp_paths = [[0] * m for _ in range(n)]
        dp_paths[0][0] = 1
        for i in range(m):
            for j in range(n):
                if i > 0:
                    dp_paths[j][i] += dp_paths[j][i - 1]
                if j > 0:
                    dp_paths[j][i] += dp_paths[j - 1][i]
        return dp_paths[-1][-1]

```

# Step2

ほかの人のPRを読む

https://github.com/Ryotaro25/leetcode_first60/pull/39/files

https://github.com/hroc135/leetcode/pull/31/files

https://github.com/philip82148/leetcode-arai60/pull/9/files

https://github.com/rossy0213/leetcode/pull/19/files

しかしまあ解き方としてはほかの人とあんまり変わらないかな？

コンビネーションを使うやり方や、一次元dpで空間計算量をO(mn)からO(m+n)に減らす方法などがあるが、個人的にはこのやり方が教育上良いと思った。




