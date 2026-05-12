# 轮转数组
- **英文**: Rotate Array
- **难度**: 中等
- **题目**: https://leetcode.cn/problems/rotate-array/

## 题目描述
给定一个整数数组 `nums`，将数组中的元素向右轮转 `k`* *个位置，其中 `k`* *是非负数。

 

**示例 1:**

**输入:** nums = [1,2,3,4,5,6,7], k = 3
**输出:** `[5,6,7,1,2,3,4]`
**解释:**
向右轮转 1 步: `[7,1,2,3,4,5,6]`
向右轮转 2 步: `[6,7,1,2,3,4,5]
`向右轮转 3 步: `[5,6,7,1,2,3,4]
```

**示例 2:**

**输入：**nums = [-1,-100,3,99], k = 2
**输出：**[3,99,-1,-100]
**解释:** 
向右轮转 1 步: [99,-1,-100,3]
向右轮转 2 步: [3,99,-1,-100]

 

**提示：**

	
- `1 5`
	
- `-231 31 - 1`
	
- `0 5`

 

**进阶：**

	
- 尽可能想出更多的解决方案，至少有 **三种** 不同的方法可以解决这个问题。
	
- 你可以使用空间复杂度为 `O(1)` 的 **原地 **算法解决这个问题吗？

## 代码模板
```python
class Solution:
    def rotate(self, nums: List[int], k: int) -> None:
        """
        Do not return anything, modify nums in-place instead.
        """
        
```

## Python 解法
```python
class Solution:
    def rotate(self, nums: List[int], k: int) -> None:
        k %= len(nums)
        nums[:] = nums[-k:] + nums[:-k]
```
