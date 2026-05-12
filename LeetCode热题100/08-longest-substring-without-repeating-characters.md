# 无重复字符的最长子串
- **英文**: Longest Substring Without Repeating Characters
- **难度**: 中等
- **题目**: https://leetcode.cn/problems/longest-substring-without-repeating-characters/

## 题目描述
给定一个字符串 `s` ，请你找出其中不含有重复字符的 **最长 子串**** **的长度。

 

**示例 1:**

**输入: **s = "abcabcbb"
**输出: **3 
**解释:** 因为无重复字符的最长子串是 `"abc"`，所以其长度为 3。注意 "bca" 和 "cab" 也是正确答案。

**示例 2:**

**输入: **s = "bbbbb"
**输出: **1
**解释: **因为无重复字符的最长子串是 `"b"`，所以其长度为 1。

**示例 3:**

**输入: **s = "pwwkew"
**输出: **3
**解释: **因为无重复字符的最长子串是 `"wke"`，所以其长度为 3。
     请注意，你的答案必须是 **子串 **的长度，`"pwke"` 是一个*子序列，*不是子串。

 

**提示：**

	
- `0 4`
	
- `s` 由英文字母、数字、符号和空格组成

## 代码模板
```python
class Solution:
    def lengthOfLongestSubstring(self, s: str) -> int:
        
```

## Python 解法
```python
class Solution:
    def lengthOfLongestSubstring(self, s: str) -> int:
        seen = {}
        l = res = 0
        for r, ch in enumerate(s):
            if ch in seen and seen[ch] >= l:
                l = seen[ch] + 1
            seen[ch] = r
            res = max(res, r - l + 1)
        return res
```
