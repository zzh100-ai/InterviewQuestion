# 最小覆盖子串
- **英文**: Minimum Window Substring
- **难度**: 困难
- **题目**: https://leetcode.cn/problems/minimum-window-substring/

## 题目描述
给定两个字符串 `s` 和 `t`，长度分别是 `m` 和 `n`，返回 s 中的 **最短窗口 子串**，使得该子串包含 `t` 中的每一个字符（**包括重复字符**）。如果没有这样的子串，返回空字符串* *`""`。

测试用例保证答案唯一。

 

**示例 1：**

**输入：**s = "ADOBECODEBANC", t = "ABC"
**输出：**"BANC"
**解释：**最小覆盖子串 "BANC" 包含来自字符串 t 的 'A'、'B' 和 'C'。

**示例 2：**

**输入：**s = "a", t = "a"
**输出：**"a"
**解释：**整个字符串 s 是最小覆盖子串。

**示例 3:**

**输入:** s = "a", t = "aa"
**输出:** ""
**解释:** t 中两个字符 'a' 均应包含在 s 的子串中，
因此没有符合条件的子字符串，返回空字符串。

 

**提示：**

	
- `m == s.length`
	
- `n == t.length`
	
- `1 5`
	
- `s` 和 `t` 由英文字母组成

 

**进阶：**你能设计一个在 `O(m + n)` 时间内解决此问题的算法吗？

## 代码模板
```python
class Solution:
    def minWindow(self, s: str, t: str) -> str:
        
```

## Python 解法
```python
class Solution:
    def minWindow(self, s: str, t: str) -> str:
        from collections import Counter
        need = Counter(t)
        missing = len(t)
        l = start = end = 0
        for r, ch in enumerate(s):
            if need[ch] > 0: missing -= 1
            need[ch] -= 1
            if missing == 0:
                while need[s[l]] < 0:
                    need[s[l]] += 1; l += 1
                if end == 0 or r - l < end - start:
                    start, end = l, r + 1
                need[s[l]] += 1; missing += 1; l += 1
        return s[start:end]
```
