# 分割等和子集
- **英文**: Partition Equal Subset Sum
- **难度**: 中等
- **题目**: https://leetcode.cn/problems/partition-equal-subset-sum/

## 题目描述
给你一个 **只包含正整数 **的 **非空 **数组 `nums` 。请你判断是否可以将这个数组分割成两个子集，使得两个子集的元素和相等。

 

**示例 1：**

**输入：**nums = [1,5,11,5]
**输出：**true
**解释：**数组可以分割成 [1, 5, 5] 和 [11] 。

**示例 2：**

**输入：**nums = [1,2,3,5]
**输出：**false
**解释：**数组不能分割成两个元素和相等的子集。

 

**提示：**

	
- `1 <= nums.length <= 200`
	
- `1 <= nums[i] <= 100`

## 代码模板
```python
class Solution:
    def canPartition(self, nums: List[int]) -> bool:
        
```

## Python 解法
```python
class Solution:
    def canPartition(self, nums: List[int]) -> bool:
        total = sum(nums)
        if total % 2: return False
        target = total // 2
        dp = [False] * (target + 1); dp[0] = True
        for n in nums:
            for i in range(target, n - 1, -1):
                dp[i] = dp[i] or dp[i - n]
        return dp[target]
```
