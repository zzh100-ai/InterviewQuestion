# 单词搜索
- **英文**: Word Search
- **难度**: 中等
- **题目**: https://leetcode.cn/problems/word-search/

## 题目描述
给定一个 `m x n` 二维字符网格 `board` 和一个字符串单词 `word` 。如果 `word` 存在于网格中，返回 `true` ；否则，返回 `false` 。

单词必须按照字母顺序，通过相邻的单元格内的字母构成，其中“相邻”单元格是那些水平相邻或垂直相邻的单元格。同一个单元格内的字母不允许被重复使用。

 

**示例 1：**

**输入：**board = [['A','B','C','E'],['S','F','C','S'],['A','D','E','E']], word = "ABCCED"
**输出：**true

**示例 2：**

**输入：**board = [['A','B','C','E'],['S','F','C','S'],['A','D','E','E']], word = "SEE"
**输出：**true

**示例 3：**

**输入：**board = [['A','B','C','E'],['S','F','C','S'],['A','D','E','E']], word = "ABCB"
**输出：**false

 

**提示：**

	
- `m == board.length`
	
- `n = board[i].length`
	
- `1 <= m, n <= 6`
	
- `1 <= word.length <= 15`
	
- `board` 和 `word` 仅由大小写英文字母组成

 

**进阶：**你可以使用搜索剪枝的技术来优化解决方案，使其在 `board` 更大的情况下可以更快解决问题？

## 代码模板
```python
class Solution:
    def exist(self, board: List[List[str]], word: str) -> bool:
        
```

## Python 解法
```python
class Solution:
    def exist(self, board: List[List[str]], word: str) -> bool:
        m, n = len(board), len(board[0])
        def dfs(i, j, k):
            if k == len(word): return True
            if i<0 or i>=m or j<0 or j>=n or board[i][j]!=word[k]: return False
            tmp = board[i][j]; board[i][j] = '#'
            found = dfs(i+1,j,k+1) or dfs(i-1,j,k+1) or dfs(i,j+1,k+1) or dfs(i,j-1,k+1)
            board[i][j] = tmp
            return found
        for i in range(m):
            for j in range(n):
                if dfs(i, j, 0): return True
        return False
```
