# 搜索旋转排序数组
- **英文**: Search in Rotated Sorted Array
- **难度**: 中等
- **题目**: https://leetcode.cn/problems/search-in-rotated-sorted-array/

## 题目描述
整数数组 `nums` 按升序排列，数组中的值 **互不相同** 。

在传递给函数之前，`nums` 在预先未知的某个下标 `k`（`0 
**输入：**nums = [4,5,6,7,0,1,2], target = 0
**输出：**4

**示例 2：**

**输入：**nums = [4,5,6,7,0,1,2], target = 3
**输出：**-1

**示例 3：**

**输入：**nums = [1], target = 0
**输出：**-1

 

**提示：**

	
- `1 4 4`
	
- `nums` 中的每个值都 **独一无二**
	
- 题目数据保证 `nums` 在预先未知的某个下标上进行了旋转
	
- `-104 4`

## 代码模板
```python
class Solution:
    def search(self, nums: List[int], target: int) -> int:
        
```

## Python 解法
```python
class Solution:
    def search(self, nums: List[int], target: int) -> int:
        l, r = 0, len(nums) - 1
        while l <= r:
            mid = l + (r - l) // 2
            if nums[mid] == target: return mid
            if nums[l] <= nums[mid]:
                if nums[l] <= target < nums[mid]: r = mid - 1
                else: l = mid + 1
            else:
                if nums[mid] < target <= nums[r]: l = mid + 1
                else: r = mid - 1
        return -1
```
