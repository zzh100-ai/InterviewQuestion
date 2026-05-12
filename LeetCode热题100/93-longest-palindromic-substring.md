# 最长回文子串
- **英文**: Longest Palindromic Substring
- **难度**: 中等
- **题目**: https://leetcode.cn/problems/longest-palindromic-substring/

## 题目描述
给你一个字符串 `s`，找到 `s` 中最长的 回文 子串。

 

**示例 1：**

**输入：**s = "babad"
**输出：**"bab"
**解释：**"aba" 同样是符合题意的答案。

**示例 2：**

**输入：**s = "cbbd"
**输出：**"bb"

 

**提示：**

	
- `1 <= s.length <= 1000`
	
- `s` 仅由数字和英文字母组成

## 代码模板
```python
class Solution:
    def longestPalindrome(self, s: str) -> str:
        
```

## Python 解法
```python
class Solution:
    def longestPalindrome(self, s: str) -> str:
        res = ''
        for i in range(len(s)):
            for l, r in [(i, i), (i, i+1)]:
                while l >= 0 and r < len(s) and s[l] == s[r]:
                    l -= 1; r += 1
                if r - l - 1 > len(res):
                    res = s[l+1:r]
        return res
```
