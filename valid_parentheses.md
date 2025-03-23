20. Valid Parentheses

Given a string s containing just the characters '(', ')', '{', '}', '[' and ']', determine if the input string is valid.

An input string is valid if:

Open brackets must be closed by the same type of brackets.
Open brackets must be closed in the correct order.
Every close bracket has a corresponding open bracket of the same type.

# Step 1

とりあえずアクセプトされるコードを書く

Stackを使えば解けるのはわかる

Pythonでstackを使うには、listかdequeかの二択になりそう

まずはモジュールのかかわらないlistでやってみよう

何回かエラーが出たため、複数回提出してAC

```python

class Solution:
    def isValid(self, s: str) -> bool:
        still_open_brackets = []
        bracket_pairs = {"(": ")", "{": "}", "[": "]"}
        for ch in s:
            if ch in ("(", "{", "["):
                still_open_brackets.append(ch)
            else:
                try:
                    most_recent_open_bracket = still_open_brackets.pop(-1)
                except:
                    return False
                if ch != bracket_pairs[most_recent_open_bracket]:
                    return False
        if len(still_open_brackets) > 0:
            return False
        return True

```

# Step 2

ほかの人のPRを読みコード改善

Fuminitonさんのプルリクが勉強になった
会話が多いため
https://github.com/Fuminiton/LeetCode/pull/6/files

https://github.com/KTakao01/leetcode/pull/5/files

いくつか変数名変えたり、同値な変形をしています。

しかし、今回のcontinueは個人的にはあんまりわかりやすくない気がしています。

if bracket in "({["とそうでない場合は対等なイメージのため。



```python

class Solution:
    def isValid(self, s: str) -> bool:
        still_open_brackets = []
        bracket_pairs = {"(": ")", "{": "}", "[": "]"}
        for bracket in s:
            if bracket in "({[":
                still_open_brackets.append(bracket)
                continue
            if len(still_open_brackets) == 0:
                return False
            if bracket != bracket_pairs[still_open_brackets.pop()]:
                return False
        return len(still_open_brackets) == 0

```

次に、dequeの練習をします。

まずdequeとは何か？右からも左からもpopできる便利なデータ構造という理解ですが、ドキュメント的にはこういうことらしい。

「このモジュールは、汎用の Python 組み込みコンテナ dict, list, set, および tuple に代わる、特殊なコンテナデータ型を実装しています。」

とくにdequeは、「両端における append や pop を高速に行えるリスト風のコンテナ」

https://docs.python.org/ja/3.13/library/collections.html

ソーソコードがようやくわかった

私が
from collections import deque
と書くとき、まずpythonのcollectionsモジュールを参照して、それがCで書かれた_collectionsモジュールを参照するという建付けらしい。

結構大きな発見

https://github.com/python/cpython/blob/3.13/Lib/collections/__init__.py

https://github.com/python/cpython/blob/3.13/Modules/_collectionsmodule.c

バグもなく一発AC

```python

class Solution:
    from collections import deque

    def isValid(self, s: str) -> bool:
        open_brackets = deque()
        bracket_pairs = {"(": ")", "{": "}", "[": "]"}
        for bracket in s:
            if bracket in "({[":
                open_brackets.append(bracket)
                continue
            try:
                recent_open_bracket = open_brackets.pop()
                if bracket != bracket_pairs[recent_open_bracket]:
                    return False
            except:
                return False
        return len(open_brackets) == 0

```

ところで、ChatGPTに聞いて面白い発見をしました

まず解答

```python

from collections import deque

class Solution:
    def isValid(self, s: str) -> bool:
        # 括弧の対応関係を定義（閉じ括弧をキー、対応する開き括弧を値として）
        mapping = {')': '(', '}': '{', ']': '['}
        stack = deque()
        
        for char in s:
            # 開き括弧の場合はstackに追加
            if char in mapping.values():
                stack.append(char)
            # 閉じ括弧の場合
            elif char in mapping:
                # stackが空であったり、直前の開き括弧と対応していなければ無効
                if not stack or stack.pop() != mapping[char]:
                    return False
            else:
                # 問題文にない文字が現れた場合（本問題では発生しない前提）
                return False
        
        # 全ての括弧が正しく閉じられていればstackは空になっている
        return not stack

```

この
if not stack or stack.pop() != mapping[char]:
の部分でpopができないとエラーが出るのではないかと思って聞いたところ、

「Pythonでは、論理演算子orは短絡評価（short-circuit evaluation）を行うため、
左側の条件がTrueの場合、右側の条件は評価されません。つまり、
if not stack or stack.pop() != mapping[char]:の部分では、
stackが空の場合、not stackがTrueとなり、stack.pop()は実行されず、
そのままTrueとしてreturn Falseが実行されます。これにより、
空のスタックに対してpop()を呼び出してエラーが発生することを防いでいます。」

とのことです。


# Step 3

三回解きます。

```python

class Solution:
    def isValid(self, s: str) -> bool:
        open_brackets = []
        bracket_pairs = {"(": ")", "{": "}", "[": "]"}
        for bracket in s:
            if bracket in "({[":
                open_brackets.append(bracket)
            elif not open_brackets or bracket_pairs[open_brackets.pop()] != bracket:
                return False
        return not open_brackets

```

elif not open_brackets or bracket_pairs[open_brackets.pop()] != bracket:
の行を読むのにかなり負荷がかかるが、これは私の読解力の問題かな？
コードの構造としては、これが美しいと思うけど。

考えたのですが、やっぱり私としてはこれが「覚えられるコード」ということで、これを写経することにしました。

```python

class Solution:
    def isValid(self, s: str) -> bool:
        open_brackets = []
        bracket_pairs = {"(": ")", "{": "}", "[": "]"}
        for bracket in s:
            if bracket in "({[":
                open_brackets.append(bracket)
            elif not open_brackets or bracket != bracket_pairs[open_brackets.pop()]:
                return False
        return not open_brackets

```

```python

class Solution:
    def isValid(self, s: str) -> bool:
        open_brackets = []
        bracket_pairs = {"(": ")", "{": "}", "[": "]"}
        for bracket in s:
            if bracket in "({[":
                open_brackets.append(bracket)
                continue
            if not open_brackets or bracket != bracket_pairs[open_brackets.pop()]:
                return False
        return not open_brackets

```
