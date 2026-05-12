# 移动零
- **英文**: Move Zeroes
- **难度**: 简单
- **题目**: https://leetcode.cn/problems/move-zeroes/

## 题目描述
给定一个数组 `nums`，编写一个函数将所有 `0` 移动到数组的末尾，同时保持非零元素的相对顺序。

**请注意** ，必须在不复制数组的情况下原地对数组进行操作。

 

**示例 1:**

**输入:** nums = `[0,1,0,3,12]`
**输出:** `[1,3,12,0,0]
```

**示例 2:**

**输入:** nums = `[0]`
**输出:** `[0]
```

 

**提示**:

	
- `1 4`
	
- `-231 31 - 1`

 

进阶：你能尽量减少完成的操作次数吗？

## 代码模板
```python
class Solution:
    def moveZeroes(self, nums: List[int]) -> None:
        """
        Do not return anything, modify nums in-place instead.
        """
        
```

## Python 解法
```python
class Solution:
    def moveZeroes(self, nums: List[int]) -> None:
        left = 0
        for right in range(len(nums)):
            if nums[right] != 0:
                nums[left], nums[right] = nums[right], nums[left]
                left += 1
```
