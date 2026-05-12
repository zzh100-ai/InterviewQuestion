# N 皇后
- **英文**: N-Queens
- **难度**: 困难
- **题目**: https://leetcode.cn/problems/n-queens/

## 题目描述
按照国际象棋的规则，皇后可以攻击与之处在同一行或同一列或同一斜线上的棋子。

**n 皇后问题** 研究的是如何将 `n` 个皇后放置在 `n×n` 的棋盘上，并且使皇后彼此之间不能相互攻击。

给你一个整数 `n` ，返回所有不同的 **n* *皇后问题** 的解决方案。

每一种解法包含一个不同的 **n 皇后问题** 的棋子放置方案，该方案中 `'Q'` 和 `'.'` 分别代表了皇后和空位。

 

**示例 1：**

**输入：**n = 4
**输出：**[[".Q..","...Q","Q...","..Q."],["..Q.","Q...","...Q",".Q.."]]
**解释：**如上图所示，4 皇后问题存在两个不同的解法。

**示例 2：**

**输入：**n = 1
**输出：**[["Q"]]

 

**提示：**

	
- `1

## 代码模板
```python
class Solution:
    def solveNQueens(self, n: int) -> List[List[str]]:
        
```

## Python 解法
```python
class Solution:
    def solveNQueens(self, n: int) -> List[List[str]]:
        res, cols, d1, d2 = [], set(), set(), set()
        board = [['.']*n for _ in range(n)]
        def backtrack(row):
            if row == n: res.append([''.join(r) for r in board]); return
            for col in range(n):
                if col in cols or (row+col) in d1 or (row-col) in d2: continue
                board[row][col] = 'Q'
                cols.add(col); d1.add(row+col); d2.add(row-col)
                backtrack(row+1)
                cols.remove(col); d1.remove(row+col); d2.remove(row-col)
                board[row][col] = '.'
        backtrack(0)
        return res
```
