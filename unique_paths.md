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

二重forループの中でif分岐をするより外側で片方のindexが0の場合に1を埋めてしまったほうがいいと思ったので、そこだけ変更。

```python

class Solution:
    def uniquePaths(self, m: int, n: int) -> int:
        dp_paths = [[0] * n for _ in range(m)]
        for i in range(m):
            dp_paths[i][0] = 1
        for j in range(n):
            dp_paths[0][j] = 1
        for i in range(1, m):
            for j in range(1, n):
                dp_paths[i][j] = dp_paths[i - 1][j] + dp_paths[i][j - 1]
        return dp_paths[-1][-1]

```

# Step3

動的計画法も慣れていない気がするので、素直に三回写経します

空間計算量をO(m+n)に抑えられるという指摘は当然あると思うのですが、条件が1 <= m, n <= 100なので二次元メモを持つ方法も悪くないかなと思っています。

やってみて思うのですが、この問題、二次元メモを使うという条件の下では、ほとんどほかの自然な書き方が無いですね（少なくともそのように感じるようになりました）。

最初のコードを見ると、どうしてこんなに酷いものが生成されたのか不思議です。


```python

class Solution:
    def uniquePaths(self, m: int, n: int) -> int:
        num_paths = [[0] * n for _ in range(m)]
        for i in range(m):
            num_paths[i][0] = 1
        for j in range(n):
            num_paths[0][j] = 1
        for i in range(1, m):
            for j in range(1, n):
                num_paths[i][j] = num_paths[i - 1][j] + num_paths[i][j - 1]
        return num_paths[-1][-1]

```

```python

class Solution:
    def uniquePaths(self, m: int, n: int) -> int:
        num_paths = [[0] * n for _ in range(m)]
        for i in range(m):
            num_paths[i][0] = 1
        for j in range(n):
            num_paths[0][j] = 1
        for i in range(1, m):
            for j in range(1, n):
                num_paths[i][j] = num_paths[i - 1][j] + num_paths[i][j - 1]
        return num_paths[-1][-1]

```

改善の余地なしだと思ってぼーっと写経していたら、メモの初期値は1でいいことに気づきました。

```python

class Solution:
    def uniquePaths(self, m: int, n: int) -> int:
        dp_paths = [[1] * n for _ in range(m)]
        for i in range(1, m):
            for j in range(1, n):
                dp_paths[i][j] = dp_paths[i][j - 1] + dp_paths[i - 1][j]
        return dp_paths[-1][-1]

```
