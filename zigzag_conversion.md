6. zigzag conversion

The string "PAYPALISHIRING" is written in a zigzag pattern on a given number of rows like this: (you may want to display this pattern in a fixed font for better legibility)

P   A   H   N
A P L S I I G
Y   I   R
And then read line by line: "PAHNAPLSIIGYIR"

Write the code that will take a string and make this conversion given a number of rows:

string convert(string s, int numRows);

# Step 1

まずはとりあえずアクセプトされるコードを書く

この問題を解いた目的の一つとして、generatorの練習をしたかったんです。
なので、generatorを使ったまあある種直感的な解法がこんな感じです。
position_generator()の実装部分がやや複雑ですが、それ以外の部分を読むと、問題文で言われた通りのことをしていると思います。

```python

class Solution:
    def convert(self, s: str, numRows: int) -> str:
        if numRows == 1:
            return s

        display_board = [[""] * len(s) for _ in range(numRows)]

        def position_generator():
            current_position = [0, 0]
            yield current_position
            while True:
                while current_position[0] < numRows - 1:
                    current_position[0] += 1
                    yield current_position
                while current_position[0] > 0:
                    current_position[0] -= 1
                    current_position[1] += 1
                    yield current_position

        positions = position_generator()
        for ch in s:
            position = positions.__next__()
            display_board[position[0]][position[1]] = ch
        return "".join(["".join(line) for line in display_board])

```

# Step 2

ほかの人のPRを読む

javaだとStringBuilderというオブジェクトがあるんですね。
Pythonだと、ストリングのリストを持って、concatenateするのかなと思います。

https://github.com/katsukii/leetcode/pull/7/files

長文が発生していてびっくりした。
C++のincludeを知らないので議論の内容は理解できなかった。

https://github.com/philip82148/leetcode-arai60/pull/5/files

まあ要するにジグザグ部分をどう処理するのかが問題で、私はgeneratorを使ってみたけど、ほかの人はrowとdirectionを持ってうまいこと動かすなど、そこが工夫のしどころみたいですね。

そうこう言ってるうちにかなり良い解法を思いついたので実装します。

rowの情報をうまいことgenerateする方法です。

```python

class Solution:
    def convert(self, s: str, numRows: int) -> str:
        if numRows == 1:
            return s

        def rows_generator():
            row = 0
            while True:
                while row < numRows - 1:
                    yield row
                    row += 1
                while row > 0:
                    yield row
                    row -= 1

        display = [[] for _ in range(numRows)]
        rows = rows_generator()
        for ch in s:
            display[rows.__next__()].append(ch)
        return "".join(["".join(line) for line in display])

```

私としては、generatorの練習もしっかりできて、納得のいくコードを書けたので、今回は写経はしません。
すみません。
