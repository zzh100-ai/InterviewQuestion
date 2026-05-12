# 最长连续序列
- **英文**: Longest Consecutive Sequence
- **难度**: 中等
- **题目**: https://leetcode.cn/problems/longest-consecutive-sequence/

## 题目描述
给定一个未排序的整数数组 `nums` ，找出数字连续的最长序列（不要求序列元素在原数组中连续）的长度。

请你设计并实现时间复杂度为 `O(n)`* *的算法解决此问题。

 

**示例 1：**

**输入：**nums = [100,4,200,1,3,2]
**输出：**4
**解释：**最长数字连续序列是 [1, 2, 3, 4]。它的长度为 4。

**示例 2：**

**输入：**nums = [0,3,7,2,5,8,4,6,0,1]
**输出：**9

**示例 3：**

**输入：**nums = [1,0,1,2]
输出：3

 

**提示：**

	
- `0 5`
	
- `-109 9`

## 代码模板
```python
class Solution:
    def longestConsecutive(self, nums: List[int]) -> int:
        
```

## Python 解法
```python
class Solution:
    def longestConsecutive(self, nums: List[int]) -> int:
        num_set = set(nums)
        longest = 0
        for n in num_set:
            if n - 1 not in num_set:
                cur, cur_len = n, 1
                while cur + 1 in num_set:
                    cur += 1; cur_len += 1
                longest = max(longest, cur_len)
        return longest
```
