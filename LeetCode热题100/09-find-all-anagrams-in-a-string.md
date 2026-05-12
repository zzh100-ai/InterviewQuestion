# 找到字符串中所有字母异位词
- **英文**: Find All Anagrams in a String
- **难度**: 中等
- **题目**: https://leetcode.cn/problems/find-all-anagrams-in-a-string/

## 题目描述
给定两个字符串 `s` 和 `p`，找到 `s`** **中所有 `p`** **的 **异位词 **的子串，返回这些子串的起始索引。不考虑答案输出的顺序。

 

**示例 1:**

**输入: **s = "cbaebabacd", p = "abc"
**输出: **[0,6]
**解释:**
起始索引等于 0 的子串是 "cba", 它是 "abc" 的异位词。
起始索引等于 6 的子串是 "bac", 它是 "abc" 的异位词。

** 示例 2:**

**输入: **s = "abab", p = "ab"
**输出: **[0,1,2]
**解释:**
起始索引等于 0 的子串是 "ab", 它是 "ab" 的异位词。
起始索引等于 1 的子串是 "ba", 它是 "ab" 的异位词。
起始索引等于 2 的子串是 "ab", 它是 "ab" 的异位词。

 

**提示:**

	
- `1 4`
	
- `s` 和 `p` 仅包含小写字母

## 代码模板
```python
class Solution:
    def findAnagrams(self, s: str, p: str) -> List[int]:
        
```

## Python 解法
```python
class Solution:
    def findAnagrams(self, s: str, p: str) -> List[int]:
        from collections import Counter
        res, p_cnt, win = [], Counter(p), Counter()
        for i, ch in enumerate(s):
            win[ch] += 1
            if i >= len(p):
                left = s[i - len(p)]
                win[left] -= 1
                if win[left] == 0: del win[left]
            if win == p_cnt: res.append(i - len(p) + 1)
        return res
```
