# 颜色分类
- **英文**: Sort Colors
- **难度**: 中等
- **题目**: https://leetcode.cn/problems/sort-colors/

## 题目描述
给定一个包含红色、白色和蓝色、共 `n`* *个元素的数组 `nums` ，**原地 **对它们进行排序，使得相同颜色的元素相邻，并按照红色、白色、蓝色顺序排列。

我们使用整数 `0`、 `1` 和 `2` 分别表示红色、白色和蓝色。

必须在不使用库内置的 sort 函数的情况下解决这个问题。

 

**示例 1：**

**输入：**nums = [2,0,2,1,1,0]
**输出：**[0,0,1,1,2,2]

**示例 2：**

**输入：**nums = [2,0,1]
**输出：**[0,1,2]

 

**提示：**

	
- `n == nums.length`
	
- `1 <= n <= 300`
	
- `nums[i]` 为 `0`、`1` 或 `2`

 

**进阶：**

	
- 你能想出一个仅使用常数空间的一趟扫描算法吗？

## 代码模板
```python
class Solution:
    def sortColors(self, nums: List[int]) -> None:
        """
        Do not return anything, modify nums in-place instead.
        """
        
```

## Python 解法
```python
class Solution:
    def sortColors(self, nums: List[int]) -> None:
        p0, p2, i = 0, len(nums) - 1, 0
        while i <= p2:
            if nums[i] == 0:
                nums[i], nums[p0] = nums[p0], nums[i]; p0 += 1; i += 1
            elif nums[i] == 2:
                nums[i], nums[p2] = nums[p2], nums[i]; p2 -= 1
            else: i += 1
```
