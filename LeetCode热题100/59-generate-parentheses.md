# 括号生成
- **英文**: Generate Parentheses
- **难度**: 中等
- **题目**: https://leetcode.cn/problems/generate-parentheses/

## 题目描述
数字 `n` 代表生成括号的对数，请你设计一个函数，用于能够生成所有可能的并且 **有效的 **括号组合。

 

**示例 1：**

**输入：**n = 3
**输出：**["((()))","(()())","(())()","()(())","()()()"]

**示例 2：**

**输入：**n = 1
**输出：**["()"]

 

**提示：**

	
- `1 <= n <= 8`

## 代码模板
```python
class Solution:
    def generateParenthesis(self, n: int) -> List[str]:
        
```

## Python 解法
```python
class Solution:
    def generateParenthesis(self, n: int) -> List[str]:
        res = []
        def backtrack(l, r, path):
            if len(path) == 2 * n: res.append(''.join(path)); return
            if l < n: path.append('('); backtrack(l+1, r, path); path.pop()
            if r < l: path.append(')'); backtrack(l, r+1, path); path.pop()
        backtrack(0, 0, [])
        return res
```
