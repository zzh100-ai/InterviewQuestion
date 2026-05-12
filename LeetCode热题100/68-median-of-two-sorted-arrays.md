# 寻找两个正序数组的中位数
- **英文**: Median of Two Sorted Arrays
- **难度**: 困难
- **题目**: https://leetcode.cn/problems/median-of-two-sorted-arrays/

## 题目描述
给定两个大小分别为 `m` 和 `n` 的正序（从小到大）数组 `nums1` 和 `nums2`。请你找出并返回这两个正序数组的 **中位数** 。

算法的时间复杂度应该为 `O(log (m+n))` 。

 

**示例 1：**

**输入：**nums1 = [1,3], nums2 = [2]
**输出：**2.00000
**解释：**合并数组 = [1,2,3] ，中位数 2

**示例 2：**

**输入：**nums1 = [1,2], nums2 = [3,4]
**输出：**2.50000
**解释：**合并数组 = [1,2,3,4] ，中位数 (2 + 3) / 2 = 2.5

 

 

**提示：**

	
- `nums1.length == m`
	
- `nums2.length == n`
	
- `0 6 6`

## 代码模板
```python
class Solution:
    def findMedianSortedArrays(self, nums1: List[int], nums2: List[int]) -> float:
        
```

## Python 解法
```python
class Solution:
    def findMedianSortedArrays(self, nums1: List[int], nums2: List[int]) -> float:
        if len(nums1) > len(nums2): nums1, nums2 = nums2, nums1
        m, n = len(nums1), len(nums2)
        l, r = 0, m
        while l <= r:
            i = l + (r - l) // 2; j = (m + n + 1) // 2 - i
            aL = nums1[i-1] if i > 0 else float('-inf')
            aR = nums1[i] if i < m else float('inf')
            bL = nums2[j-1] if j > 0 else float('-inf')
            bR = nums2[j] if j < n else float('inf')
            if aL <= bR and bL <= aR:
                if (m + n) % 2: return max(aL, bL)
                return (max(aL, bL) + min(aR, bR)) / 2
            elif aL > bR: r = i - 1
            else: l = i + 1
```
