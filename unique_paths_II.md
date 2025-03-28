63. Unique Paths II

You are given an m x n integer array grid. There is a robot initially located at the top-left corner (i.e., grid[0][0]). The robot tries to move to the bottom-right corner (i.e., grid[m - 1][n - 1]). The robot can only move either down or right at any point in time.

An obstacle and space are marked as 1 or 0 respectively in grid. A path that the robot takes cannot include any square that is an obstacle.

Return the number of possible unique paths that the robot can take to reach the bottom-right corner.

The testcases are generated so that the answer will be less than or equal to 2 * 109.

# Step1

まずアクセプトされるコードを書く

例外処理の部分に少し不満があります...

```python

class Solution:
    def uniquePathsWithObstacles(self, obstacleGrid: List[List[int]]) -> int:
        length = len(obstacleGrid)
        width = len(obstacleGrid[0])
        num_paths = [[0] * width for _ in range(length)]
        for i in range(length):
            for j in range(width):
                if obstacleGrid[i][j] == 1:
                    continue
                if i == 0 and j == 0:
                    num_paths[i][j] = 1
                if i > 0:
                    num_paths[i][j] += num_paths[i - 1][j]
                if j > 0:
                    num_paths[i][j] += num_paths[i][j - 1]
        return num_paths[-1][-1]

```


# Step2

ほかの人のコードを見る

https://github.com/olsen-blue/Arai60/pull/34/files

https://github.com/philip82148/leetcode-arai60/pull/10/files

二次元配列の変数名どうする問題がありますね。

しかし、解き方の基本的なところはみんな同じなのかなと思いました。


今回は、例外処理のところをどうするか気になったのでChatGPTにも聞いてみました。

やっぱり読みやすいコードだと思います。

```python

class Solution:
    def uniquePathsWithObstacles(self, obstacleGrid: List[List[int]]) -> int:
        m = len(obstacleGrid)
        n = len(obstacleGrid[0])
        
        # スタートまたはゴールが障害物なら経路は存在しない
        if obstacleGrid[0][0] == 1 or obstacleGrid[m-1][n-1] == 1:
            return 0
        
        # 二次元DPテーブルを初期化
        dp = [[0] * n for _ in range(m)]
        dp[0][0] = 1  # スタート地点の初期値
        
        # グリッドを走査して各セルへの経路数を計算
        for i in range(m):
            for j in range(n):
                # 現在のセルが障害物なら経路数は0（すでに初期化済み）
                if obstacleGrid[i][j] == 1:
                    dp[i][j] = 0
                else:
                    # 上からの経路があれば加算
                    if i > 0:
                        dp[i][j] += dp[i-1][j]
                    # 左からの経路があれば加算
                    if j > 0:
                        dp[i][j] += dp[i][j-1]
                        
        # 右下端のセルの値が全経路数となる
        return dp[m-1][n-1]

```

私としての完成コードはこんな感じです。

```python
if obstacleGrid[i][j]:
    continue
```
の部分がチャーミングだと思っています。

```python

class Solution:
    def uniquePathsWithObstacles(self, obstacleGrid: List[List[int]]) -> int:
        rows = len(obstacleGrid)
        cols = len(obstacleGrid[0])
        num_paths = [[0] * cols for _ in range(rows)]
        if obstacleGrid[0][0]:
            return 0
        num_paths[0][0] = 1
        for i in range(rows):
            for j in range(cols):
                if obstacleGrid[i][j]:
                    continue
                if i > 0:
                    num_paths[i][j] += num_paths[i - 1][j]
                if j > 0:
                    num_paths[i][j] += num_paths[i][j - 1]
        return num_paths[-1][-1]

```

# Step 3

写経

もう何回書いても同じになりそう

```python

class Solution:
    def uniquePathsWithObstacles(self, obstacleGrid: List[List[int]]) -> int:
        rows = len(obstacleGrid)
        cols = len(obstacleGrid[0])
        num_paths = [[0] * cols for _ in range(rows)]
        if obstacleGrid[0][0]:
            return 0
        num_paths[0][0] = 1
        for i in range(rows):
            for j in range(cols):
                if obstacleGrid[i][j]:
                    continue
                if i > 0:
                    num_paths[i][j] += num_paths[i - 1][j]
                if j > 0:
                    num_paths[i][j] += num_paths[i][j - 1]
        return num_paths[-1][-1]

```
