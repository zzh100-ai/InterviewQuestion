# 电话号码的字母组合
- **英文**: Letter Combinations of a Phone Number
- **难度**: 中等
- **题目**: https://leetcode.cn/problems/letter-combinations-of-a-phone-number/

## 题目描述
给定一个仅包含数字 `2-9` 的字符串，返回所有它能表示的字母组合。答案可以按 **任意顺序** 返回。

给出数字到字母的映射如下（与电话按键相同）。注意 1 不对应任何字母。

 

**示例 1：**

**输入：**digits = "23"
**输出：**["ad","ae","af","bd","be","bf","cd","ce","cf"]

**示例 2：**

**输入：**digits = "2"
**输出：**["a","b","c"]

 

**提示：**

	
- `1 <= digits.length <= 4`
	
- `digits[i]` 是范围 `['2', '9']` 的一个数字。

## 代码模板
```python
class Solution:
    def letterCombinations(self, digits: str) -> List[str]:
        
```

## Python 解法
```python
class Solution:
    def letterCombinations(self, digits: str) -> List[str]:
        if not digits: return []
        phone = {'2':'abc','3':'def','4':'ghi','5':'jkl','6':'mno','7':'pqrs','8':'tuv','9':'wxyz'}
        res = []
        def backtrack(i, path):
            if i == len(digits): res.append(''.join(path)); return
            for ch in phone[digits[i]]: path.append(ch); backtrack(i+1, path); path.pop()
        backtrack(0, [])
        return res
```
