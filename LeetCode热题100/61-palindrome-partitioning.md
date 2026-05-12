# 分割回文串
- **英文**: Palindrome Partitioning
- **难度**: 中等
- **题目**: https://leetcode.cn/problems/palindrome-partitioning/

## 题目描述
给你一个字符串 `s`，请你将* *`s`* *分割成一些 子串，使每个子串都是 **回文串** 。返回 `s` 所有可能的分割方案。

 

**示例 1：**

**输入：**s = "aab"
**输出：**[["a","a","b"],["aa","b"]]

**示例 2：**

**输入：**s = "a"
**输出：**[["a"]]

 

**提示：**

	
- `1 <= s.length <= 16`
	
- `s` 仅由小写英文字母组成

## 代码模板
```python
class Solution:
    def partition(self, s: str) -> List[List[str]]:
        
```

## Python 解法
```python
class Solution:
    def partition(self, s: str) -> List[List[str]]:
        res = []
        def backtrack(start, path):
            if start == len(s): res.append(path[:]); return
            for end in range(start+1, len(s)+1):
                sub = s[start:end]
                if sub == sub[::-1]:
                    path.append(sub); backtrack(end, path); path.pop()
        backtrack(0, [])
        return res
```
