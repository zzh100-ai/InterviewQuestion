# 柱状图中最大的矩形
- **英文**: Largest Rectangle in Histogram
- **难度**: 困难
- **题目**: https://leetcode.cn/problems/largest-rectangle-in-histogram/

## 题目描述
给定 *n* 个非负整数，用来表示柱状图中各个柱子的高度。每个柱子彼此相邻，且宽度为 1 。

求在该柱状图中，能够勾勒出来的矩形的最大面积。

 

**示例 1:**

**输入：**heights = [2,1,5,6,2,3]
**输出：**10
**解释：**最大的矩形为图中红色区域，面积为 10

**示例 2：**

**输入：** heights = [2,4]
输出： 4

 

**提示：**

	
- `1 5`
	
- `0 4`

## 代码模板
```python
class Solution:
    def largestRectangleArea(self, heights: List[int]) -> int:
        
```

## Python 解法
```python
class Solution:
    def largestRectangleArea(self, heights: List[int]) -> int:
        stack, res = [], 0
        heights.append(0)
        for i, h in enumerate(heights):
            while stack and h < heights[stack[-1]]:
                height = heights[stack.pop()]
                width = i if not stack else i - stack[-1] - 1
                res = max(res, height * width)
            stack.append(i)
        return res
```
