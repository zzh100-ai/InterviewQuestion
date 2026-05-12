# 和为 K 的子数组
- **英文**: Subarray Sum Equals K
- **难度**: 中等
- **题目**: https://leetcode.cn/problems/subarray-sum-equals-k/

## 题目描述
给你一个整数数组 `nums` 和一个整数 `k` ，请你统计并返回 *该数组中和为 `k`** **的子数组的个数 *。

子数组是数组中元素的连续非空序列。

 

**示例 1：**

**输入：**nums = [1,1,1], k = 2
**输出：**2

**示例 2：**

**输入：**nums = [1,2,3], k = 3
**输出：**2

 

**提示：**

	
- `1 4`
	
- `-1000 7 7`

## 代码模板
```python
class Solution:
    def subarraySum(self, nums: List[int], k: int) -> int:
        
```

## Python 解法
```python
class Solution:
    def subarraySum(self, nums: List[int], k: int) -> int:
        prefix = {0: 1}
        cur = res = 0
        for n in nums:
            cur += n
            res += prefix.get(cur - k, 0)
            prefix[cur] = prefix.get(cur, 0) + 1
        return res
```
